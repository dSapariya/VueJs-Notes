# VueJs
Vue is a JavaScript framework.Vue is more lightweight and easier to start with.

There are two different ways to write code in Vue: 
- The Options API and The Composition API.

### Options API
The Options API organizes component code by logical options like `data`, `methods`, `computed`, `watch`, and lifecycle hooks. This is the traditional way of writing Vue components and is quite straightforward, making it easier for beginners to understand and use.
Properties defined by options are exposed on this inside functions, which points to the component instance:

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

or 

```vue
<script setup>
  import { ref, onMounted, computed } from 'vue'

  const typeWriters = ref(10);

  onMounted(() => {
    console.log(`The initial count is ${count.value}.`)
  })

  function remove(){
    if(typeWriters.value>0){
      typeWriters.value--;
    }
  }

  const storageComment = computed(
    function(){
      if(typeWriters.value > 5) {
        return "Many left"
      }
      else if(typeWriters.value > 0){
        return "Very few left"
      }
      else {
        return "No typewriters left"
      }
    }
  )
</script>
```

### Key Differences

- **Options API**: Ideal for simple and small applications. It's easier to read and write for beginners. 
- **Composition API**: Better for complex applications where logic needs to be reused or shared between components. It can make the code more maintainable and organized.

### Choosing Between the Two

- **Learning Curve**: If you're new to Vue or front-end development in general, starting with the Options API might be easier.
- **Project Size and Complexity**: For larger, more complex projects, the Composition API may offer better code organization and reusability.
- **Personal Preference**: Some developers prefer the structure of the Options API, while others like the flexibility of the Composition API.

---

## Vue.js Application Setup

### Root Component

The object we are passing into `createApp` is, in fact, a component. An application instance won't render anything until its `.mount()` method is called. The `.mount()` method should always be called after all app configurations and asset registrations are done.

```javascript
import { createApp } from 'vue';
// import the root component App from a single-file component.
import App from './App.vue';

const app = createApp(App);
app.mount('#app');
```

### Multiple Application Instances

You are not limited to a single application instance on the same page. The `createApp` API allows multiple Vue applications to co-exist on the same page, each with its own scope for configuration and global assets:

```javascript
const app1 = createApp({
  /* ... */
});
app1.mount('#container-1');

const app2 = createApp({
  /* ... */
});
app2.mount('#container-2');
```

This setup enables you to manage different parts of the page with separate Vue applications, each with its own configurations and components.

## Vue.js Reactivity and Refs

### Declaring Reactive State with `ref()`

In the Composition API, the recommended way to declare reactive state is using the `ref()` function.

```javascript
import { ref } from 'vue';

const count = ref(0);
```

### How `ref()` Works

- **Ref Object**: `ref()` takes the argument and returns it wrapped within a ref object with a `.value` property.
- **Template Convenience**: Notice that you do not need to append `.value` when using the ref in the template. For convenience, refs are automatically unwrapped when used inside templates (with a few caveats).

### Why Refs?

You might be wondering why we need `refs` with the `.value` property instead of plain variables. To explain that, we need to briefly discuss how Vue's reactivity system works.

- **Automatic DOM Updates**: When you use a `ref` in a template and change the `ref`'s value later, Vue automatically detects the change and updates the DOM accordingly. This is made possible with a dependency-tracking-based reactivity system.
- **Dependency Tracking**: When a component is rendered for the first time, Vue tracks every `ref` that was used during the render. Later on, when a `ref` is mutated, it will trigger a re-render for components that are tracking it.
- **Standard JavaScript Limitation**: In standard JavaScript, there is no way to detect the access or mutation of plain variables. However, we can intercept the `get` and `set` operations of an object's properties using getter and setter methods.

### The `.value` Property

The `.value` property gives Vue the opportunity to detect when a `ref` has been accessed or mutated. Under the hood, Vue performs tracking in its getter and performs triggering in its setter. Conceptually, you can think of a `ref` as an object that looks like this:

```javascript
const ref = {
  get value() {
    // Track the dependency
  },
  set value(newValue) {
    // Trigger updates
  }
};
```

By using `refs`, Vue ensures a responsive and reactive UI by automatically handling changes to the data and updating the DOM efficiently.

## Deep Reactivity​
Refs can hold any value type, including deeply nested objects, arrays, or JavaScript built-in data structures like Map.

