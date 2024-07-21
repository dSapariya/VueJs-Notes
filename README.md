# VueJs
Vue is a JavaScript framework.Vue is more lightweight and easier to start with.

There are two different ways to write code in Vue: 
- The Options API and The Composition API.

### Options API
The Options API organizes component code by logical options like `data`, `methods`, `computed`, `watch`, and lifecycle hooks. This is the traditional way of writing Vue components and is quite straightforward, making it easier for beginners to understand and use.

```javascript
export default {
  data() {
    return {
      message: 'Hello Vue!'
    };
  },
  methods: {
    greet() {
      alert(this.message);
    }
  }
};
```

### Composition API
The Composition API allows you to organize component code by logical concerns rather than options. This approach can be more flexible and scalable, especially for larger projects, as it lets you reuse logic more easily. It's inspired by React Hooks and can make code more readable and maintainable.

```javascript
import { ref } from 'vue';

export default {
  setup() {
    const message = ref('Hello Vue!');
    const greet = () => {
      alert(message.value);
    };
    
    return {
      message,
      greet
    };
  }
};
```

### Key Differences

- **Options API**: Ideal for simple and small applications. It's easier to read and write for beginners. 
- **Composition API**: Better for complex applications where logic needs to be reused or shared between components. It can make the code more maintainable and organized.

### Choosing Between the Two

- **Learning Curve**: If you're new to Vue or front-end development in general, starting with the Options API might be easier.
- **Project Size and Complexity**: For larger, more complex projects, the Composition API may offer better code organization and reusability.
- **Personal Preference**: Some developers prefer the structure of the Options API, while others like the flexibility of the Composition API.

---

# Vue Directives

Vue directives are special HTML attributes with the prefix `v-` that give the HTML tag extra functionality.

## List of Directives

- **v-bind**: Dynamically bind an attribute to an expression.
- **v-model**: Create two-way data binding on form input, textarea, and select elements.
- **v-if**: Conditionally render the element based on the expression.
- **v-else**: Define an "else" block for `v-if`.
- **v-else-if**: Define an "else-if" block for `v-if`.
- **v-show**: Toggle the element's visibility based on the expression.
- **v-for**: Render a list of items using an array or object.
- **v-on**: Attach event listeners to the element.
- **v-pre**: Skip compilation for this element and all its children.
- **v-once**: Render the element and component only once.
- **v-html**: Insert HTML content dynamically.
- **v-text**: Insert text content dynamically.
- **v-slot**: Define named slots for content distribution in a child component.
- **v-bind.sync**: Two-way binding of a prop from a parent component.
- **v-model.lazy**: Two-way binding that listens to the `change` event instead of `input`.
- **v-model.number**: Automatically typecast user input as a number.
- **v-model.trim**: Automatically trim whitespace from user input.

## v-show vs. v-if

The difference between `v-show` and `v-if` is that `v-if` creates (renders) the element depending on the condition, but with `v-show` the element is already created; `v-show` only changes its visibility.

- **v-show**: It is better to use `v-show` when switching the visibility of an object, because it is easier for the browser to do, and it can lead to a faster response and better user experience.

- **v-if**: A reason for using `v-if` over `v-show` is that `v-if` can be used with `v-else-if` and `v-else`.

---

## Vue Methods
Vue methods are functions that belong to the Vue instance under the `methods` property.

- Vue methods are great to use with event handling (`v-on`) to do more complex things.
- Vue methods can also be used to do other things than event handling.

## Vue Event Modifiers
Event modifiers in Vue modify how events trigger the running of methods and help us handle events in a more efficient and straightforward way.

Event modifiers are used together with the Vue `v-on` directive, for example:
- Prevent the default submit behavior of HTML forms (`v-on:submit.prevent`).
- Ensure that an event can only run once after the page is loaded (`v-on:click.once`).
- Specify what keyboard key to use as an event to run a method (`v-on:keyup.enter`).

## Vue Forms
Vue gives us an easy way to improve the user experience with forms by adding extra functionality like responsiveness and form validation.

- Vue uses the `v-model` directive when handling forms.

