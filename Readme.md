# array

A better array for the browser and node.js. Supports events & many functional goodies.

The functional bits are based on the [Enumerable](https://github.com/component/enumerable) component.

## Installation

### Node.js

    npm install array.js

### Browser with component

    component install matthewmueller/array

### Browser (standalone, amd, etc.)

* Development (24k): [dist/array.js](https://raw.github.com/MatthewMueller/array/master/dist/array.js)
* Production (4k w/ gzip): [dist/array.js](https://raw.github.com/MatthewMueller/array/master/dist/array.min.js)

## Examples

### Events:

```js
users.on('add', function(user) {
  console.log('added ', user);
});

user.on('remove', function(user) {
  console.log('removed ', user);
});

users.push(user);
```

### Iteration:

```js
users
  .map('friends')
  .select('age > 20')
  .map('name.first')
  .select(/^T/)
```

```js
fruits.find({ name : 'apple' }).color
```

## Design

This library uses an array-like object to implement all it's methods. This is very similar to how jQuery lets you do: `$('.modal')[0]` and `$('p').length`.

This library differs from `component/enumerable` in that it has events and does not wrap the array. To access the actual array in enumerable you had to call `.value()`. You can treat `array`, just like an array, because it implements all the same methods.

## Events

* `add` (item, ...) - emitted when items are added to the array (push, unshift, splice)
* `remove` (item, ...) - emitted when items are removed from the array (pop, shift, splice)

## API

### `array(mixed)`

Initialize an `array`.

#### Empty array:

    var arr = array();

#### Array with values:

    var arr = array([1, 2, 3, 4]);

### Array methods

`array` implements all the same methods as a native array. For more information, visit [MDN](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Array).

#### Mutators:

Mutator methods that modify the array will emit "add" and "remove" events.

* pop: Removes the last element from an array and returns that element.
* push: Adds one or more elements to the end of an array and returns the new length of the array.
* reverse: Reverses the order of the elements of an array -- the first becomes the last, and the last becomes the first.
* shift: Removes the first element from an array and returns that element.
* sort: Sorts the elements of an array.
* splice: Adds and/or removes elements from an array.
* unshift: Adds one or more elements to the front of an array and returns the new length of the array.

#### Accessors:

*concat: Returns a new array comprised of this array joined with other array(s) and/or value(s).
* join: Joins all elements of an array into a string.
* slice: Extracts a section of an array and returns a new array.
* toString: Returns a string representing the array and its elements. Overrides the Object.prototype.toString method.
* indexOf: Returns the first (least) index of an element within the array equal to the specified value, or -1 if none is found.
* lastIndexOf: Returns the last (greatest) index of an element within the array equal to the specified value, or -1 if none is found.

### Iteration Methods:

`array` iteration methods implements most of the methods of [component/enumerable](https://github.com/component/enumerable). The documentation below was originally written by [visionmedia](https://github.com/visionmedia).

#### `.each(fn)`

  Iterate each value and invoke `fn(val, i)`.

```js
 users.each(function(val, i){

 })
```

#### `.map(fn|str)`

  Map each return value from `fn(val, i)`.

  Passing a callback function:

```js
 users.map(function(user){
   return user.name.first
 })
```


  Passing a property string:

```js
 users.map('name.first')
```

#### `.select(fn|str)`

  Select all values that return a truthy value of `fn(val, i)`. The argument passed in can either be a function or a string.

```js
 users.select(function(user){
   return user.age > 20
 })
```

  With a property:

```js
 items.select('complete')
```

  With a condition:

```js
  users.select('age > 20')
```

#### `.unique()`

  Select all unique values.

```js
 nums.unique()
```

#### `.reject(fn|str|mixed)`

  Reject all values that return a truthy value of `fn(val, i)`.

  Rejecting using a callback:

```js
 users.reject(function(user){
   return user.age < 20
 })
```


  Rejecting with a property:

```js
 items.reject('complete')
```


  Rejecting values via `==`:

```js
 data.reject(null)
 users.reject(toni)
```

#### `.compact()`

  Reject `null` and `undefined`.

```js
 [1, null, 5, undefined].compact()
 // => [1,5]
```

#### `.find(fn)`

  Return the first value when `fn(val, i)` is truthy,
  otherwise return `undefined`.

```js
 users.find(function(user){
   return user.role == 'admin'
 })
```

#### `.findLast(fn)`

  Return the last value when `fn(val, i)` is truthy,
  otherwise return `undefined`.

```js
 users.findLast(function(user){
   return user.role == 'admin'
 })
```

#### `.none(fn|str)`

  Assert that none of the invocations of `fn(val, i)` are truthy.

  For example ensuring that no pets are admins:

```js
 pets.none(function(p){ return p.admin })
 pets.none('admin')
```

#### `.any(fn)`

  Assert that at least one invocation of `fn(val, i)` is truthy.

  For example checking to see if any pets are ferrets:

```js
 pets.any(function(pet){
   return pet.species == 'ferret'
 })
```

#### `.count(fn)`

  Count the number of times `fn(val, i)` returns true.

```js
 var n = pets.count(function(pet){
   return pet.species == 'ferret'
 })
```

#### `.indexOf(obj)`

  Determine the indexof `obj` or return `-1`.

#### `.has(obj:Mixed)`

  Check if `obj` is present in this enumerable.

#### `.reduce(fn, mixed)`

  Reduce with `fn(accumulator, val, i)` using
  optional `init` value defaulting to the first
  enumerable value.

#### `.max(fn|str)`

  Determine the max value.

  With a callback function:

```js
 pets.max(function(pet){
   return pet.age
 })
```


  With property strings:

```js
 pets.max('age')
```


  With immediate values:

```js
 nums.max()
```

#### `.sum(fn|str)`

  Determine the sum.

  With a callback function:

```js
 pets.sum(function(pet){
   return pet.age
 })
```


  With property strings:

```js
 pets.sum('age')
```


  With immediate values:

```js
 nums.sum()
```

#### `.first([mixed])`

  Return the first value, or first `n` values. If you pass in an object or a function, first will call `find(fn)`.

#### `.last([mixed])`

  Return the last value, or last `n` values.

## License

(The MIT License)

Copyright (c) 2013 Matt Mueller <mattmuelle@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
