# observable

<a style="color: white; background:green;padding:5px;margin:5px;border-radius:2px" href="https://egghead.io/lessons/javascript-sync-the-ui-with-the-app-state-using-mobx-observable-and-observer-in-react">Egghead.io lesson 1: observable & observer</a>
<a style="color: white; background:green;padding:5px;margin:5px;border-radius:2px"  href="https://egghead.io/lessons/react-use-observable-objects-arrays-and-maps-to-store-state-in-mobx">Egghead.io lesson 4: observable objects & maps</a>

Usage:
* `observable(value)`
* `@observable classProperty = value`

Observable values can be JS primitives, references, plain objects, class instances, arrays and maps.
The following conversion rules are applied, but can be fine-tuned by using *modifiers*. See below.

1. If *value* is an ES6 `Map`: a new [Observable Map](map.md) will be returned. Observable maps are very useful if you don't want to react just to the change of a specific entry, but also to the addition or removal of entries.
1. If *value* is an array, a new [Observable Array](array.md) will be returned.
1. If *value* is an object *without* prototype, all its current properties will be made observable. See [Observable Object](object.md)
1. If *value* is an object *with* a prototype, a JavaScript primitive or function, a [Boxed Observable](boxed.md) will be returned. MobX will not make objects with a prototype automatically observable; as that is the responsibility of its constructor function. Use `extendObservable` in the constructor, or `@observable` in its class definition instead.

These rules might seem complicated at first sight, but you will notice that in practice they are very intuitive to work with.
Some notes:
* To create **dynamically keyed objects** use an [Observable Map](map.md)! Only initially existing properties on an object will be made observable, although new ones can be added using `extendObservable`.
* To use the `@observable` decorator, make sure that [decorators are enabled](observable-decorator.md) in your transpiler (babel or typescript).
* By default, making a data structure observable is *infective*; that means that `observable` is applied automatically to any value that is contained by the data structure, or will be contained by the data structure in the future. This behavior can be changed by using *modifiers*.

Some examples:

```javascript
const map = observable.map({ key: "value"});
map.set("key", "new value");

const list = observable([1, 2, 4]);
list[2] = 3;

const person = observable({
    firstName: "Clive Staples",
    lastName: "Lewis"
});
person.firstName = "C.S.";

const temperature = observable(20);
temperature.set(25);
```
