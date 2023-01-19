# Vue.js

## Template syntax

### Interpolations

#### Text

```vue
<span>Message: {{ msg }}</span>
```

```vue
<span v-once>This will never change: {{ msg }}</span>
```

#### Raw HTML

```vue
<p>
  Use v-html directive:
  <span v-html="rawHtml"></span>
</p>
```

#### Attributes

```vue
<div v-bind:id="dynamicId"></div>
```

##### Boolean value

If `isButtonDisabled` is falsy, the attribute `disabled` will not appear in the rendered DOM.

```vue
<button v-bind:disabled="isButtonDisabled">Button</button>
```

#### JavaScript expressions

<u>*Only single line expressions!*</u>

```vue
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

### Directives

Directives reactively apply side effects to the DOM when the value of its expression changes.

**`v-if`** shows/hides a tag based on the value of the expression.

```vue
<p v-if="seen">Now you see me</p>
```

#### Arguments

Some directives can take an "argument" followed by a `:`.

##### **`v-bind:attribute`**

Updates an HTML attribute based on the value of the expression. It can be shortened to `:attribute`.

```vue
<a v-bind:href="url">Go to link</a>
<a :href="url">Go to link</a>
<a :[attr]="url">Go to link</a>
```

##### **`v-on:event`**

Listens to DOM events. It can be shortened to `@event`.

```vue
<a v-on:click="doStuff">Click me</a>
<a @click="doStuff">Click me</a>
<a @[event]="doStuff">Click me</a>
```

#### Dynamic Arguments

`null` removes the binding. Use lower-case keys (`someattr`) for in-DOM templates.

```vue
<a v-bind:[someAttr]="value"></a>
```

## Computed Properties and Watchers

```vue
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
  <p>Same computed reversed message: "{{ reverseMessage() }}"</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    },

    // getter and setter
    reversed: {
      get: function () {
        return this.message.split('').reverse().join('')
      },
      set: function (newReversedValue) {
        this.message = newReversedValue.split('').reverse().join('')
      }
    }
  },
  methods: {
    reverseMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
```

The computed property `reversedMessage` is **always** dependent on the value of the property `message`. Its value is cached, meaning unless `message` changes, the value is never recalculated.
The method `reverseMessage`, instead, will run the function everytime it's being called.



## props

```javascript
props: {
	pointer: {
		type: Boolean,
		default: false
	},
	icon: {
		type: String,
		// When default is obj, use callback 
		default: () => ({ text: 'info', color: 'blue' })
	}
}
```