A ref will make its value deeply reactive. This means you can expect changes to be detected even when you mutate nested objects or arrays:

```javascript
import { ref } from 'vue'

const obj = ref({
  nested: { count: 0 },
  arr: ['foo', 'bar']
})

function mutateDeeply() {
  // these will work as expected.
  obj.value.nested.count++
  obj.value.arr.push('baz')
}
```

# reactive()​
There is another way to declare reactive state, with the reactive() API. Unlike a ref which wraps the inner value in a special object, reactive() makes an object itself reactive:

```javascript
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

## Vue Reactivity: `reactive` vs `ref`

### `reactive`

The `reactive` function in Vue is used to create a deeply reactive state object.

- **Usage**: Best for creating reactive objects.
- **Automatic Unwrapping**: Properties are automatically unwrapped when accessed in the template.
- **Deep Reactivity**: All nested properties are made reactive.

**Example**:
```javascript
import { reactive } from 'vue';

const state = reactive({
  count: 0,
  user: {
    name: 'John',
    age: 30
  }
});
```

### `ref`

The `ref` function in Vue is used to create a reactive reference to a value.

- **Usage**: Best for primitive values or single reactive properties.
- **.value Property**: The reactive value is accessed and mutated using the `.value` property.
- **Shallow Reactivity**: Only the value itself is reactive; nested properties are not automatically reactive unless they are refs or reactive objects themselves.

**Example**:
```javascript
import { ref } from 'vue';

const count = ref(0);
const user = ref({
  name: 'John',
  age: 30
});
```

### Key Differences

1. **Reactivity Depth**:
   - `reactive` creates a deeply reactive object where all nested properties are reactive.
   - `ref` creates a shallow reactive reference where only the value is reactive. For objects, you need to wrap nested properties in `ref` or `reactive` if you want them to be reactive.

2. **Accessing Values**:
   - With `reactive`, you access properties directly.
   - With `ref`, you access the value using `.value`.

3. **Usage Context**:
   - Use `reactive` when you need a reactive state object with multiple properties.
   - Use `ref` for primitive values or when you need a single reactive property.

### When to Use Which

- **`reactive`**:
  - When you need to manage complex state with nested properties.
  - Example: Managing form data, complex objects, or state that involves multiple related properties.

- **`ref`**:
  - When you need to manage a single reactive property or primitive value.
  - Example: Single pieces of state like counters, flags, or individual reactive properties within a larger state object.

### Combining `reactive` and `ref`

You can combine `reactive` and `ref` to manage state efficiently.

**Example**:
```javascript
import { reactive, ref } from 'vue';

const state = reactive({
  user: {
    name: 'John',
    age: ref(30) // Nested ref for a single property
  },
  loggedIn: ref(false) // Single ref for a boolean flag
});
```

This approach allows you to create a deeply reactive object with `reactive` while still taking advantage of `ref` for specific properties when needed.

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

---

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

# Scoped Styling

Styling defined inside the `<style>` tag in a component, or in `App.vue`, is actually available globally in all components.

To keep the styling limited locally to just the component, we can use the scope attribute on that component: `<style scoped>`

---

# Local Components

In Vue.js, components can be registered locally within other components. This allows you to use a component only within the context of another component, keeping your global scope clean and improving the reusability of components.

## Registering Local Components

To register a component locally, you need to import it and then declare it in the `components` option of your Vue component.

### Steps to Register a Local Component

1. **Import the Component:**
   First, import the component you want to register locally.

2. **Declare the Component:**
   Next, declare the imported component in the `components` option of your Vue component.

### Example

Here’s a basic example of how to register and use a local component:

```javascript
<template>
  <div>
    <LocalComponent />
  </div>
</template>

<script>
import LocalComponent from './LocalComponent.vue';

