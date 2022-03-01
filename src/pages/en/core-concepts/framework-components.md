---
layout: ~/layouts/MainLayout.astro
title: Framework Components
description: Learn how to use React, Svelte, etc.
---

- TODO: Talk about what frameworks we support, including links to them (React, Preact, Svelte, Vue, Lit, Solid.js)
- TODO: Link to the custom renderer docs

## Hydrate Interactive Components

A framework component can be hydrated using a `client:*` directive. This is a component attribute to define how your component should be rendered and hydrated. It describes whether your component should be rendered at build-time, and when your component's JavaScript should be loaded by the browser, client-side.

Most directives will **render** the component on the server at build time, and **hydrate** the component on the client at run time according to the specific directive.

```astro
---
// Example: hydrating framework components in the browser.
import MyReactComponent from '../components/MyReactComponent.jsx';
import MySvelteComponent from '../components/MySvelteComponent.svelte';
---
<!-- This component's JS will begin importing when the page loads -->
<MySvelteComponent client: load />

<!-- This component's JS will not be sent to the client until 
the user scrolls down and the component is visible on the page -->
<MyReactComponent client:visible />
```

>⚠️ Any renderer JS necessary for the component's framework (e.g. React, Svelte) is downloaded with the page. The `client:*` directives only dictate when the component JS is imported and when the component is hydrated.

### `<MyComponent client:load />`

Start importing the component JS at page load. Hydrate the component when import completes.

### `<MyComponent client:idle />`

Start importing the component JS as soon as main thread is free (uses [requestIdleCallback()][mdn-ric]). Hydrate the component when import completes.

### `<MyComponent client:visible />`

Start importing the component JS as soon as the element enters the viewport (uses [IntersectionObserver][mdn-io]). Hydrate the component when import completes.

💡 *Useful for content lower down on the page.*

### `<MyComponent client:media={QUERY} />`

Start importing the component JS as soon as the browser matches the given media query (uses [matchMedia][mdn-mm]). Hydrate the component when import completes. 

💡 *Useful for sidebar toggles, or other elements that should only display on mobile or desktop devices.*

### `<MyComponent client:only=" " />`

Start importing the component JS at page load and hydrate when the import completes, similar to `client:load`.

 >⚠️ The component will be **skipped** at build time, and should specify which renderer to use from the array in your [`astro.config.mjs` configuration](/en/reference/configuration-reference))
 >
 > e.g. `<client:only="react" />` or `<client:only="my-custom-renderer" />`
 
 💡 *Useful for components that are entirely dependent on client-side APIs.* 


## Interactivity in Astro Components

[Astro components](/en/core-concepts/astro-components) (`.astro` files) are HTML-only templating components with no client-side runtime. 

### Can I Hydrate Astro Components?

No! If you try to hydrate an Astro component with a `client:` modifier, you will get an error.

### `<script>`

You can add a `<script>` tag to your Astro component HTML template and send JavaScript to the browser that executes in the global scope.

```astro
---
// Example: Using Astro with script tags
---
<h1>Not clicked</h1>
<button>Click to change heading</button>
<script>
document.querySelector("button").addEventListener("click",() => {
    document.querySelector("h1").innerText = "clicked"
})
</script>
```
### XElement

The community project [XElement](https://www.npmjs.com/package/astro-xelement) allows you to create dynamic HTML elements with interactivity (e.g. animations, transitions, event listeners) natively in an Astro components.

## TODO

- Talk about using Preact + React + Solid together in the same project (Fred: this is a bit technically tricky, I can write this section if you leave it for me)
- anything else? I can't think of anything!


[mdn-io]: https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
[mdn-ric]: https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback
[mdn-mm]: https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia
