---
title: 'VueJS #2 App Configurations'
date: 2024-04-16 04:00:21
toc: true
---

Every application instance exposes a `config` object that contains the configuration settings for that application. You can modify its properties before mounting your application.

## Application API
### CreateApp()
Creates an application instance.
- **Type**

```ts
function createApp(rootComponent: Component, rootProps?: object): App
```

- **Details**

The first argument is the root component. The second optional argument is the props to be passed to the root component.

- **Example**

With inline root component:

```js
import { createApp } from 'vue'

const app = createApp({
  /* root component options */
})
```

With imported component:

```js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
```

### createSSRApp()
​Creates an application instance in [SSR Hydration](https://vuejs.org/guide/scaling-up/ssr#client-hydration) mode. Usage is exactly the same as `createApp()`.

### app.mount()
​Mounts the application instance in a container element.

- **Type**

```ts
interface App {
  mount(rootContainer: Element | string): ComponentPublicInstance
}
```

- **Details**

The argument can either be an actual DOM element or a CSS selector (the first matched element will be used). Returns the root component instance.

If the component has a template or a render function defined, it will replace any existing DOM nodes inside the container. Otherwise, if the runtime compiler is available, the `innerHTML` of the container will be used as the template.

In SSR hydration mode, it will hydrate the existing DOM nodes inside the container. If there are [mismatches](https://vuejs.org/guide/scaling-up/ssr#hydration-mismatch), the existing DOM nodes will be morphed to match the expected output.

For each app instance, `mount()` can only be called once.

- **Example**

```js
import { createApp } from 'vue'
const app = createApp(/* ... */)

app.mount('#app')
```

Can also mount to an actual DOM element:

```js
app.mount(document.body.firstChild)
```

### app.unmount()
​Unmounts a mounted application instance, triggering the unmount lifecycle hooks for all components in the application's component tree.

- **Type**

```ts
interface App {
  unmount(): void
}
```

### app.component()
​Registers a global component if passing both a name string and a component definition, or retrieves an already registered one if only the name is passed.

- **Type**

```ts
interface App {
  component(name: string): Component | undefined
  component(name: string, component: Component): this
}
```

- **Example**

```ts
import { createApp } from 'vue'

const app = createApp({})

// register an options object
app.component('my-component', {
  /* ... */
})

// retrieve a registered component
const MyComponent = app.component('my-component')
```

- **See also** [Component Registration](https://vuejs.org/guide/components/registration)

### app.directive()
Registers a global custom directive if passing both a name string and a directive definition, or retrieves an already registered one if only the name is passed.

- **Type**

```ts
interface App {
  directive(name: string): Directive | undefined
  directive(name: string, directive: Directive): this
}
```

- **Example**

```js
import { createApp } from 'vue'
const app = createApp({
  /* ... */
})

// register (object directive)
app.directive('my-directive', {
  /* custom directive hooks */
})

// register (function directive shorthand)
app.directive('my-directive', () => {
  /* ... */
})

// retrieve a registered directive
const myDirective = app.directive('my-directive')
```

- **See also** [Custom Directives](https://vuejs.org/guide/reusability/custom-directives)

### app.use()
​Installs a [plugin](https://vuejs.org/guide/reusability/plugins).

- **Type**

```ts
interface App {
  use(plugin: Plugin, ...options: any[]): this
}
```

- **Details**

Expects the plugin as the first argument, and optional plugin options as the second argument.

The plugin can either be an object with an `install()` method, or just a function that will be used as the `install()` method. The options (second argument of `app.use()`) will be passed along to the plugin's `install()` method.

When `app.use()` is called on the same plugin multiple times, the plugin will be installed only once.

- **Example**

```js
import { createApp } from 'vue'
import MyPlugin from './plugins/MyPlugin'

const app = createApp({
  /* ... */
})

app.use(MyPlugin)
```

- **See also** [Plugins](https://vuejs.org/guide/reusability/plugins)

### app.mixin()
​Applies a global mixin (scoped to the application). A global mixin applies its included options to every component instance in the application.

> **_Not Recommended_**
>
> _Mixins are supported in Vue 3 mainly for backwards compatibility, due to their widespread use in ecosystem libraries. Use of mixins, especially global mixins, should be avoided in application code._
>
> _For logic reuse, prefer Composables instead._

- **Type**

```ts
interface App {
  mixin(mixin: ComponentOptions): this
}
```

### app.provide()
​Provide a value that can be injected in all descendant components within the application.

- **Type**

```ts
interface App {
  provide<T>(key: InjectionKey<T> | symbol | string, value: T): this
}
```

- **Details**

Expects the injection key as the first argument, and the provided value as the second. Returns the application instance itself.

- **Example**

```js
import { createApp } from 'vue'

const app = createApp(/* ... */)

app.provide('message', 'hello')
```

Inside a component in the application:

```js
export default {
  inject: ['message'],
  created() {
    console.log(this.message) // 'hello'
  }
}
```

- **See also**
    - [Provide / Inject](https://vuejs.org/guide/components/provide-inject)
    - [App-level Provide](https://vuejs.org/guide/components/provide-inject#app-level-provide)
    - [app.runWithContext()](https://vuejs.org/api/application.html#app-runwithcontext)