## Vue Computed Properties
Computed properties are like data properties, except they depend on other properties.

- Computed properties are written like methods, but they do not accept any input arguments.
- Computed properties are updated automatically when a dependency changes, while methods are called when something happens, like with event handling.
- Computed properties are used when outputting something that depends on something else.

### Computed Properties vs. Methods
- Methods run when called from HTML, but computed properties update automatically when a dependency changes.
- Computed properties are used the same way we use data properties, but they are dynamic.

## Vue Watchers
A watcher is a method that watches a data property with the same name.

- A watcher runs every time the data property value changes.
- Use a watcher if a certain data property value requires an action.

### Watchers vs. Methods
- Methods are called from HTML.
- Methods are often called when an event happens.
- Methods automatically receive the event object as an input.
- We can also send other values we choose as an input to a method.
- Watchers are only called when the watched data property value changes, and this happens automatically.
- Watchers automatically receive the new and old value from the watched property.
- We cannot choose to send any other values with a watcher as an input.

### Watchers vs. Computed Properties
- Watchers and computed properties are both written as functions.
- Watchers and computed properties are both called automatically when a dependency changes, and never called from HTML.

#### Differences between computed properties and watchers:
- Watchers only depend on one property, the property they are set up to watch.
- Computed properties can depend on many properties.
- Computed properties are used like data properties, except they are dynamic.
- Watchers are not referred to from HTML.

---

## Vue Templates
A template in Vue is what we call the HTML part of our Vue application.

- The `<template>` tag will later be used in `.vue` files to structure our code in a better way.
- It is possible to use `template` as a configuration option in the Vue instance and put the HTML code inside.

```javascript
const app = Vue.createApp({
  template: `
    <h1>{{ message }}</h1>
    <p>This is a second line of HTML code, inside back tick quotes</p>
  `,
  data() {
    return {
      message: "Hello World!"
    };
  }
});
app.mount('#app');
```

## Single File Components (SFCs)
SFCs (Single File Components), or `.vue` files, are easier to work with but cannot run directly in the browser. We need to set up our computer to compile our `.vue` files to `.html`, `.css`, and `.js` files so that the browser can run our Vue application.

All SFC `.vue` files consist of three parts:
- `<template>`: where the HTML content is.
- `<script>`: for our Vue code.
- `<style>`: where we write the CSS styling.

Example of a Single File Component (`MyComponent.vue`):

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <p>This is a single file component</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello from SFC!"
    };
  }
};
</script>

<style>
div {
  color: blue;
}
</style>
```

## Vue Components
Components in Vue let us decompose our web page into smaller pieces that are easy to work with.

- We can work with a Vue component in isolation from the rest of the web page, with its own content and logic.
- A web page often consists of many Vue components.
- Components are reusable and self-contained pieces of code that encapsulate a specific part of the user interface, making Vue applications scalable and easier to maintain.

A very useful and powerful property when working with components in Vue is that we can make them behave individually, without having to mark elements with unique IDs like we must do with plain JavaScript. Vue automatically takes care to treat each component individually.

Example of a simple Vue component:

```javascript
Vue.component('my-component', {
  template: '<h1>{{ message }}</h1>',
  data() {
    return {
      message: 'Hello from My Component!'
    };
  }
});

const app = new Vue({
  el: '#app'
});
```

Example of using components in a Vue instance:

```html
<div id="app">
  <my-component></my-component>
</div>
```

With components, we can build complex user interfaces by composing smaller, reusable pieces of code, ensuring a clean and maintainable codebase.

---

## Vue Props
Props is a configuration option in Vue that allows us to pass data to components via custom attributes on the component tag.

### Defining Props

Props can be defined in a component with specific types, default values, and validators.

Example:

```javascript
Vue.component('food-item', {
  props: {
    foodDesc: {
      type: String,
      required: false,
      default: 'This is the default description.',
      validator: function(value) {
        return value.length > 20 && value.length < 50;
      }
    }
  },
  template: '<p>{{ foodDesc }}</p>'
});
```

In this example, `foodDesc` is a prop of type `String`, it is not required, has a default value, and includes a validator function that ensures the length of the value is between 20 and 50 characters.

### Using Props

Props are passed to components via custom attributes in the component tag.

Example:

```html
<div id="app">
  <food-item food-desc="This is a custom description that fits within the length constraints."></food-item>
  <food-item></food-item>
