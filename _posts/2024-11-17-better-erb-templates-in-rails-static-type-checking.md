---
title: "Better erb templates in Rails: static type checking"
excerpt_separator: "<!--more-->"
categories:
  - Software
tags:
  - Ruby
  - Ruby on Rails
  - sorbet
  - erb
  - Static Typing
---

A while ago I started ~complaining~ thinking about how the default erb views in Ruby on Rails could be improved.

Some context: at Cobot we run a Rails app that was started in 2009 and has about 1000 `.html.erb` files, including partials and [component views](https://viewcomponent.org/).

I had identified the following issues:

- The syntax is very complex due to the fact that any Ruby construct can be embedded.
  - This makes it harder to read/write views.
  - and also for [tools to format them](https://prettier.io/), or do auto-completion, syntax highlighting etc.
- Correctness of HTML (think missing closing tags) cannot be verified by tools.
- I rarely write tests for views (mostly only via [feature tests](http://teamcapybara.github.io/capybara/)) so confidence in their correctness (also in conjunction with their associated controller) is not very high - I'm also not really planning to get into it.

<!--more-->

I like that it's still HTML (and our designers agree), but I find the Ruby in there problematic.
In our frontend [Ember JS](https://emberjs.com/) apps we use [Glimmer](https://glimmerjs.github.io/glimmer-experimental/), which uses the same syntax as [Handlebars](https://handlebarsjs.com/playground.html):

{% raw %}

```hbs
{{#if myCondition}}
  <p>{{firstname}} {{loud lastname}}</p>
{{/if}}
```

{% endraw %}

I like the concept of having a limited syntax for control flow and callig into code on top of HTML.

Not too long ago we added [glint](https://typed-ember.gitbook.io/glint) to our Ember stack, which together with TypeScript gives us static type checking in our Glimmer templates. Very nice.

**And that gave me the idea that lead to this post:** we already use [Sorbet](https://sorbet.org/) for type checking our Ruby code. If we could have type checking for our Rails views as well, that would give us much more confidence - without having to write tests for them.

So far, I haven't figured out a way to do this with controllers since the interface between them and views is just too fuzzy. But for view components it was actually easy.

The following code makes a copy of each component's Ruby class, compiles the associated `.erb` file to Ruby and embeds it into the class as a method:

```ruby
require 'pathname'

class ComponentViewCompiler
  INITIALIZE_METHOD_REGEX = /def initialize.*?end/m
  COMPONENTS_PATH = Pathname.new('app/components')

  def initialize(target_dir)
    @target_dir = target_dir
    FileUtils.mkdir_p(@target_dir)
  end

  def compile_components
    COMPONENTS_PATH
      .glob('**/*.html.erb')
      .each do |view_file|
        class_file = view_file.sub_ext('').sub_ext('.rb')
        compile_component(view_file, class_file)
      end
  end

  private

  def compile_component(view_file, class_file)
    view = File.read(view_file)
    compiled_view = compile_view(view)
    code = File.read(class_file)
    call_method =
      "\n\nsig { void }\ndef call\noutput_buffer = ActionView::OutputBuffer.new\n#{
        compiled_view.gsub('@output_buffer', 'output_buffer')
      }\nend\n"
    index = code.match(INITIALIZE_METHOD_REGEX).end(0)
    code.insert(index, call_method) # yes this is ugly. will break with no or multiple initialize methods, e.g. nested classes. but it works.
    target_file =
      @target_dir.join(class_file.relative_path_from(COMPONENTS_PATH))
    FileUtils.mkdir_p(File.dirname(target_file))
    File.write(target_file, code)
  end

  def compile_view(view)
    handler = ActionView::Template.handler_for_extension('erb')
    handler.call(OpenStruct.new(type: 'erb'), view)
  end
end
```

Given a simple component:

_my_component.rb_:

```ruby
# typed: true

class MyComponent < ApplicationComponent
  extend T::Sig # add sorbet

  sig { params(name: String).void } # sorbet method signature
  def initialize(name)
    super
    @name = name
  end

  sig { returns(String) } # sorbet signature
  attr_reader :name
end
```

_my_component.html.erb_:

```erb
Hello <%= namne %>
```

This will be compiled to:

_compiled_components/my_component.rb_:

```ruby
# typed: true

class MyComponent < ApplicationComponent
  extend T::Sig # add sorbet

  sig { params(name: String).void } # sorbet method signature
  def initialize(name)
    super
    @name = name
  end

  sig { void }
  def call
    output_buffer = ActionView::OutputBuffer.new
    output_buffer.safe_append='hello'.freeze; output_buffer.append=( namne )
    output_buffer
  end

  sig { returns(String) } # sorbet signature
  attr_reader :name
end
```

The resulting file can be typechecked by sorbet (or [steep](https://github.com/soutaro/steep) if you prefer):

```sh
$ srb tc

compiled_components/my_component.rb:15: Method namne does not exist on MyComponent https://srb.help/7003
    15 |    output_buffer.safe_append='hello'.freeze; output_buffer.append=( namne )
                                                                             ^^^^^
  Did you mean name? Use -a to autocorrect
    compiled_components/my_component.rb:15: Replace with name
    15 |    output_buffer.safe_append='hello'.freeze; output_buffer.append=( namne )
                                                                             ^^^^^
    compiled_components/my_component.rb:20: Defined here
    20 |  attr_reader :name
          ^^^^^^^^^^^^^^^^^
```

And as you can see I made a typo in the template.

The downside here is that any type errors point to the compiled class, so nether the filename nor the line number in the error message are correct.

But it's a start. And to get around the part where this is not working for controllers, we just convert any complex and/or very important view into a component.

As a bonus, I added the component compiler to [Procfile.dev](https://railsnotes.xyz/blog/procfile-bin-dev-rails7), so now as long as `bin/rails` is running, the compiled components are kept up-to-date automatically.

_Procfile.dev_:

```yaml
compiled_component_views: npx -y watch "rails sorbet:compile_component_views" app/components
```

_sorbet.rake_:

```ruby
# typed: false

namespace :sorbet do
  desc 'Generate Ruby code from component views for Sorbet. This allows to type-check component views.'
  task :compile_component_views do
    require 'pathname'
    require 'fileutils'
    require_relative '../component_view_compiler'

    puts 'Compiling component views'
    dir = Pathname.new('compiled_components')
    FileUtils.rm_rf(dir)
    compiler = ComponentViewCompiler.new(dir)
    compiler.compile_components
  end
end

```

The same Rake task also runs on CI.

Happy static type checking!
