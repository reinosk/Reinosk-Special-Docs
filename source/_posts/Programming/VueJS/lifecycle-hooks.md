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

Excluding `beforeCreate` and `created` (which are replaced by the `setup` method itself), there are 12 of the Options API lifecycle host that we can access in our setup method.

See details [Composition API Lifecycle Hooks](https://vuejs.org/api/composition-api-lifecycle.html).

When we import them and access them in our code, it would look like this.

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
</script>
```

without `setup`

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