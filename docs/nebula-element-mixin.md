# Nebula.ElementMixin

`Nebula.ElementMixin` is a Polymer Project [Class Expression Mixin](https://www.polymer-project.org/2.0/docs/devguide/custom-elements#mixins) that extends a custom element with a set of utility methods. The mixin adds utility functions similar to those provided in Polymer v1 (that were removed in Polymer v2) including `listen` and `unlisten`, `fire`, and `debounce`. It also provides the ability to define property observers and computed properties imperatively using `observe` and `compute`.

JavaScript does not have explicit support for private or protected methods. To avoid conflicts, Nebula elements and mixins use [Symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol), a new feature of ES2015 that provides the ability to define methods with tokens that are guaranteed to be unique. The use of symbols provide better conflict protection than the commonly used naming convention based on prefixing methods with underscores. Protected methods are defined using `Symbol.for` to add or retrieve them from the global registry, and private methods are defined using `Symbol` to create a symbol for local use only.

The following example demonstrates how to to retrieve symbols for calling the protected methods provided by this mixin, and how you utilize symbols in your own elements.

```html
<link rel="import" href="../nebula-element-mixin/nebula-element-mixin.html">

<script>
(function() {

  const _observe = Symbol.for('Nebula.ElementMixin.observe')
  const _compute = Symbol.for('Nebula.ElementMixin.compute')
  const _listen = Symbol.for('Nebula.ElementMixin.listen')
  const _unlisten = Symbol.for('Nebula.ElementMixin.unlisten')
  const _fire = Symbol.for('Nebula.ElementMixin.fire')
  const _debounce = Symbol.for('Nebula.ElementMixin.debounce')

  const __onResize = Symbol()
  const __onSizeChanged = Symbol()
  const __computeOrientation = Symbol()

  class WindowSize extends Nebula.ElementMixin(Polymer.Element) {
    static get is() { return 'window-size' }

    static get properties() {
      return {
        size: Object,
        orientation: String
      }
    }

    constructor() {
      super.ready()
      this[_observe]('size', this[__onSizeChanged])
      this[_compute]('orientation', 'size', this[__computeOrientation])
    }

    connectedCallback() {
      super.connectedCallback()
      this[_listen](window, 'resize', this[__onResize])
    }

    disconnectedCallback() {
      super.disconnectedCallback()
      this[_unlisten](window, 'resize')
    }

    [__onResize](e) {
      this[_debounce]('resize', () => {
        this.set('size' {
          height: window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight,
          width: window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth,
        })
      }, 200)
    }

    [__onSizeChanged]() {
      this[_fire]('size-changed')
    }

    [__computeOrientation](size) {
      if (!size) return
      return (size.width >= size.height) ? 'landscape' : 'portrait'
    }
  }

  customElement.define(WindowSize.is, WindowSize)
}())
</script>
```

## API Reference

### Methods

#### [compute](target, source, callback)

Adds a computed property binding imperatively. The callback context is automatically bound to the element. This is a specialized method to extend the standard Polymer capability. It utilizes a delegate pattern to allow a Polymer computed property to be bound to an inline function, or to a function with a computed name.

The `source` argument supports the same binding expression used with a standard computed property definition.

```js
const compute = Symbol.for('Nebula.ElementMixin.compute')
this[compute]('myProperty', 'prop1, prop2', callback) 
```

To trigger computed property handlers during initialization, add them to the constructor. To trigger computed property callbacks after element initialization, add them to the `ready` lifecycle callback.

#### [debounce](jobName, callback, delay)

Collapses multiple function calls into a single invocation with a specified delay timespan. The callback context is automatically bound to the element.

```js
const debounce = Symbol.for('Nebula.ElementMixin.debounce')
this[debounce]('myJob', calllback, 500)
```

#### [fire](eventType, detail, options)

Dispatches a `CustomEvent`.

```js
const fire = Symbol.for('Nebula.ElementMixin.fire')
this[fire]('my-event', {message: 'Hello World'}, {bubbles: true})
```

#### [listen](target, eventType, callback)

Adds an event listener to the target object. The callback context is automatically bound to the element. When the callback is invoked, `this` will be set to the element instance.

```js
const listen = Symbol.for('Nebula.ElementMixin.listen')
this[listen](this, 'tap', callback)
```

#### observe(source, calllback)

Adds a property observer imperatively. The callback function context is automatically bound to the element. This is a specialized method to extend the standard Polymer capability. It utilizes a delegate pattern to allow a Polymer property observer to be bound to an inline function, or to a function with a computed name.

The `source` argument supports the same binding expression used with a standard property observer definition.

```js
const observe = Symbol.for('Nebula.ElementMixin.observe')
this[observe]('myProp, myProp2.*, myProp3.splices', callback) 
```

To trigger observer callbacks during element initialization, add them to the constructor. To trigger observer callbacks after element initialization, add them to the `ready` lifecycle callback.

#### [unlisten](target, eventType)

Removes an event listener on the target object.

```js
const unlisten = Symbol.for('Nebula.ElementMixin.unlisten')
this[unlisten](this, 'tap')
```
