source "https://rubygems.org"

gem 'bigdecimal'
gem 'csv'
gem 'ostruct'
gem "tzinfo-data"

# The github-pages gem pins liquid to 4.0.3, which is broken on Ruby >= 3.2
# (the taint APIs it calls were removed). Jekyll and the plugins used by this
# site are tracked explicitly instead, mirroring the GitHub Pages set.
gem "jekyll", ">= 3.9.0", "< 5.0"
gem "liquid", ">= 4.0.4" # 4.0.3 calls taint APIs removed in Ruby 3.2+
gem "kramdown-parser-gfm" # GitHub Pages builds kramdown with GFM input

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-remote-theme"
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-include-cache"
  gem "jekyll-redirect-from"
  gem "jekyll-seo-tag"
  gem "jekyll-github-metadata"
  gem "jekyll-algolia"
end