export default {
  name: 'ParentComponent',
  components: {
    LocalComponent
  }
};
</script>
```

In this example:
- `LocalComponent.vue` is imported.
- It is then registered locally in the `ParentComponent` using the `components` option.
- `LocalComponent` can now be used within the template of `ParentComponent`.

## Benefits of Local Components

- **Scoped Usage:** Local components are only available within the component that registers them, which helps in avoiding naming conflicts and keeps the global scope clean.
- **Improved Reusability:** By keeping components modular and local, you can easily reuse and manage them within different parts of your application.
- **Better Organization:** Local registration helps in maintaining a clear structure and organization in your codebase, making it easier to navigate and understand.

Using local components is a good practice in larger applications to ensure a modular and maintainable codebase.

# Global Components

In Vue.js, components can be registered globally, making them available throughout your entire application without needing to import them individually in each component.

## Registering Global Components

To register a component globally, you typically do this in your main JavaScript file, usually `main.js`, where your Vue instance is created.

### Steps to Register a Global Component

1. **Import the Component:**
   First, import the component you want to register globally.

2. **Use `Vue.component` to Register:**
   Register the component globally using the `Vue.component` method.

### Example

Here’s a basic example of how to register and use a global component:

```javascript
// main.js
import Vue from 'vue';
import App from './App.vue';
import GlobalComponent from './components/GlobalComponent.vue';

Vue.component('GlobalComponent', GlobalComponent);

new Vue({
  render: h => h(App),
}).$mount('#app');
```

In this example:
- `GlobalComponent.vue` is imported.
- It is then registered globally using `Vue.component`.
- `GlobalComponent` can now be used in the template of any component within the application without needing to import it again.

### Using the Global Component

Once registered globally, you can use the component in any template:

```vue
<template>
  <div>
    <GlobalComponent />
  </div>
</template>

<script>
export default {
  name: 'AnyComponent'
};
</script>
```

## Benefits of Global Components

- **Ease of Use:** Global components can be used in any part of the application without the need for repetitive import statements.
- **Centralized Registration:** Components are registered in a central place, typically in `main.js`, making it easy to manage.
- **Convenience:** For frequently used components (e.g., buttons, modals, etc.), global registration provides a convenient way to ensure availability across the application.

## Considerations

- **Namespace Pollution:** Registering too many components globally can lead to namespace pollution and potential conflicts, especially in large applications.
- **Maintainability:** As the number of global components grows, it can become harder to track where components are used and manage dependencies.

Using global components is a powerful feature in Vue.js, but it should be used judiciously to maintain a clean and maintainable codebase. For components that are used in specific parts of the application, consider using local registration to keep your code modular and organized.


# Slots in Vue.js

Slots are a powerful feature in Vue that allow for more flexible and reusable components. They enable you to pass content from a parent component into a child component, enhancing the versatility and modularity of your application.

## Basic Usage

Slots provide a way to pass content from the parent component into the `<template>` of a child component. This allows the child component to render content specified by the parent, making it more reusable.

### Example

```vue
<!-- ParentComponent.vue -->
<template>
  <div>
    <ChildComponent>
      <p>This content is passed from the parent to the child.</p>
    </ChildComponent>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  name: 'ParentComponent',
  components: {
    ChildComponent
  }
};
</script>

<!-- ChildComponent.vue -->
<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'ChildComponent'
};
</script>
```

In this example:
- The parent component (`ParentComponent.vue`) uses the `ChildComponent` and provides content within the `<ChildComponent>` tags.
- The child component (`ChildComponent.vue`) has a `<slot>` element in its template, which acts as a placeholder for the content passed from the parent.

## v-slot(Named slots)

We need the v-slot directive to refer to named slots.
Named slots allow for more control over where the content is placed within the child component's template.
Named slots can be used to create more flexible and reusable components.
Named slots allow you to pass multiple sections of content to different parts of the child component.

v-slot can also be used in a <template> tag to direct larger parts of content to a certain <slot>. becaues We use the <template> tag to direct some content to a certain <slot> because the <template> tag is not rendered, it is just a placeholder for the content. You can see this by inspecting the built page: you will not find the template tag there.

### Example

```vue
<!-- ParentComponent.vue -->
<template>
  <div>
    <ChildComponent>
      <template v-slot:header>
        <h1>This is the header content.</h1>
      </template>
      <template v-slot:footer>
        <p>This is the footer content.</p>
      </template>
    </ChildComponent>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  name: 'ParentComponent',
  components: {
    ChildComponent
  }
};
</script>

