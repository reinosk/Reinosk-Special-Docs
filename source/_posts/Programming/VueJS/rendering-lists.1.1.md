---
title: 'VueJS #4 Rendering Lists 1.1'
date: 2024-04-16 08:12:21
toc: true
---

## Maintaining State with `key`
​When Vue is updating a list of elements rendered with `v-for`, by default it uses an "in-place patch" strategy. If the order of the data items has changed, instead of moving the DOM elements to match the order of the items, Vue will patch each element in-place and make sure it reflects what should be rendered at that particular index.

This default mode is efficient, but only suitable when your list render output does not rely on child component state or temporary DOM state (e.g. form input values).

To give Vue a hint so that it can track each node's identity, and thus reuse and reorder existing elements, you need to provide a unique `key` attribute for each item:

```vue
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>
```

When using `<template v-for>`, the key should be placed on the `<template>` container:

```vue
<template v-for="todo in todos" :key="todo.name">
  <li>{{ todo.name }}</li>
</template>
```

> **_Note_**
>
> _`key` here is a special attribute being bound with `v-bind`. It should not be confused with the property key variable when [using `v-for` with an object](https://vuejs.org/guide/essentials/list.html#v-for-with-an-object)._

[It is recommended](https://vuejs.org/style-guide/rules-essential#use-keyed-v-for) to provide a `key` attribute with `v-for` whenever possible, unless the iterated DOM content is simple (i.e. contains no components or stateful DOM elements), or you are intentionally relying on the default behavior for performance gains.

The `key` binding expects primitive values - i.e. strings and numbers. Do not use objects as `v-for` keys. For detailed usage of the `key` attribute, please see the key [API documentation](https://vuejs.org/api/built-in-special-attributes#key).

## `v-for` with a Component

> _This section assumes knowledge of [Components](https://vuejs.org/guide/essentials/component-basics). Feel free to skip it and come back later._

You can directly use `v-for` on a component, like any normal element (don't forget to provide a `key`):

```vue
<MyComponent v-for="item in items" :key="item.id" />
```

However, this won't automatically pass any data to the component, because components have isolated scopes of their own. In order to pass the iterated data into the component, we should also use props:

```vue
<MyComponent
  v-for="(item, index) in items"
  :item="item"
  :index="index"
  :key="item.id"
/>
```

The reason for not automatically injecting `item` into the component is because that makes the component tightly coupled to how `v-for` works. Being explicit about where its data comes from makes the component reusable in other situations.
​
### Example of a Simple Todo List
This example of a simple todo list to see how to render a list of components using v-for, passing different data to each instance.

```vue
// App.vue

<script>
import TodoItem from './TodoItem.vue'
  
export default {
  components: { TodoItem },
  data() {
    return {
      newTodoText: '',
      todos: [
        {
          id: 1,
          title: 'Do the dishes'
        },
        {
          id: 2,
          title: 'Take out the trash'
        },
        {
          id: 3,
          title: 'Mow the lawn'
        }
      ],
      nextTodoId: 4
    }
  },
  methods: {
    addNewTodo() {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
}
</script>

<template>
	<form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    />
    <button>Add</button>
  </form>
  <ul>
    <todo-item
      v-for="(todo, index) in todos"
      :key="todo.id"
      :title="todo.title"
      @remove="todos.splice(index, 1)"
    ></todo-item>
  </ul>
</template>
```

```vue
// TodoItem.vue

<script>
export default {
	props: ['title'],
  emits: ['remove']
}
</script>

<template>
  <li>
    {{ title }}
    <button @click="$emit('remove')">Remove</button>
  </li>
</template>
```

![Simple Todo List](https://i.imgur.com/aI6sJWG.png)

## Array Change Detection
### Mutation Methods
​Vue is able to detect when a reactive array's mutation methods are called and trigger necessary updates. These mutation methods are:

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

### Replacing an Array
​Mutation methods, as the name suggests, mutate the original array they are called on. In comparison, there are also non-mutating methods, e.g. `filter()`, `concat()` and `slice()`, which do not mutate the original array but always return a new array. When working with non-mutating methods, we should replace the old array with the new one:

```js
this.items = this.items.filter((item) => item.message.match(/Foo/))
```

You might think this will cause Vue to throw away the existing DOM and re-render the entire list - luckily, that is not the case. Vue implements some smart heuristics to maximize DOM element reuse, so replacing an array with another array containing overlapping objects is a very efficient operation.

## Displaying Filtered/Sorted Results
​Sometimes we want to display a filtered or sorted version of an array without actually mutating or resetting the original data. In this case, you can create a computed property that returns the filtered or sorted array.

For example:

```js
data() {
  return {
    numbers: [1, 2, 3, 4, 5]
  }
},
computed: {
  evenNumbers() {
    return this.numbers.filter(n => n % 2 === 0)
  }
}
```

```vue
<li v-for="n in evenNumbers">{{ n }}</li>
```

In situations where computed properties are not feasible (e.g. inside nested `v-for` loops), you can use a method:

```js
data() {
  return {
    sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
  }
},
methods: {
  even(numbers) {
    return numbers.filter(number => number % 2 === 0)
  }
}
```

```vue
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>
```

Be careful with `reverse()` and `sort()` in a computed property! These two methods will mutate the original array, which should be avoided in computed getters. Create a copy of the original array before calling these methods:

```diff
- return numbers.reverse()
+ return [...numbers].reverse()
```