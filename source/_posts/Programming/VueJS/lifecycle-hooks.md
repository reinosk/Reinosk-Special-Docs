---
title: 'VueJS #6 Lifecycle Hooks'
date: 2024-04-16 20:00:21
toc: true
---

Each Vue component instance goes through a series of initialization steps when it's created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. Along the way, it also runs functions called lifecycle hooks, giving users the opportunity to add their code at specific stages.

## Registering Lifecycle Hooks
For example, the `mounted` hook can be used to run code after the component has finished the initial rendering and created the DOM nodes:​

```js
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```

There are also other hooks which will be called at different stages of the instance's lifecycle, with the most commonly used being `mounted`, `updated`, and `unmounted`.

All lifecycle hooks are called with their `this` context pointing to the current active instance invoking it. Note this means you should avoid using arrow functions when declaring lifecycle hooks, as you won't be able to access the component instance via `this` if you do so.

If our project uses the Options API, we don’t have to change any of the code for our Vue lifecycle hooks. This is because Vue 3 is designed to be compatible with prior releases of Vue.

However, the way we access these hooks is a little bit different when we decide to use the Composition API -  which is especially useful in larger Vue projects.

By the end of this article, hope you'll know how to use lifecycle hooks in both the Options API and Composition API and be on your way to writing better code.

## Lifecycle Diagram
Below is a diagram for the instance lifecycle. You don't need to fully understand everything going on right now, but as you learn and build more, it will be a useful reference.

<details>
<summary><b>Open The Diagram<b></summary>

