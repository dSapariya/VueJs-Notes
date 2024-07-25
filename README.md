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