<!-- ChildComponent.vue -->
<template>
  <div>
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'ChildComponent'
};
</script>
```

In this example:
- The parent component uses named slots (`header` and `footer`) to pass content to specific parts of the child component.
- The child component defines multiple slots and uses the `name` attribute to identify them.

The shorthand for v-slot: is #.

## Scoped Slots

A Scoped slot provides local data from the component so that the parent can choose how to render it.

Scoped slots provide a way for the child component to pass data back to the parent component, making the slot content dynamic.

We use v-bind or : in the component(child) slot to send local data to the parent:

### Example

```vue
<!-- ParentComponent.vue -->
<template>
  <div>
    <ChildComponent>
      <template v-slot:default="slotProps">
        <p>Message from child: {{ slotProps.message }}</p>
      </template>
    </ChildComponent>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  name: 'ParentComponent',
  components: {
    ChildComponent
  }
};
</script>

<!-- ChildComponent.vue -->
<template>
  <div>
    <slot :message="message"></slot>
  </div>
</template>

<script>
export default {
  name: 'ChildComponent',
  data() {
    return {
      message: 'Hello from child component!'
    };
  }
};
</script>
```

In this example:
- The child component passes data (`message`) to the parent component through the scoped slot.
- The parent component accesses the data using `slotProps` and renders it within the slot content.

Slot: The basic placeholder for content passed from the parent component to the child component.
v-slot: The directive used to denote named and scoped slots in the parent component.
Scoped Slot: A slot that allows the child component to pass data back to the parent component, making the slot content dynamic.

### Dynamic Components

Dynamic Components can be used to flip through pages within your page, like tabs in your browser, with the use of the 'is' attribute.

The Component Tag and The 'is' Attribute
To make a dynamic component we use the <component> tag to represent the active component. The 'is' attribute is tied to a value with v-bind, and we change that value to the name of the component we want to have active.
 
Using `v-if`/`v-else` and dynamic components with the `<component>` tag are both methods to conditionally render components in Vue.js, but they have different use cases and implications.

### `v-if`/`v-else`
`v-if`/`v-else` directives allow you to conditionally render elements based on a boolean expression.

**Example:**
```html
<template>
  <div>
    <button @click="toggleValue = !toggleValue">
      Switch component
    </button>
    <comp-one v-if="toggleValue"></comp-one>
    <comp-two v-else></comp-two>
  </div>
</template>

<script>
import CompOne from './components/CompOne.vue'
import CompTwo from './components/CompTwo.vue'

export default {
  components: {
    'comp-one': CompOne,
    'comp-two': CompTwo
  },
  data() {
    return {
      toggleValue: true
    }
  }
}
</script>
```

### Dynamic Components with `<component :is="">`
Dynamic components use the `<component>` tag with the `:is` attribute to dynamically switch between components. 

**Example:**
```html
<template>
  <div>
    <button @click="toggleValue = !toggleValue">
      Switch component
    </button>
    <component :is="activeComp"></component>
  </div>
</template>

<script>
import CompOne from './components/CompOne.vue'
import CompTwo from './components/CompTwo.vue'

export default {
  components: {
    'comp-one': CompOne,
    'comp-two': CompTwo
  },
  data() {
    return {
      toggleValue: true
    }
  },
  computed: {
    activeComp() {
      return this.toggleValue ? 'comp-one' : 'comp-two';
    }
  }
}
</script>
```

### Key Differences

1. **Reusability and Extensibility**:
   - **`v-if`/`v-else`**: Straightforward for simple conditions and small numbers of components. Less flexible if you need to switch among many components.
   - **Dynamic Components**: More scalable and easier to manage when switching among many components. Simply change the value of `:is` to switch components.

2. **Component State Preservation**:
   - **`v-if`/`v-else`**: Components are destroyed and recreated each time the condition changes. This means their state (e.g., input data, scroll position) is lost.
   - **Dynamic Components**: You can preserve the state of the component by using `<keep-alive>`.

     ```html
     <template>
       <div>
         <button @click="toggleValue = !toggleValue">
           Switch component
         </button>
         <keep-alive>
           <component :is="activeComp"></component>
         </keep-alive>
       </div>
     </template>
     ```

3. **Performance**:
   - **`v-if`/`v-else`**: Might be more efficient for very simple toggles because there's no need for the `<component>` abstraction.
   - **Dynamic Components**: May be more efficient for complex scenarios with multiple components and state preservation needs.

4. **Use Cases**:
   - **`v-if`/`v-else`**: Best for simple, binary conditional rendering where the components do not need to maintain state between switches.
   - **Dynamic Components**: Best for more complex scenarios, especially when dealing with many components or when component state needs to be preserved.

### Summary
- Use `v-if`/`v-else` for simple, straightforward conditional rendering.
- Use dynamic components for more complex, scalable, and stateful component switching.

---

## Dynamic Components with `<KeepAlive>`

To keep the state of your previous inputs when returning to a component, use the `<KeepAlive>` tag around the `<component>` tag. This is necessary because without `<KeepAlive>`, the component is unmounted and then mounted again, reloading the component.

### Example

```vue
<template>
  <h1>Dynamic Components</h1>
  <p>App.vue switches between which component to show.</p>
  <button @click="toggleValue = !toggleValue">
    Switch component
  </button>
  <KeepAlive>
    <component :is="activeComp"></component>
  </KeepAlive>