![Lifecycle Diagram](https://vuejs.org/assets/lifecycle.MuZLBFAS.png)

</details>

Essentially, each main Vue lifecycle event is separated into two hooks that are called right before that event and then right after. There are four main events (8 main hooks) that you can utilize in your Vue app.

- **Creation** - runs on your component's creation
- **Mounting** - runs when the DOM is mounted.
- **Updates** - runs when reactive data is modified.
- **Destruction** - runs right before your element is destroyed.

## Using our Vue Lifecycle Hooks in the Options API

With the Options API, our lifecycle hooks are exposed as options on our Vue instance. We don't need to import anything, just invoke the method and write the code for that.

For example, let's say we wanted to access our `mounted()` and our `update()` lifecycle hooks. It might look something like this.

```vue
<script>
export default {
  mounted() {
    console.log('mounted!')
  },
  updated() {
    console.log('updated!')
  },
}
</script>
```

See details [Options API Lifecycle Hooks](https://vuejs.org/api/options-lifecycle.html).

## Using our Vue Lifecycle Hooks in the Vue Composition API
> **_Usage Note_**
>
> _All APIs listed on this page must be called synchronously during the `setup()` phase of a component. See [Guide - Lifecycle Hooks](https://vuejs.org/guide/essentials/lifecycle.html) for more details._

In the Composition API, we've to import lifecycle hooks into our project before we can use them. This is to help keep projects as lightweight as possible.

```js
import { onMounted } from 'vue'
```

Excluding `beforeCreate()` and `created()` (which are replaced by the `setup()` method itself), there are 12 of the Options API lifecycle host that we can access in our setup method.

See details [Composition API Lifecycle Hooks](https://vuejs.org/api/composition-api-lifecycle.html).

When we import them and access them in our code, it would look like this.

```vue
<script>
import { onMounted } from 'vue'

export default {
  setup() {
    onMounted(() => {
      console.log('mounted in the composition api!')
    })
  },
}
</script>
```

## An In-Depth Look at Each Lifecycle Hook
We now understand two important things:

- The different lifecycle hooks we can use
- How to use them in both the Options API and the Composition API

Let’s take a deeper dive at each lifecycle hook and look at how they’re used, what kind of code we can write in each one, and the differences between them in the Options API and Composition API.

## Creation Hooks - The Start of the VueJS Lifecycle
Creation hooks are they very first thing that runs in program.

### `beforeCreate()` - Options API
Since the craeted hook is the thing that initializes all of the reactive data and events, `beforeCreate()` doesn't have access to any of a component's reactive data and events.

```vue
<script>
export default {
  data() {
    return {
      val: "Hello"
    }
  },
  beforeCreate() {
    console.log(`Value of val is: ${this.val}`)
  }
}
</script>

<template></template>
```

The output value of `val` is `undefined` 'cause data hasn't been initialized yet. You also can't call your component methods in this method either:

![beforeCreate()](https://i.imgur.com/9Mf222Q.png)

If you wanna see a full list of what is available, I'd recommend just running `console.log(this)` to see what has been initialized. This is useful in every hook too using the Options API.

![Full List of What is Available](https://i.imgur.com/K7Cnypd.png)

### `created()` - Options API
We now have access to the component's data and events. So modifying the example from above to use `created()` instead `beforeCreate()` we see how the output changes.

```vue
<script>
export default {
  data() {
    return {
      val: "Hello"
    }
  },
  created() {
    console.log(`Value of val is: ${this.val}`)
  }
}
</script>

<template></template>
```

![created()](https://i.imgur.com/cdV98eQ.png)

The output of this would be `Value of val is: hello` because we have initialized our data.

Using the created method is useful when dealing with reading/writing the reactive data. For example, if you want to make an API call and then store that value, this is the place to do it.

It’s better to do that here than in mounted because it happens earlier in Vue’s synchronous initialization process and you perform data reading/writing all you want.

### What about the Composition API Creation Hooks?
For the Vue 3 Lifecycle Hooks using the Composition API, both `beforeCreate()` and `created()` are replaced by the `setup()` method. This means that any code you would have put inside either of these methods is now just inside your setup method.

```vue
<script>
import { ref } from 'vue'
export default {
  setup() {
    const val = ref('hello this is composition API')
    console.log('Value of val is: ' + val.value)
    return {
      val,
    }
  },
}
</script>

<template></template>
```
![Composition API Creation Hooks](https://i.imgur.com/2xveaxk.png)

## Mounting Hooks – Accessing the DOM
These mounting hooks handle mounting and rendering the component. These are some of the most commonly used hooks in projects and applications.

### `beforeMount()` and `onBeforeMount()`

Registers a hook to be called right before the component is to be mounted.

<details>
<summary><b>Options API <code>beforeMount()</code><b></summary>

- **Type**

```ts
interface ComponentOptions {
  beforeMount?(this: ComponentPublicInstance): void
}
```

- **Details**

When this hook is called, the component has finished setting up its reactive state, but no DOM nodes have been created yet. It is about to execute its DOM render effect for the first time.

> **_This hook is not called during server-side rendering._**

- **Example**

```js
export default {
  beforeMount() {
    console.log(this.$el)
  },
}
```

![beforeMount()](https://i.imgur.com/byIaTbY.png)

</details>

<details>
<summary><b>Composition API <code>onBeforeMount()</code><b></summary>

- **Type**

```ts
function onBeforeMount(callback: () => void): void
```

- **Details**

When this hook is called, the component has finished setting up its reactive state, but no DOM nodes have been created yet. It is about to execute its DOM render effect for the first time.

> **_This hook is not called during server-side rendering._**

- **Example**

```vue
<template>
  <div ref="root">Hello World</div>
</template>

<script>
import { ref, onBeforeMount } from 'vue'

export default {
  setup() {
    const root = ref(null)
    onBeforeMount(() => {
      console.log(root.value)
    })
    return {
      root,
    }
  },
  beforeMount() {
    console.log(this.$el)
  },
}
</script>
```

![onBeforeMount()](https://i.imgur.com/AQ8JcTL.png)
</details>

Then, the corresponding script to try and access the ref.

Since, `app.$el` isn't yet created, the output will be undefined.

While it's preferred that you use `created()`/`setup()` to perform your API calls, this is really the last step you should call them before it's unnecessary late in the process because it’s right after created — they have access to the same component variables.

### `mounted()` and `onMounted()`

Register a callback to be called after the component has been mounted

<details>
<summary><b>Options API <code>mounted()</code><b></summary>

- **Type**

```ts
interface ComponentOptions {
  mounted?(this: ComponentPublicInstance): void
}
```

- **Details**

A component is considered mounted after:

- All of its synchronous child components have been mounted (does not include async components or components inside `<Suspense>` trees).
- Its own DOM tree has been created and inserted into the parent container. Note it only guarantees that the component's DOM tree is in-document if the application's root container is also in-document.

This hook is typically used for performing side effects that need access to the component's rendered DOM, or for limiting DOM-related code to the client in a [server-rendered application](https://vuejs.org/guide/scaling-up/ssr).

> **_This hook is not called during server-side rendering._**

- **Example**

```js
export default {
  mounted() {
    console.log(this.$el)
  },
}
```

![mounted()](https://i.imgur.com/zvAyyzr.png)


</details>

<details>
<summary><b>Composition API <code>onMounted()</code><b></summary>

- **Details**

A component is considered mounted after:

- All of its synchronous child components have been mounted (does not include async components or components inside `<Suspense>` trees).
- Its own DOM tree has been created and inserted into the parent container. Note it only guarantees that the component's DOM tree is in-document if the application's root container is also in-document.
- This hook is typically used for performing side effects that need access to the component's rendered DOM, or for limiting DOM-related code to the client in a [server-rendered application](https://vuejs.org/guide/scaling-up/ssr.html).

> **_This hook is not called during server-side rendering._**

- **Example**

```vue
<script>
import { ref, onMounted } from 'vue'

export default {
  setup() {
    /* Composition API */

    const root = ref(null)

    onMounted(() => {
      console.log(root.value)
    })

    return {
      root,
    }
  },
}
</script>

<template></template>
```

![onMounted()](https://i.imgur.com/czTBiN8.png)
</details>

## Update Hooks – Reactivity in the VueJS Lifecycle

The updated lifecycle event is triggered whenever reactive data is modified, triggering a render update.

### `beforeUpdate()` and `onBeforeUpdate()`

Register a hook to be called right before the component is about to update it's DOM tree due to a reactive state change.

<details>
<summary><b>Options API <code>beforeUpdate()</code><b></summary>

- **Type**

```ts
interface ComponentOptions {
  beforeUpdate?(this: ComponentPublicInstance): void
}
```

- **Details**

This hook can be used to access the DOM state before Vue updates the DOM. It is also safe to modify component state inside this hook.

> **_This hook is not called during server-side rendering._**

- **Example**


```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="updateMessage">Update Message</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, World!'
    }
  },
  methods: {
    updateMessage() {
      this.message = 'Message Updated!'
    }
  },
  beforeUpdate() {
    console.log('Component will update!')
  }
}
</script>
```

![beforeUpdate()](https://i.imgur.com/DGuNkDs.png)

`beforeUpdate()` could be useful for tracking the number of edits made to a component or even tracking the actions to create an “undo” feature.

</details>

<details>
<summary><b>Composition API <code>onBeforeUpdate()</code><b></summary>

- **Type**

```ts
function onBeforeUpdate(callback: () => void): void
```

- **Details**

This hook can be used to access the DOM state before Vue updates the DOM. It is also safe to modify component state inside this hook.

> **_This hook isn't called during server-side rendering._**

- **Example**

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="updateMessage">Update Message</button>
  </div>
</template>

<script>
import { ref, onBeforeUpdate } from 'vue'

export default {
  setup() {
    const message = ref('Hello, World!')

    const updateMessage = () => {
      message.value = 'Message Updated!'
    }

    onBeforeUpdate(() => {
      console.log('Component will update!')
    })

    return {
      message,
      updateMessage
    }
  }
}
</script>
```

![onBeforeUpdate()](https://i.imgur.com/fni2cWN.png)

</summary>

These methods are useful, but for a lot of use cases we may want to consider using watchers to detect these data changes instead. Watchers are good because they give the old value and the new value of the changed data.

Another option is using computed values to change the state based on elements.

</details>

### `update()` and `onUpdate()`

Progress ...