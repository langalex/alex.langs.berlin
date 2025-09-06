---
title: "Testing bindable properties on Svelte 5"
excerpt_separator: "<!--more-->"
categories:
  - Making Software
tags:
  - Svelte
  - Testing
  - JavaScript
---

I was recently working on a Svelte (5) component that was using a `$bindable` property. When it came to writing a test for it, I got stuck on how to pass bind said property to a variable in my test module.

Suppose the component looks like this:

<!--more-->

```html

<script>
  const { count = $bindable() } = props();
</script>

<button onclick={() => count = count + 1}>Click</button>
```

From another component you would call it like this:

```html
<script>

  let myCounter = $state(0):

</script>

<CountButton bind:count={myCounter}/>

<p>Count is {myCounter}</p>
```

But from a test:

```js
it('updates the bound counter', async () => {
			let boundCounter = 0;

			const { container } = render(CountButton, {
				props: {
					counter: boundCounter // this is not bound and you can’t use bind:counter here
				}
			});

			const button = container.querySelector('button'):
			button.dispatchEvent(new Event('click'));

		      expect(boundCounter).toBe(1); // nope, still 0
		});

```

There is a very simple solution to this problem. I could not find it in the docs or on the Internet, neither could any AI help me out.

What I ended up doing is checking out the compiled code of the parent component above. If you have installed Svelte plugins in for example VS Code, there is a button in the top right corner to see it.

And this is what bound properties get compiled to:

```js
it('updates the bound counter', async () => {
			let boundCounter = 0;

			const { container } = render(CountButton, {
				props: {
					// simulate bind:sharedScrollPosition
					get count() {
						return boundCounter;
					},
					set counter(value) {
						boundCounter = value;
					}
				}
			});

			const button = container.querySelector('button'):
			button.dispatchEvent(new Event('click'));

		      expect(boundCounter).toBe(1);
		});
```

A getter and a setter! Of course!

And this is how you can test bound properties. And any other “magic” code.