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
    - [Part 2: Introduction to SvelteKit](#part-2-introduction-to-sveltekit)
      - [Concepts](#concepts)
      - [Routing](#routing)
      - [Loading data](#loading-data)
      - [Forms](#forms)
      - [API Routes](#api-routes)
      - [Errors and Redirects](#errors-and-redirects)
      - [Page options](#page-options)

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

* [x] ~~_Concepts_~~ [2023-01-16]
* [x] ~~_Routing_~~ [2023-01-16]
* [x] ~~_Loading data_~~ [2023-01-16]
* [x] ~~_Forms_~~ [2023-01-17]
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

### Part 2: Introduction to SvelteKit

#### Concepts

- SvelteKit is an `app framework` (i.e. a meta-framework)
- It comes with built-in solutions for:
  - Routing
  - Server-side rendering (SSR)
  - Data fetching
  - Service workers
  - TypeScript integration
  - Singe-page apps (SPAs)
  - Library packaging
  - Optimized production builds
  - Deployment solutions, etc.
- SvelteKit apps are server-rendered by default
- _A SvelteKit app can be thought of as two distinct entities working in tandem: the server and the client_
  - The Server's basic job is to turn a a request into a response
  - Client refers to JavaScript that loads in the browser

#### Routing

- SvelteKit uses file-based routing
  - Routes are created under `src/routes` with the `+oage.svelte` naming convention
- `+layout.svelte` naming convention can be used to share common ui among child/sibling routes
- `Dynamic routes` are achieved via the `[paramVar]` directory naming convention

#### Loading data

- To fetch data (or execute any server-only code prior to loading an adjacent `+page.svelte`) - define a `+page.server.js` file
  - The code in `+page.server.js` will only run on the server
  - A special `load` function can be declared whose return value will be passed to the adjacent page route as a `data` prop
    - `export let data`
  - The `load` function associated with a dynamic page route will be passed a `params` parameter to determine the specific route param that was hit (i.e. `export function load( { params } ) { ... }`
- To share `load` data among children routes use `layout.server.js` (watch out for name conflicts)

#### Forms

- Client can send data to the server via the `form` element (using `request.formData()` on the server and exporting an `actions`)

```jsx
export const actions = {
  default: async ({ cookies, request }) => {
    const data = await request.formData()
    db.createTodo(cookies.get('userid'), data.get('description'))
  },
}
```

- NOTE: _Default actions cannot coexist with named actions._
- `form` element has an optional `action` attribute (e.g `action="?/create"`)
- Hidden form input example

```jsx
<form method='POST' action='?/delete'>
  <input type='hidden' name='id' value={todo.id} />
  <button aria-label='Mark as complete'>✔</button>

  {todo.description}
</form>
```

- `fail` function can be used to return error feedback to user and display on page that sent the failed request
  - Unhandled errors are automatically hidden by SvelteKit from the user
- `form` props will contain the `fail` return content and is only ever populated after a form failure
  - `export let form` to gain access to form prop in component
- `	import { enhance } from '$app/forms';` and `<form use:enhance ... />` to `progressively enhance the native form element
  - Progressive enhancement of the native form element enables:
    - update of the `form` prop
    - invalidate all data on successful response (e.g. re-run `load` function on server)
    - navigate to new page on redirect response
    - renders nearest error page on failure
    - updates page instead of reload on submission (allowing for animations between states)
- `use:enhance` also allows for `pending states` and `optimistic UI`

- Enhanced creation

```jsx
<form
	method="POST"
	action="?/create"
	use:enhance={() => {
		creating = true;

		return async ({ update }) => {
			await update();
			creating = false;
		};
	}}
>
```

- Optimistic deletion

```jsx
	{#each data.todos.filter((todo) => !deleting.includes(todo.id)) as todo (todo.id)}
		<li class="todo" in:fly={{ y: 20 }} out:slide>
			<form
				method="POST"
				action="?/delete"
				use:enhance={() => {
					deleting = [...deleting, todo.id];
					return async ({ update }) => {
						await update();
						deleting = deleting.filter((id) => id !== todo.id);
					};
				}}
			>
				<input type="hidden" name="id" value={todo.id} />
				<button aria-label="Mark as complete">✔</button>

				{todo.description}
			</form>
		</li>
	{/each}
```

- `use:enhance` is very customizable
  - you can cancel() submissions
  - handle redirects
  - control whether the form is reset

#### API Routes

#### Errors and Redirects

#### Page options

```

```