</template>

<script>
import CompOne from './components/CompOne.vue'
import CompTwo from './components/CompTwo.vue'

export default {
  components: {
    'comp-one': CompOne,
    'comp-two': CompTwo
  },
  data() {
    return {
      toggleValue: true
    }
  },
  computed: {
    activeComp() {
      return this.toggleValue ? 'comp-one' : 'comp-two';
    }
  }
}
</script>
```

### The `include` and `exclude` Attributes

All components inside the `<KeepAlive>` tag will be kept alive by default. However, you can specify which components to keep alive using the `include` or `exclude` attributes on the `<KeepAlive>` tag.

When using the `include` or `exclude` attributes, you must name the components with the `name` option.

**Example:**
```vue
<script>
export default {
  name: 'CompOne',
  data() {
    return {
      imgSrc: 'img_question.svg'
    }
  }
}
</script>
```

- `<KeepAlive include="CompOne">`: Only the `CompOne` component will remember its state.
- `<KeepAlive exclude="CompOne">`: Only the `CompTwo` component will remember its state.
- `<KeepAlive include="CompOne, CompThree">`: Both the `CompOne` and the `CompThree` components will remember their state.

### The `max` Attribute

You can use the `max` attribute on the `<KeepAlive>` tag to limit the number of components the browser needs to remember the state of.

**Example:**
```vue
<KeepAlive :max="2">
  <component :is="activeComp"></component>
</KeepAlive>
```
With `<KeepAlive :max="2">`, the browser will only remember the user input of the last two visited components.

---

Use `<KeepAlive>` with dynamic components, including how to specify which components should retain their state and how to limit the number of components that do so.

---

## Teleport

The Vue `<Teleport>` tag is used to move content to a different place in the DOM structure.

### `<Teleport>` and the `to` Attribute

To move some content to another location in the DOM structure, use the Vue `<Teleport>` tag around the content and the `to` attribute to define where to move it.

**Example:**
```vue
<Teleport to="body">
  <p>Hello!</p>
</Teleport>
```

The `to` attribute value is given as a CSS selector. If you want to send some content to the `body` tag, as in the code above, you simply write `<Teleport to="body">`.

## Usecase 
Modals and Dialogs: Use <Teleport> to render modals directly under the <body> tag to avoid z-index and CSS issues.
Tooltips: Use <Teleport> to position tooltips relative to the viewport for accurate placement.
Context Menus: Use <Teleport> to display context menus at the mouse position for proper interaction.

---

## Template Refs

Template Refs are used to refer to specific DOM elements in Vue.js.

### What are Template Refs?

When the `ref` attribute is set on an HTML tag, the resulting DOM element is added to the `$refs` object. Template Refs provide an alternative to methods in plain JavaScript like `getElementById()` or `querySelector()`.

### Example

```vue
<template>
  <h1>Example</h1>
  <p>Click the button to put "Hello!" as the text in the green p element.</p>
  <button @click="changeVal">Change Text</button>
  <p ref="pEl">This is the initial text</p>
</template>

