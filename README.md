# SvelteKitTutorial Notes

- [SvelteKitTutorial Notes](#sveltekittutorial-notes)
  - [Sections](#sections)
  - [Notes](#notes)
    - [Part 1: Welcome to Svelte](#part-1-welcome-to-svelte)
      - [Introduction](#introduction)
      - [Reactivity](#reactivity)

## Sections

- Part 1: Welcome to Svelte

* [x] ~~_Introduction_~~ [2023-01-14]
* [ ] Reactivity
* [ ] Props
* [ ] Logic
* [ ] Events
* [ ] Bindings
* [ ] Lifecycle
* [ ] Stores

- Part 2: Introduction to SvelteKit

* [ ] Concepts
* [ ] Routing
* [ ] Loading data
* [ ] Forms
* [ ] API routes
* [ ] Errors and redirects
* [ ] Page options

- Part 3: Advanced Svelte

* [ ] Motion
* [ ] Transitions
* [ ] Animations
* [ ] Actions
* [ ] Advanced Bindings
* [ ] Classes
* [ ] Component composition
* [ ] Context API
* [ ] Special elements
* [ ] Module context
* [ ] Debugging
* [ ] Next steps

- Part 4: Advanced SvelteKit

* [ ] Hooks
* [ ] Stores
* [ ] Advanced routing
* [ ] Advanced loading
* [ ] Environment variables

## Notes

### Part 1: Welcome to Svelte

#### Introduction

- `Svelte` is a tool for building web apps via a declarative, component-based paradigm.
- A `component` is a self-contained block of code that encapsulates `HTML`, `CSS` and `JavaScript` that belong together.
- [[Shorthand attributes]] ... i.e. `<img src={src} />` becomes `<img {src} />`
- In Svelte, components are imported within a `<script>` tag

```jsx
<script>
  import Nested from './Nested.svelte'
</script>

<p>This is a paragraph.</p>
<Nested/>

<style>
	p {
		color: purple;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>
```

- Component names are always capitalized, to distinguish them from HTML elements.
- To enable parsing HTML strings use `<p>{@html string}</p>`

#### Reactivity
