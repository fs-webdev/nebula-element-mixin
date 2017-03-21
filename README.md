# \<nebula-element-mixin\>

A set of custom element utility methods.

## Installation

```sh
$ bower install -S arsnebula/nebula-element-mixin
```

## Usage

Import the package.

```html
<link rel="import" href="/bower_components/nebula-element-mixin/nebula-element-mixin.html"> 
```

Add the mixin to an element.

```js
class MyElement extends Nebula.ElementMixin(Polymer.Element) {}
```

The mixin adds utility functions similar to those provided in Polymer v1, that were removed in Polymer v2 including `listen` and `unlisten`, `fire`, and `debounce`. It also provides the ability to define property observers and computed properties imperatively using `observe` and `compute`.

### Observe

Use the `observe` method allows adding a property observer imperatively. Each observer must include a string expression containing one or more properties (can include wildcards and splices), and a callback function. The callback function context is automatically bound to the element.

```js
constructor() {
  super()
  this.observe('myProp, myProp2.*, myProp3.splices', this._onDataChanged) 
}
```

To trigger observer callbacks during element initialization, add them to the constructor. To trigger observer callbacks after element initialization, add them to the `ready` lifecycle callback.

### Compute

Use the `compute` method adds a computed property binding imperatively. The callback context is automatically bound to the element.

```js
constructor() {
  super()
  this.compute('myProperty', 'prop1, prop2', this._computeMyProperty) 
} 
```

To trigger computed property handlers during initialization, add them to the constructor. To trigger computed property callbacks after element initialization, add them to the `ready` lifecycle callback.

### Listen and Unlisten

The 'listen' and 'unlisten' methods add and remove event listeners with the context automatically bound to the element. When the callback invoked, `this` will be set to the element instance.

```js
this.listen(this, 'tap', this._onTap)
this.unlisten(this, 'tap')
```

### Debounce

Debounce collapses multiple function calls into a single invocation with a specified delay timespan. The callback context is automatically bound to the element.

```js
this.debounce('myJob', function() { console.log('debounce') }, 500)
```

### Fires

The fire method dispatches a `CustomEvent`.

```js
this.fire('my-event', {message: 'Hello World'}, {bubbles: true})
```

*For more information on element properties and methods see the element API documentation.*

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Change Log

See [CHANGELOG](/CHANGELOG.md)

## License

See [LICENSE](/LICENSE.md)