<script>
export default {
  methods: {
    changeVal() {
      this.$refs.pEl.innerHTML = "Hello!";
    }
  }
}
</script>
```

In this example:

- The `ref` attribute is set on the `<p>` element, giving it a reference name of `pEl`.
- The `changeVal` method is called when the button is clicked.
- The `changeVal` method accesses the `<p>` element via `this.$refs.pEl` and changes its inner HTML to "Hello!".

### Use Cases

Template Refs are useful in scenarios where direct manipulation of the DOM is needed, such as:

- Focusing an input element programmatically.
- Integrating with third-party libraries that require direct DOM access.
- Managing animations or transitions that need to interact with DOM elements directly.

### Summary

Template Refs in Vue.js provide a simple and effective way to reference and manipulate specific DOM elements within your components, enhancing the flexibility and control you have over your application's user interface.

---

## Lifecycle Hooks

In Vue.js, every time a component reaches a new stage in its lifecycle, a specific function runs. We can add code to these functions, which are called lifecycle hooks because we can "hook" our code into that stage.

### Lifecycle Hooks Overview and Usage

### beforeCreate

- **Description**: Happens before the component is initialized, before Vue has set up the component's data, computed properties, methods, and event listeners.
- **Usage**: Rarely used, as no data or methods are available.
- **When to Use**: Perform logic that does not depend on the component's properties or methods.

```javascript
beforeCreate() {
  console.log('Component is about to be created');
}
```

### created

- **Description**: Occurs after the component is initialized, so Vue has already set up the component's data, computed properties, methods, and event listeners. This hook is commonly used to fetch data with HTTP requests or set up initial data values.
- **Usage**: Commonly used for fetching initial data or setting up component properties.
- **When to Use**: Access to data, computed properties, methods, and events is available.

```javascript
created() {
  console.log('Component has been created');
  this.fetchData();
}
```

### beforeMount

- **Description**: Happens right before the component is mounted, just before it is added to the DOM.
- **Usage**: Last chance to perform logic before the component is added to the DOM.
- **When to Use**: Modify the component before it is inserted into the DOM.

```javascript
beforeMount() {
  console.log('Component is about to be mounted');
}
```

### mounted

- **Description**: Called right after the component is added to the DOM tree, making it a good place to interact with the DOM or perform operations that depend on the component being in the DOM.
- **Usage**: Perform DOM-dependent operations, such as initializing third-party libraries or manipulating the DOM.
- **When to Use**: The component is in the DOM and can interact with other DOM elements.

```javascript
mounted() {
  console.log('Component has been mounted');
  this.initializePlugin();
}
```

### beforeUpdate

- **Description**: Runs before the component's DOM is updated.
- **Usage**: Allows you to access the current state before the update.
- **When to Use**: Implement logic that needs to run before the DOM is updated.

```javascript
beforeUpdate() {
  console.log('Component is about to update');
}
```

### updated

- **Description**: Runs after the component's DOM has been updated.
- **Usage**: Perform actions after the DOM update is complete.
- **When to Use**: React to changes in the DOM or trigger additional updates.

```javascript
updated() {
  console.log('Component has been updated');
}
```

### beforeUnmount

- **Description**: Runs before the component is unmounted from the DOM.
- **Usage**: Perform cleanup tasks, such as removing event listeners.
- **When to Use**: The component is still in the DOM, but will be removed soon.

```javascript
beforeUnmount() {
  console.log('Component is about to be unmounted');
}
```

### unmounted

- **Description**: Runs after the component has been unmounted from the DOM.
- **Usage**: Clean up resources, such as timers or subscriptions.
- **When to Use**: The component is no longer in the DOM.

```javascript
unmounted() {
  console.log('Component has been unmounted');
}
```

### errorCaptured

- **Description**: Runs when an error is captured from a child or descendant component.
- **Usage**: Handle or log errors gracefully.
- **When to Use**: Monitor and respond to errors in the component tree.

```javascript
errorCaptured(error, vm, info) {
  console.error('Error captured:', error);
  return false; // Prevent the error from propagating further
}
```

### serverPrefetch

- **Description**: Runs only during server-side rendering (SSR).
- **Usage**: Prefetch data for server-rendered components.
- **When to Use**: Ensure data is ready before rendering on the server.

```javascript
serverPrefetch() {
  return this.fetchData();
}
```

### Summary

Vue.js lifecycle hooks provide a structured way to perform actions at different stages of a component's lifecycle. By leveraging these hooks, you can manage the creation, updating, and destruction of components effectively.

---

### Provide/Inject
Provide/Inject in Vue is used to provide data from one component to other components, particularly in large projects.
Provide makes data available to other components.
Inject is used to get the provided data.
Provide/Inject is a way to share data as an alternative to passing data using props.

Compare the `Props` and `Provide/Inject`

### Using Props

**Props** are a standard way to pass data from a parent component to its child components. This method is straightforward and works well for smaller or less complex component hierarchies.

**Example:**

```vue
<!-- ParentComponent.vue -->
<template>
  <ChildComponent :message="parentMessage" />
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent },
  data() {
    return {
      parentMessage: 'Hello from Parent'
    };
  }
};
</script>