</div>
```

```javascript
new Vue({
  el: '#app'
});
```

In this example, the first `food-item` component receives a custom `food-desc` value, while the second `food-item` component uses the default description.

---

## Vue $emit()
With the built-in `$emit()` method in Vue, we can create a custom event in the child component that can be captured in the parent element.

- Props are used to send data from the parent element to the child component.
- `$emit()` is used to do the opposite: to pass information from the child component to the parent.

### Example

**Parent Component:**

```html
<div id="app">
  <child-component @custom-event="handleEvent"></child-component>
</div>
```

```javascript
new Vue({
  el: '#app',
  methods: {
    handleEvent(message) {
      alert(message);
    }
  }
});
```

**Child Component:**

```javascript
Vue.component('child-component', {
  template: `
    <button @click="sendEvent">Click me</button>
  `,
  methods: {
    sendEvent() {
      this.$emit('custom-event', 'Hello from the child component!');
    }
  }
});
```

In this example:
- The parent component listens for the `custom-event` from the child component using `@custom-event="handleEvent"`.
- The child component triggers the `custom-event` using `this.$emit('custom-event', 'Hello from the child component!')`.
- The `handleEvent` method in the parent component is called when the event is emitted, and it displays an alert with the message from the child component.


Here's the updated README file for Vue Fallthrough Attributes without the slot example:

---

# Vue Guide: Fallthrough Attributes

## Vue Fallthrough Attributes

In Vue, components can be called with attributes that are not explicitly declared as props. These attributes will fall through to the root element of the component. This feature allows you to:

- **Simplify Code**: You don’t need to declare every attribute as a prop in your component, which reduces boilerplate.
- **Maintain Flexibility**: Attributes like `class`, `style`, and `v-on` can be passed directly to the root element, providing a cleaner and more intuitive approach.

### Example

**Parent Component:**

```html
<div id="app">
  <custom-button class="btn-primary" style="font-size: 20px;" @click="handleClick">Click Me</custom-button>
</div>
```

**Child Component (`CustomButton.vue`):**

```vue
<template>
  <button v-bind="$attrs" v-on="$listeners">
    Click Me
  </button>
</template>

<script>
export default {
  name: 'CustomButton',
  inheritAttrs: false,  // Prevent automatic inheritance of attributes
};
</script>
```

In this example:
- The `custom-button` component receives `class`, `style`, and `@click` attributes from the parent component.
- These attributes fall through to the `<button>` element in the `CustomButton` component.

### `$attrs` and `v-bind`

When using fallthrough attributes, Vue’s `$attrs` object contains all attributes not declared as props. You can use `$attrs` with `v-bind` to apply these attributes to the root element.

**Example:**

```vue
<template>
  <input v-bind="$attrs" v-on="$listeners" />
</template>

<script>
export default {
  name: 'CustomInput',
  inheritAttrs: false,
};
</script>
```

In this example:
- The `input` element receives attributes like `type`, `placeholder`, and event listeners (`v-on`) from the parent component.

### Multiple Root Elements

If a component has multiple root elements, it’s essential to specify which element should receive the fallthrough attributes.

**Example:**

```vue
<template>
  <div>
    <span v-bind="$attrs" v-on="$listeners">Text</span>
    <button v-bind="$attrs" v-on="$listeners">Button</button>
  </div>
</template>

<script>
export default {
  name: 'MultipleRoots',
  inheritAttrs: false,
};
</script>
```

In this case:
- Attributes will apply to both `<span>` and `<button>` elements. You may need to manually assign `$attrs` to specific elements if this is not the desired behavior.

### Summary

Fallthrough attributes in Vue provide a way to pass attributes to components without declaring them as props, simplifying your component code. By using `$attrs` and `v-bind`, you can efficiently handle attributes and maintain flexibility in your component designs.

---