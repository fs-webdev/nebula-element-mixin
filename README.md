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
class MyElement extends Nebula.ElementMixin(Polymer.Element) {

}
```

The mixin adds back many of the utility functions provided in Polymer v1 that were removed in Polymer v2 including `listen` and `unlisten`, `fire`, and `debounce`.

### Listen and Unlisten

Use automatic node finding and the convenience methods listen and unlisten.
```js
this.listen(this.$.myButton, 'tap', this._onTap)
this.unlisten(this.$.myButton, 'tap')
```

The context of the listener callback function is automatically bound to the element, and is invoked with `this` set to the element instance.

WARNING: The callback parameter of the `listener` method takes a `function`, NOT a `string` containing the name of the callback handler. This change enables binding to methods with computed function names.

### Debounce

Debounce collapses multiple function calls into a single invocation with a specified delay timespan.

```js
this.debounce('myJob', function() { console.log('debounce') }, 500)
```

The context of the debounce function is automatically bound to the element, and is invoked with `this` set to the element instance.

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