<!-- ChildComponent.vue -->
<template>
  <p>{{ message }}</p>
</template>

<script>
export default {
  props: {
    message: String
  }
};
</script>
```

**Use Case:**
- When you have a clear hierarchy and need to pass data from parent to child components.
- Best for small to medium-sized applications where data flow is predictable and linear.

### Using Provide/Inject

**Provide/Inject** is useful in scenarios where you have deeply nested components, or when you want to share data or functions across many layers without prop drilling.

**Example:**

```vue
<!-- GrandparentComponent.vue -->
<template>
  <ParentComponent />
</template>

<script>
import ParentComponent from './ParentComponent.vue';

export default {
  components: { ParentComponent },
  provide() {
    return {
      sharedMessage: 'Hello from Grandparent'
    };
  }
};
</script>

<!-- ParentComponent.vue -->
<template>
  <ChildComponent />
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent }
};
</script>

<!-- ChildComponent.vue -->
<template>
  <p>{{ sharedMessage }}</p>
</template>

<script>
export default {
  inject: ['sharedMessage']
};
</script>
```

**Use Case:**
- When you need to share data or methods across multiple levels of nested components.
- Useful in large applications where passing data through props would be cumbersome.
- Ideal for sharing global configurations, theme settings, or services like a notification system.

### Comparison

1. **Simplicity:**
   - **Props:** Simple to use and understand. Data flows in a straightforward, one-way manner from parent to child.
   - **Provide/Inject:** More complex, especially for reactivity and when managing deeply nested components.

2. **Data Flow:**
   - **Props:** Data flow is unidirectional (from parent to child). It’s explicit and easy to track.
   - **Provide/Inject:** Data flow can be less explicit and harder to trace because it bypasses the normal parent-to-child relationship.

3. **Reactivity:**
   - **Props:** Reactive by default. Changes in the parent’s data automatically propagate to the child component.
   - **Provide/Inject:** Not reactive by default. To make it reactive, you might need to use Vue’s reactive objects or Vuex.

4. **Component Hierarchy:**
   - **Props:** Best for flat or moderately nested component trees.
   - **Provide/Inject:** Best for deeply nested component trees where prop drilling becomes problematic.

5. **Maintenance:**
   - **Props:** Easier to maintain and understand as the data flow is clear and explicit.
   - **Provide/Inject:** Can become harder to maintain if overused or misused, as the data sharing mechanism is less transparent.

In summary, while `Provide/Inject` offers a way to avoid prop drilling and manage data sharing in large applications, props remain a simpler and more predictable method for passing data in more straightforward component hierarchies.


Certainly! To make `Provide` and `Inject` reactive in Vue.js, you need to use Vue's reactivity system. Vue 3’s `ref` and `reactive` utilities are useful for this. 

Here’s an example using Vue 3 to demonstrate reactive `Provide` and `Inject`:

### Example: Reactive Provide/Inject

**1. GrandparentComponent.vue (Providing Data):**

```vue
<template>
  <div>
    <p>Grandparent Component</p>
    <button @click="changeMessage">Change Message</button>
    <ParentComponent />
  </div>
</template>

<script>
import { ref } from 'vue';
import ParentComponent from './ParentComponent.vue';

export default {
  components: { ParentComponent },
  setup() {
    const sharedMessage = ref('Hello from Grandparent');

    function changeMessage() {
      sharedMessage.value = 'Message updated from Grandparent';
    }

    return {
      sharedMessage,
      changeMessage
    };
  },
  provide() {
    return {
      sharedMessage: this.sharedMessage
    };
  }
};
</script>
```

**2. ParentComponent.vue (Intermediate Component):**

```vue
<template>
  <div>
    <p>Parent Component</p>
    <ChildComponent />
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent }
};
</script>
```

**3. ChildComponent.vue (Injecting Data):**

```vue
<template>
  <div>
    <p>Child Component</p>
    <p>{{ sharedMessage }}</p>
  </div>
