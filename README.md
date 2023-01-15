# SvelteKitTutorial Notes

- [SvelteKitTutorial Notes](#sveltekittutorial-notes)
  - [Sections](#sections)
  - [Notes](#notes)
    - [Part 1: Welcome to Svelte](#part-1-welcome-to-svelte)
      - [Introduction](#introduction)
      - [Reactivity](#reactivity)
      - [Props](#props)
      - [Logic](#logic)
      - [Events](#events)
      - [Bindings](#bindings)
      - [Lifecycle](#lifecycle)
      - [Stores](#stores)

## Sections

- Part 1: Welcome to Svelte

* [x] ~~_Introduction_~~ [2023-01-14]
* [x] ~~_Reactivity_~~ [2023-01-14]
* [x] ~~_Props_~~ [2023-01-14]
* [x] ~~_Logic_~~ [2023-01-14]
* [x] ~~_Events_~~ [2023-01-14]
* [x] ~~_Bindings_~~ [2023-01-15]
* [x] ~~_Lifecycle_~~ [2023-01-15]
* [x] ~~_Stores_~~ [2023-01-16]

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

- `Reactivity` is how Svelte keeps the DOM in sync with application state

```jsx
<script>
	let count = 0;

	function increment() {
    count += 1
	}
</script>

<button on:click={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>
```

- `reactive declarations` , `$: [expression or value]` are Svelte's version of computed properties. Multiple statements can be encased in a block-scope

```jsx
<script>
	let count = 0;

  $: doubled = count * 2;

	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>
```

- Note: `Reactivity` is triggered via `assignments` (i.e. mutating methods won't trigger DOM updates)

#### Props

- `Props` are declared in components by via the `export` keyword in the `<script>`. An optional defaultValue can also be passed to exported props

```jsx
<script>
export let answer = [defaultValue];
<script>
```

- `Prop spreading` is possible via <Component {...object} /> syntax
- Within the template portion of a Svelte component,`$$props` can be referenced to access all props that were passed to a component
  - _Avoid do to poor optimization_

#### Logic

- `If/else if/else` block syntax:

```jsx
{#if [expression]}
// if conditional html
{:if else [expression]}
// else if conditional html
{:else}
// else conditional html
{/if}
```

- `Each` blocks:

```jsx
<ul>
{#each cats as cat, index}
<li key={cat.id}>#{index + 1} - {cat.name}</li>
{/each}
</ul>
```

- Of course, it's possible for a list to change order then we must use a `Keyed each` block:

```jsx
<ul>
{#each cats as cat, index (cat.id)}
<li key={cat.id}>#{index + 1} - {cat.name}</li>
{/each}
</ul>
```

- `Await` blocks

```jsx
{#await promise}
// render something while promise is resolved
{:then response}
// do something with response
{:catch error}
// do something with error
{/await}
```

- No loading state? No error handling? Then a shorthand version of the above Await block syntax is possible:

```jsx
{#await promise then response}
// do something with response
{/await}
```

#### Events

- `Event handlers` can be declared on elements by the `on:` directive, (e.g. `<div on:click={handleClick} />`)
- `Inline handlers` are cool and performant (e.g. `<input on:click={e => {name = event.target.value}} />`)
  - wrapping inline event handlers in quotation marks might improve syntax highlighting
- `Event modifiers`
  - `preventDefault`
  - `stopPropagation`
  - `passive`
  - `nonpassive`
  - `capture`
  - `once`
  - `self`
  - `trusted`
- Modifiers are chainable. `on:click|preventDefault|stopPropagation`
- `Component events`:

```jsx
<script>
  import {createEventDispatcher} from 'svelte' const dispatch = createEventDispatcher() const handleClick = () =>
  dispatch('eventName', event.detail )
</script>
```

- `Component events` _don't bubble_. They must be explicitly forwarded by intermediate components
  - The shorthand way to do this is by using `event directives` on intermediate components that have no defined value
  - `<input on:message />` - will forward `message` events to parent component
-

#### Bindings

- Svelte supports `two-way binding` via the `bind:[attribute]={variable}` directive. (e.g. `<input bind:value={name} />`)
- _In the DOM , everything is a string_
- For input `type="checkbox"`, the bind attribute is `checked` (e.g. `<input type="checkbox bind:checked={checked} />`)
- Radio/Checkbox groups make use of the `bind:group={optionsVariable}` directive
  ![](images/Screenshot%202023-01-15%20at%2010.09.12.png)
- All `bind:` directives have a shorthand version (i.e. `bind:[attribute]={variable}` can be shortened to `bind:attribute` if attributeName === variableName)
- Select elements use the `bind:value` directive
- A multi-select can be achieved via the `multiple` attribute

#### Lifecycle

- `onMount`: use this to run code after the component renders
  - Use this to fetch data on the client side
  - Does not run on SSR
  - If `onMount` returns a function it will run when the component is unMounted
- `onDestroy`: use this to run code after the component is destroyed
- `beforeUpdate` and `afterUpdate` allow for imperative component manipulation
- `tick` batch updates after all pending state updates resolve

#### Stores

- Svelte has ships with its own application state `store` module

```jsx
import { writable } from 'svelte/store'

export const count = writable(0)

---

count.subscribe(value => {
  component_local_var = value
})
```

- `writable` store has the following methods:
  - `.subscribe` lets a component tap into store changes
    - `subscribe` return an `unsubscribe` event that should be called within `onDestroy` lifecycle hook
  - `.set` to set the state value
  - `.update` to update the state value
- `store` values can be references (an auto-subscribed) via `$store` (dollar sign referencing)
- `readable` stores limit external writes

```jsx
import { readable } from 'svelte/store'

export const time = readable(new Date(), function start(set) {
  // implementation goes here
  const interval = setInterval(() => {
    set(new Date())
  }, 1000)

  return function stop() {
    clearInterval(interval)
  }
})
```

- Stores can be `derived` from other stores:

```jsx
import { readable, derived } from 'svelte/store'

export const time = readable(new Date(), function start(set) {
  const interval = setInterval(() => {
    set(new Date())
  }, 1000)

  return function stop() {
    clearInterval(interval)
  }
})

const start = new Date()

export const elapsed = derived(time, ($time) => {
  return ($time - start) / 1000
})
```

- `Custom stores` (resembles React useContext pattern)

```jsx
import { writable } from 'svelte/store'

function createCount() {
  const { subscribe, set, update } = writable(0)

  return {
    subscribe,
    increment: () => {},
    decrement: () => {},
    reset: () => {},
  }
}

export const count = createCount()
```

- `$count` has the value
- Two-way binding (`bind:`) is possible with `writable` stores
