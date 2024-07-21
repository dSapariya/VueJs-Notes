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



Here's a complete README file for Vue directives:

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

Here's a complete README file for Vue methods, event modifiers, forms, computed properties, and watchers:

---

# Vue Guide

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