</template>

<script>
import { inject } from 'vue';

export default {
  setup() {
    const sharedMessage = inject('sharedMessage');

    return {
      sharedMessage
    };
  }
};
</script>
```

### Explanation:

1. **GrandparentComponent.vue:**
   - **Reactive State:** We use `ref` to create a reactive variable `sharedMessage`.
   - **Provide:** We use the `provide` function to make `sharedMessage` available to descendant components. Since `provide` doesn’t inherently support reactivity, we return the `ref` object itself.
   - **Change Message:** A method to update the message, demonstrating that changes are reactive.

2. **ParentComponent.vue:**
   - This component acts as an intermediate layer and does not directly interact with the provided data. It simply passes down the component tree.

3. **ChildComponent.vue:**
   - **Inject:** We use `inject` to access the `sharedMessage` provided by the Grandparent component. Since we provided a `ref`, the injected value is reactive and will update automatically when the provided value changes.

### Points to Note:
- **Reactivity:** By providing the `ref` object directly, any changes to `sharedMessage` in the Grandparent component will automatically reflect in the Child component.
- **Component Hierarchy:** The data can be accessed by any component in the hierarchy below the provider, not just the immediate child.


### Vue Router Overview

**Vue Router** allows you to:
- Navigate between different components or views.
- Use URLs to direct users to specific parts of your application.
- Manage browser history for navigation with back and forward buttons.
- It happens on client side without full page reload, which results in a faster user experience.

### Installation

First, you need to install Vue Router if you haven't already:

```sh
npm install vue-router
```

### Setting Up Vue Router

**1. Defining Routes:**

You define routes to map paths to components.

```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router';
import HomeComponent from '../components/HomeComponent.vue';
import AboutComponent from '../components/AboutComponent.vue';

const routes = [
  { path: '/', component: HomeComponent },
  { path: '/about', component: AboutComponent }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;
```

**2. Registering Router in Vue Application:**

```javascript
// main.js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';

const app = createApp(App);
app.use(router);
app.mount('#app');
```

### `<router-view>` Component

The `<router-view>` component is a placeholder that Vue Router uses to display the component matched by the current route.

**Example:**

```vue
<!-- App.vue -->
<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
};
</script>
```

### `<router-link>` Component

The `<router-link>` component is used to create navigation links. It automatically applies active classes when the linked route is active.

**Example:**

```vue
<!-- HomeComponent.vue -->
<template>
  <div>
    <h1>Home</h1>
  </div>
</template>

<script>
export default {
  name: 'HomeComponent'
};
</script>

<!-- AboutComponent.vue -->
<template>
  <div>
    <h1>About</h1>
  </div>
</template>

<script>
export default {
  name: 'AboutComponent'
};
</script>
```

**Active Link Styling:**

By default, Vue Router adds the `router-link-active` class to the active `<router-link>`. You can customize this class if needed.

**Example:**

```vue
<!-- App.vue -->
<template>
  <div id="app">
    <nav>
      <router-link to="/" active-class="active-link">Home</router-link>
      <router-link to="/about" active-class="active-link">About</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<style>
.active-link {
  font-weight: bold;
  color: red;
}
</style>

<script>
export default {
  name: 'App'
};
</script>
```

### Browser History Management

Vue Router uses the HTML5 History API to manage routing. This allows navigation without full page reloads and supports back and forward button functionality.

### Summary

- **Vue Router**: Enables client-side routing in Vue applications.
- **<router-view>**: A placeholder for rendering matched components.
- **<router-link>**: Creates navigational links with automatic active state management.
- **URL Navigation**: Allows direct navigation to specific components using URLs.
- **Browser History**: Supports back and forward navigation using browser buttons.

This setup provides a seamless and fast user experience by eliminating full page reloads and enabling smooth navigation within your Vue application.


Build
The `npm run build` build command compiles our Vue project into .html, .js and .css files that are optimized to run directly in the browser.

When your project is built, Vite creates a folder dist with all the files needed to run your project on a public server, with files the browser understands *.html, *.css and *.js instead of the *.vue files we use during development.

To see your built project in the browser, use the commando: `npm run preview`