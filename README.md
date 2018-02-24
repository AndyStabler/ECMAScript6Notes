# ECMAScript 6 notes

A lot (all?) of these notes are taken from [Nicholas C. Zakas](https://twitter.com/slicknet)'s amazing book [Understanding ECMAScript 6](http://amzn.eu/iA7V2fr).

## Contents

* [Naming - the most confusing part](#naming---the-most-confusing-part)
* [Block bindings](#block-bindings)
  * [Let](#let)
* [Constants](#constants)
* [Temporal dead zone (TDZ)](#temporal-dead-zone-tdz)
* [Functions in loops](#functions-in-loops)
* [Global block bindings](#global-block-bindings)
* [Strings](#strings)
  * [Canonical equivalence](#canonical-equivalence)
  * [Compatability](#compatability)
* [Regular expressions](#regular-expressions)
  * [Methods for identifying substrings](#methods-for-identifying-substrings)
* [Backticks](#backticks)
* [Tagged template literals](#tagged-template-literals)
  * [Raw values in template literals](#raw-values-in-template-literals)
* [Functions](#functions)
* [Default parameter TDZ](#default-parameter-tdz)
* [Rest parameters](#rest-parameters)
* [Function constructor](#function-constructor)
* [Spread operator](#spread-operator)
* [Name property](#name-property)
* [Dual purpose functions (ECMAScript 5)](#dual-purpose-functions-ecmascript-5)
  * [[[construct]]](#construct)
  * [[[call]]](#call)
  * [How do you know if a function is called with `new`?](#how-do-you-know-if-a-function-is-called-with-new)
  * [New.target](#newtarget)
* [Block-level functions](#block-level-functions)
* [Arrow functions](#arrow-functions)
  * [Examples](#examples)
  * [Immediately invoked function expressions (IIFE)](#immediately-invoked-function-expressions-iife)
  * [No `this` binding](#no-this-binding)
  * [No `arguments` binding](#no-arguments-binding)
* [Objects](#objects)
  * [Object literal syntax extensions](#object-literal-syntax-extensions)
* [Property initializer shorthand](#property-initializer-shorthand)
  * [Concise methods](#concise-methods)
    * [Old way](#old-way)
    * [Concise way](#concise-way)
  * [Computed property names](#computed-property-names)
* [New object methods](#new-object-methods)
  * [`object.is`](#objectis)
  * [`object.assign`](#objectassign)
* [Duplicate object literal properties](#duplicate-object-literal-properties)
* [Changing an object's prototype](#changing-an-objects-prototype)
  * [Super](#super)
* [Destructuring](#destructuring)
  * [Object destructuring](#object-destructuring)
    * [Destructuring assignment](#destructuring-assignment)
    * [Default values](#default-values)
    * [Assigning to different local varaible names](#assigning-to-different-local-varaible-names)
    * [Nested object destructuring](#nested-object-destructuring)
  * [Array destrutcuring](#array-destrutcuring)
    * [Destructuring assinment](#destructuring-assinment)
    * [Default values](#default-values)
    * [Nested array destructuring](#nested-array-destructuring)
    * [Rest items](#rest-items)
    * [Cloning arrays](#cloning-arrays)
  * [Mixed destructuring (objects and arrays)](#mixed-destructuring-objects-and-arrays)
  * [Destructuring in parameters](#destructuring-in-parameters)
* [Symbols and symbol properties](#symbols-and-symbol-properties)
  * [Creating symbols](#creating-symbols)
  * [Identifying symbols](#identifying-symbols)
  * [Using symbols](#using-symbols)
  * [Sharing symbols](#sharing-symbols)
  * [Symbol coercion](#symbol-coercion)
  * [Retrieving symbol properties](#retrieving-symbol-properties)
  * [Exposing internal operations with well-known symbols](#exposing-internal-operations-with-well-known-symbols)
    * [Symbol.hasinstance](#symbolhasinstance)
    * [Symbol.isconcatspreadable](#symbolisconcatspreadable)
    * [Regex well-known symbols](#regex-well-known-symbols)
    * [Symbol.toprimitive](#symboltoprimitive)
    * [Symbol.tostringtag](#symboltostringtag)
    * [Symbol.unscopables](#symbolunscopables)
* [Sets and maps](#sets-and-maps)
  * [Sets](#sets)
    * [Foreach](#foreach)
  * [Converting a set to an array](#converting-a-set-to-an-array)
  * [Weak sets](#weak-sets)
  * [Maps](#maps)
    * [Map initialisation](#map-initialisation)
    * [For each](#for-each)
  * [Weak maps](#weak-maps)
* [Iterators and generators](#iterators-and-generators)
  * [Iterators](#iterators)
  * [Generators](#generators)
  * [Iterables](#iterables)
  * [Accessing the default iterator](#accessing-the-default-iterator)
  * [Creating iterables](#creating-iterables)
* [Built-in iterators](#built-in-iterators)
  * [Collection iterators](#collection-iterators)
  * [String iterators](#string-iterators)
* [Spread operator and nonarray iterables](#spread-operator-and-nonarray-iterables)
* [Advanced Iterator Functionality](#advanced-iterator-functionality)
  * [Passing arguments to Iterators](#passing-arguments-to-iterators)
  * [Throwing errors in Iterators](#throwing-errors-in-iterators)
  * [Return statement in Generators](#return-statement-in-generators)
  * [Delegating Generators](#delegating-generators)
* [Classes](#classes)
  * [Anonymous Class Expressions](#anonymous-class-expressions)
  * [Named Class Expressions](#named-class-expressions)
  * [Classes as first-class citizens](#classes-as-first-class-citizens)
  * [Accessor properties](#accessor-properties)
  * [Computed method names](#computed-method-names)
  * [Generator Methods](#generator-methods)
  * [Static Members](#static-members)
  * [Inheritance with Derived Classes](#inheritance-with-derived-classes)
  * [Shadowing (Overriding) Methods](#shadowing-overriding-methods)
  * [Inherited Static Members](#inherited-static-members)
  * [Derived Classes from Expressions](#derived-classes-from-expressions)
  * [Inheriting from Built-Ins](#inheriting-from-built-ins)
  * [The Symbol.species property](#the-symbolspecies-property)
  * [new.target (in a class constructor)](#newtarget-in-a-class-constructor)
* [Arrays](#arrays)
  * [Array.of](#arrayof)
  * [Array.from](#arrayfrom)
    * [Mapping](#mapping)
    * [Iterables](#iterables-1)
  * [New methods on Arrays](#new-methods-on-arrays)
    * [find](#find)
    * [findIndex](#findindex)
    * [fil](#fill)
    * [copyWithin](#copywithin)
    * [Typed Arrays](#typed-arrays)
    * [Numeric data types](#numeric-data-types)
    * [Array Buffer](#array-buffer)
    * [Manipulating Array Buffers with Views](#manipulating-array-buffers-with-views)
    * [Retrieving View information](#retrieving-view-information)
    * [Reading/Writing Data](#readingwriting-data)
    * [Typed Arrays are views](#typed-arrays-are-views)
    * [Creating Type Specific Views](#creating-type-specific-views)
    * [Similarities Between Typed and Regular Arrays](#similarities-between-typed-and-regular-arrays)
    * [Differences between Typed and Regular Arrays](#differences-between-typed-and-regular-arrays)\
* [Promises and Asynchronous Programming](#promises-and-asynchronous-programming)
  * [Promise Basics](#promise-basics)
    * [The Promise Life Cycle](#the-promise-life-cycle)


## Naming - The most confusing part

ECMAScript is defined in the ECMA-262 standard

JavaScript used in browsers is a superset of ECMAScript -- it adds extra stuff to handle the DOM

ECMAScript 3 was introduced in 1999

in 2007 TC-39 (the committee responsible for developing ECMAScript) started draft for ECMAScript 4. It proposed *lots*, like syntax changes, modules, classical inheritance.

Folk thought it was trying to acheive too much, so an alternative, slimmed down, version was proposed -- this is ECMAScript 3.1.

Operation Harmony -- In 2008 decision made to focus on ECMAScript 3.1 first, then work on introducing features proposed in ECMAScript 4.

ECMAScript 3.1 standardised as ECMAScript 5 (ECMAScript 4 never released to avoid confusion with failed previous proposal)

Work then began on ECMAScript Harmony. ECMAScript 6 was the child of this project, released in 2015.

ECMAScript 6 == ECMAScript 2015.

ECMAScript introduces a heck load of new stuff
  * patterns
  * syntax
  * modules

## Block Bindings

Declare bindings not accessible out of a block

### let

let == var, but limits scope to only current block - prevents variable hoisting occuring from

```js
var x = function(condition) {
  // actually results in var x; here
  if (condition) {
    var x = 1;
  } else {
  }
}
```

Can't redeclare in same block using `let`

```js
var x = 5;
var x = 5; // OK

var x = 5;
let x = 10; // Duplicate delcaration
```

Declaring variable in new block is fine

```js
var x = 5;
if (true)
  let x = 10; // OK
```
## Constants

```js
const value = 5; // can't be reset
```

must be initialized on declaration, since they can't be reset

```js
const value; // syntax error
```

constants are *not* hoisted and are only available in the block they are declared in.

```js
if (thing) { const x = 5;} // x not available outside of if block
```

can't redeclare a variable as a constant

```js
var x = 5;
const x = 10; // error: x is read-only
```

constants cannot be changed, but the underlying object can

```js
const obj = { a: 1 }
obj.a = 2 // OK
```

## Temporal Dead Zone (TDZ)

variables defined using let can only be accessed after they're declared.

```js
for ( .. ) {
  // TDZ
  console.log(typeof x); // throws an error
  let x = 1;
}
```

```js
console.log(typeof x); // undefined - Not in TDZ (not in the same block, and above the declaration) - doesn't throw error
for ( .. ) {
  let x = 1;
}
```

Use block level bindings for for loops

```js
for (let x = 0; x < 10; x++) { ... }

console.log(x); // x is not defined
```
using var, the variable is hoisted so is available outside of the for block

```js
for (var i = 0; i < 10; i++) { .. }

console.log(i); // prints 9
```

## functions in loops

functions in loops have access to the same reference of the variables in the loop

```js
var funcs = [];

for(var i = 0; i < 10; i++) {
  funcs.push(function() {
    console.log("first: " + i)
  });
};

funcs.forEach(function(func){ func(); }); // prints 10 10 times

var funcs2 = [];

for(var i = 0; i < 10; i++) {
  funcs2.push((function(i){
    return function() { console.log("second: " + i); }
  })(i));// IIFE (immediately invoked function expression has i passed in. it creates copy and stores i)
};

funcs2.forEach(function(func){ func(); }); // prints 0...9
```

This ^ looks complicated and block level scoping fixes this. It creates a fresh copy of the variable.

Using let pulls the loop function into a function called `_loop` and invokes it immediately, just like the IIFE, but this is hidden.

```js
var funcs3 = [];

for (let x = 0; x < 10; x++) {
  funcs3.push(function(){ console.log(x); }); // this is pulled into another function
}

funcs3.forEach(function(func){ func(); }); // prints 0..9
```

## Global Block Bindings

```js
var x = "hello";
```

in a global scope, sets `window.x` to "hello". This would overwrite any value that existed on the window already for `x`.

```js
let x = "hello";
```

in global scope creates binding in global scope, but no property is added to the `window` object.

You *can't* overwrite a global variable using let/const, you can only *shadow* it.


STANDARD: instead of using var everywhere, use const as a default. Only use let if you know the object needs to change.

## Strings

ECMAScript 5 (ECMAScript 3.1) could only handle UTF-16 code points. 

UTF-16 contains 2^16 16 bit code points -- the Basic Multilingual Plane (BMP).

Everything beyond 2^16 is the Supplementary Plane.
These can't be represented using 16 bits, so UTF joins them together to create *surrogate pairs*

Any "single" character can be either one 16 bit character on the BMP, or two 16 bit characters on the Supplementary Plane.

THIS IS WEIRD AND NOT HANDLED WELL IN ECMAScript 5

for example:

```js
const myText = "𠜎"; // < this is just one chinese character

console.log(myText.length); // 2
console.log(myText.charAt(0)); // "�"
console.log(myText.charAt(1)); // "�"
console.log(myText.charCodeAt(0)); // 55361
console.log(myText.charCodeAt(1)); //  57102
```

in ECMAScript 6 you can use

```js
codePointAt(0) > 0xFFFF
```

to determine if the character is a surrogate pair.

codePointAt returns the full code point even though the code point spans multiple code units

```js
console.log(myText.codePointAt(0)); // 132878

console.log(myText.codePointAt(0) > 0xFFFF); // true
console.log("andy".codePointAt(0) > 0xFFFF); // false
```

```js
String.fromCodePoint(codePoint); // gives you the full character represented by the code point

console.log(String.fromCodePoint(132878)); // 𠜎
```

### Canonical equivalence

two sequences of code points are considered interchangeable

### Compatability

two compatible sequences of codes look different, but can be used interchangeabley in some situations (ae and æ)

Normalise strings before sorting/comparing them

```js
"string".normalize(); // NFC, NFD, NFKC, NFKD are available forms of normalisation
```

## Regular Expressions

JS expects strings in regex to be 16 bit code units, where each represents a single character

u flag fixes this in ECMAScript 6

u flag stops regex treating surrogate pairs like separate characters

```js
/^.$/.test("𠜎") // false
/^.$/u.test("𠜎") // true
```

compares characters instead of codes

### Methods for identifying substrings

```js
includes(string, optional_index)
startsWith(string, optional_index)
endsWith(string, optional_index)
repeat(times) // "x".repeat(4) -> "xxxx"
```

## backticks

Used to create multiline strings

```js
const text = `hello
world`;
console.log(text);
```

String substitutions

```js
const quantity = 3, price = 2.3;
console.log(`That will be ${(price * quantity).toFixed(2)}. Thanks!`);
```

## Tagged Template Literals

```js
function myTag(literals, ...substitutions) {
  var moods = {5: "happy"};
  var str = "";
  for(let i = 0; i < substitutions.length; i++) {
    str += literals[i];
    str += moods[substitutions[i]];
  }

  str += literals[literals.length - 1];
  return str;
};
var mood = 5;
var thing = myTag`something ${mood} is happening.`;
console.log(thing);
// "something happy is happening."
```

### Raw values in Template Literals

```js
console.log(`hello\nWorld`);
// "hello
// world"
console.log(String.raw`hello\nworld`);
// "hello\\nworld"
```

literals array has a `raw` property, so can mimic this `String.raw` inside template:

```js
function myTag(literals, ...substitutions) {
  var str = "";
  for(let i = 0; i < substitutions.length; i++) {
    str += literals.raw[i];
    str += substitutions[i];
  }

  str += literals.raw[literals.length - 1];
  return str;
};
```

## Functions

Default parameters are supported in ES6, they weren't in ES5.

```js
// ES5
function myFunc(id, age, name) {
  age = (typeof age === "unefined") ? 21 : age;
  name = (typeof name === "undefined") ? "Andy" : name;
  // ... 
};
// ES6
function myFunc(id, age = 21, name = "Andy") {
  // ... 
};
```

Note: Passing in undefined as a parameter `myFunc(1, undefined, 21)` would cause the default value to be used.
BUT passing in `null` would not cause it to be used.

named parameters are detatched from the `arguments` object – just like in ES5 strict mode

```js
function myFunc(id, age = 21, name = "Andy") {
  console.log(arguments["age"]);
  age = 10;
  console.log(arguments["age"]);
  console.log(arguments.length); // 1
  // ...
}
```

arguments.length is 1 there because the assignment in the constructor doesn't modify the arguments array

## Default Parameter TDZ

Fine to reference parameters in the method definition after they have been initialized in the constructor

```js
function getTwo(value) {
  return value + 1;
}

function sum(one, two = getTwo(one)) {
  console.log(`${one + two}`);
};

sum(1); // 3
sum(1,1); // 2
```

the reverse isn't true though

```js
function getTwo(value) {
  return value + 1;
};

function sum(one = two, two) {
  console.log(`${one + two}`);
};

sum(1,1) // 2
sum(undefined, 1) // NaN
```

second example translates to

```js
let one = two
let two = 1
```

and since `let` declarations aren't "hoisted" two is undefined

default parameter value cannot access variables defined in the function body

## Rest Parameters

```js
function pick(object, ...keys) {
  var result = Object.create(null);

  for (let i = 0; i < keys.length; i++) {
    result[keys[i]] = object[keys[i]];
  }

  return result;
}
```

1. can only have one rest parameter
2. rest parameter has to be the last argument in the list
3. rest parameter cannot live in in object literal setter

```js
let myObj = {
  // syntax error - can only pass one value into object literal setters - rest are infinite
  set name(...value) {
    // ...
  }
}
```

Rest arguments designed to replace `arguments` object

## Function constructor

Metaprogramming in JavaScript

```js
var sum = new Function("first", "second = first", "return first + second");
var pickFirst = new Function("...args", "return args[0]");

console.log(sum(1,2)); // 3
console.log(pickFirst(1,2,3)); // 1
```

Same capabilities as the declarative method to create functions

## Spread Operator

reduce an array into individual arguments for a function

```js
let myNumbers = [1,2,3,5];
let max = Math.max(...myNumbers);

console.log(max); // 5
```
can put the spread operator at any position

```js
let max = Math.max(...myNumbers, 0); // incase there are negative numbers
```

## Name property

All functions have a name property, which makes debugging easier than in ECMAScript 5

```js
var myFunction = function myFunc() {
  // ...
};

myFunction.name; // myFunc
new Function().name // Anonymous
(function {}).name // "" < that doesn't seem to have a name?
myFunction.bind().name // "bound myFunc"
```

## Dual purpose functions (ECMAScript 5)

Internally, functions are represented as one of two methods [[Call]] or [[Construct]].

### [[Construct]]

function called with `new` means the [[Construct]] method is called.

1. object created
2. object assigned to this
3. function executes referencing the object through `this.x`
4. functions that have a [[Construct]] method are called _constructors_

### [[Call]]

function not called with `new` means the [[Call]] method is called.

1. function executed
2. any references to this references the global object

## How do you know if a function is called with `new`?

```js
function Car(colour) {
  if (this instanceof Car)
    this.colour = colour;
  else
    throw new Error("Must use new");
}

var car = new Car("red")
console.log(car.colour); // red

Car.call(car, "blue")
console.log(car.colour); // blue

console.log(Car("red").colour); // Must use new
```

Using `Car.call` broke the instanceof check. We didn't use `new`, but `this` was still set to an instance of `Car`, which meant colour was still set.

There's no way to differentiate `new Class` and `Class.apply(...)` or `Class.call(...)`.

### new.target

`new.target` is a _metaproperty_

_metaproperty_: gives meta information about a target (`new` in this case).

when a function's [[Construct]] method is called, `new.target` is filled with the target of the `new` operator (usually the constructor of the newly created object).

if [[Call]] is called, then `new.target` is not filled.

```js
function Car(colour) {
  if (typeof new.target !== "undefined")
    this.colour = colour;
  else
    throw new Error("Must use new");
}

var car = new Car("red")
console.log(car.colour); // red

Car.call(car, "blue");
console.log(car.colour); // Must use new
```

## Block-Level functions

functions can be declared inside a block, this threw an error in ECMAScript 5.

you can use `let` to prevent function definition hoisting.

ECMAScript 6 strict-mode:

```js
if (true) {
  // function hoisted
  console.log(typeof hoisted); // function
  function hoisted() {}; // doesn't throw error in ES6 ✅

  // function not hoisted with let
  // TDZ
  console.log(typeof notHoisted); // undefined
  let notHoisted = function() {};
}
```

in non-strict mode, the method definition is hoisted to the containing function or global scope if they aren't in a function.

## Arrow Functions

1. No `this`, `arguments`, or `new.target` bindings – they're defined by closest containing non-arrow function
   * this means you avoid having to keep track of `this` when creating an event handler for example.
2. can't be called with `new`
  * Arrow functions don't have a [[Construct]] method and so can't be called with `new`.
3. No `prototype`
  * no `new` binding, and so no need for `prototype`
4. Can't ~touch~ change this
5. No duplicate named parameters

### Examples

```js
let sum = (first, second) => first + second;
sum(5,1); // 6
```

```js
let singleArg = arg => arg;
singleArg(1); // 1
```

```js
let name = () => "Andy";
name(); // Andy
```

```js
let multiLine = () => {
  let x = 5;
  return x;
};
```

```js
let objectLiteral = () => ({ a: 1})
objectLiteral() // Object {a: 1}
```

```js
const arr = [5,2,3,4,1];
arry.sort((a,b) => a - b);
```

### Immediately Invoked Function Expressions (IIFE)

```js
let personModel = ((name) => {
  return {
    getName: function() {
      return name;
    }
  };
})("Andy");

personModel.getName(); // "Andy"
```

### No `this` binding

```js
let thing = {
  handler: function() {
    document.addEventListener("click", function() {
      this.thing();
    }.bind(this), false);
  },
  thing: function() {
    console.log("thing!");
  }
};
```

using `.bind(this)` creates an extra function whose `this` is bound to the current `this`.

```js
let thing = {
  handler: function() {
    document.addEventListener("click", () => this.thing(), false);
  },
  thing: function() {
    console.log("thing!");
  },
};
```

Arrow functions are "throw away" functions – not used to define new types. Use them where you'd define an anonymous function.

### No `arguments` Binding

There is `arguments` array for the arrow function, but the arrow function can access the `arguments` array of the parent function.

```js
function createFunctionValue(number) {
  return () => arguments[0];
}

const func = createFunctionValue(5);
console.log(func()); // 5
```

## Objects


[4 object categories](https://www.ecma-international.org/ecma-262/6.0/#sec-terms-and-definitions):

1. Ordinary objects
  * object has default behaviour for the essential internal methods that must be supported by all objects
2. Exotic objects
  * object does **not** have default behaviour for the essential internal methods that must be supported by all objects
  * Any object not an ordinary object is an exostic one.
3. Standard objects
  * object whose semantics are defined by ECMAScript 6 specification
4. Built-in objects
  * object specified and supplied by an ECMAScruipt implementation

### Object Literal Syntax Extensions

## Property Initializer Shorthand

No need to repeat parameters

```js

function Person(name, age) {
  return {
    name: name,
    age: age
  };
}
```

```js
function Person(name, age) {
  return { name, age };
}
```

### Concise Methods

#### Old way

```js
var person = {
  name: "Andy",
  sayName: function() { console.log(`Hiya, ${this.name}`); }
};
```

#### Concise way

```js
var person = {
  name: "Andy",
  sayName() { console.log(`Hiya, ${this.name}`); }
};
```

concise methods are able to use `super`.

### Computed Property Names

```js
let person = {};
const firstName = "first name";
person[firstName] = "Andy"; // error inn ECMAScript 5, but fine in ECMAScript 6
```

expressions are OK too now

```js
let person = {};
let prefix = "first";
person[prefix + "name"] = "Andy";
```

Use square brackets when setting a property name in object literal syntax. The square brackets indicate that the name is computed:

```js
let lastName = "last name";

let person = {
  "first name": "Andy",
  [lastName]: "Stabler"
};

console.log(person["first name"]);
console.log(person[lastName]);
```


## New Object methods

Methods added here when they don't belong anywhere else

### `Object.is`

works the same as `===` (no type coercion), exception for special cases:

```js
console.log(-0 === +0); // true
console.log(Object.is(-0, +0)); // false

console.log(NaN === NaN); // false
console.log(Object.is(NaN, NaN)); // true
```


### `Object.assign`

Used as a mixin.

```js
var Woofer = {
  woof: function() { console.log("woof"); }
};

var doggo = {};
Object.assign(doggo, Woofer);
doggo.woof();
```

## Duplicate Object Literal Properties

This caused an error in ES5 strict mode, but not in ES6

```js
var dog = {
  name: "Max",
  name: "Barney"
};
console.log(dog.name); // Barney
```

## Changing an Object's prototype

In ES5 there was no way to change a prototype.
In ES6 you can do this with the `setPrototypeOf` method.

```js
let human = {
  getGreeting() {
    return "Hiya!";
  }
};

let dog = {
  getGreeting() {
    return "Bork!";
  }
};

let friend = Object.create(human);
console.log(friend.getGreeting()); // Hiya!
console.log(Object.getPrototypeOf(friend) === human); // true

Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting()); // Bork!
console.log(Object.getPrototypeOf(friend) === dog); // true
```

prototype value is stored in the internal-only property [[prototype]]
`getPrototypeOf` returns the value of this property
`setPrototypeOf` sets the value of this property

### Super

super can be used to access an object's prototype

```js
let person = {
  getGreeting() {
    return "Hiya";
  }
};

let dog = {
  getGreeting() {
    return "Bark";
  }
};

let friend = {
  getGreeting() {
    // return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    // this can now be written as
    return super.getGreeting() + ", hi!";
  }
};

Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting());

Object.setPrototypeOf(friend, person);
console.log(friend.getGreeting());
```

Concise methods are able to use super, but not the usual methods (what's the name for these?)

```js
let friend = {
  getGreeting: function() {
    return super.getGreeting() + ", hi!"; // syntax error - Uncaught SyntaxError: 'super' keyword unexpected here
  }
}
```

`Object.getPrototypeOf` doesn't work with multiple levels of inheritance, but super does!

ECMAScript 6 formally defines "method"s, which ECMAScript 5 didn't do.

Method (in ECMAScript 6): function that has an internal `[[HomeObject]]` property. This references the object to which the method belongs.

if the method isn't assigned to an object it won't have a `[[HomeObject]] set, and so using `super` will not work.

## Destructuring

(This is my favourite bit)

Destructuring makes it super easy to extract data from complex datastructures.

It is available for _Arrays_ and _Objects_.

### Object Destructuring

Object literal on left side of operator:


```js
let andy = {
  name: "Andy",
  age: "25"
};

let {name, age} = andy;
```

`name` and `age` are now local variables

with ECMAScript 5 this would looks something like:

```js
let andy = {
  name: "Andy",
  age: "25"
};

var name = andy.name, age = andy.age;
```

which is _fine_, but this gets complicated over larger data structures.

#### Destructuring Assignment

```js
let node = {
  type: "toad",
  size: 5
};

let type = "frog";
let size = "2";

({type, size} = node);
```

We set type and size to new values.

Note how we used parenthesis around the assignment– this was to work around a syntax constraint. Curly braces indicate a block statement,
which can't appear on the left side of an assignment. The parenthesis signals that the curly brace is an expression and not a block.

Destructuring assignment can be used anywhere a value is expected:

```js
let node = {
  type: "toad",
  size: 5,
  height: 50,
  skin: "slimy"
};

function printDimensions(dimensions) {
  console.log(`dimensions are ${JSON.stringify(dimensions)}`);
};

printDimensions({ size, height } =  node);
// {type: "toad", size: 5, height: 50, skin: "slimy"}
size;
// 5
height;
// 50
skin;
// error - Uncaught ReferenceError: skin is not defined
```

See how that printed everything and not just size and height? That's because destructuring assignment evaluates to the right side
of the expression. (I didn't know this when I started writing that example ^ TIL :) )


#### Default values

```js
let node = {
  type: "toad",
  size: 5,
  height: 50,
  skin: "slimy"
};

let { type, attitude } = node;
type // toad
attitude // null

({ type, attitude = "hungry" } = node);
type // toad
attitude // hungry
```

#### Assigning to different local varaible names

```js
let node = {
  type: "toad",
  size: 5,
  height: 50,
  skin: "slimy"
};

let { type: animal, skin: texture} = node;
animal // toad
texture // slimy
```

Note that this syntax is the opposite of traditional object literal syntax (name on left of colon, value on the right).

Default values can still be used when using a different local variable name:

```js
let node = {
  type: "toad",
  size: 5,
  height: 50,
  skin: "slimy"
};

let { type: animal, attitude: personality = "hungry" } = node;
animal // toad
personality // hungry
```


#### Nested Object Destructuring

Identifier before colon in destructuring pattern is the location to inspect. The right side of the colon assigns a value.

Note you can also set the local variable in nested destructuring

```js
let node = {
  type: "toad",
  dimensions: {
    size: 5,
    height: 50
  },
  skin: "slimy"
};

let { dimensions: { size: width} } = node;
width
// 5
```

### Array Destrutcuring

Use array literal syntax instead of object literal syntax:

```js
let pets = ['toad', 'snail', 'slime'];
let [ firstFriend, secondFriend ] = pets;
firstFriend; // toad
secondFriend; // slime
```

The array is not mutated.

You can skip elements:

```js
let pets = ['toad', 'snail', 'slime'];
let [ , , friend ] = pets;
friend; // slime
```

#### Destructuring assinment

No need to use curly braces like we did with object destructuring.

```js
let pets = ['toad', 'snail', 'slime'];
let firstFriend = 'crow';
let seconFriend = 'frog';

[ firstFriend, secondFriend ] = pets;
firstFriend; // toad
secondFriend; // snail
```

Array Destructuring makes it easy to swap variables around

```js
let a = 1, b = 2;
[ a, b ] = [ b, a ];
a; // 2
b; // 1
```

#### Default values

```js
let friends = ["toad"];
let [ bestFriend, secondBestFriend = "crow" ] = friends;
bestFriend; // toad
secondBestFriend; // crow
```

#### Nested Array Destructuring

```js
let friends = ["toad", ["crow", "pigdeon"]];
let [ bestFriend, [ secondBestFriend ] ] = friends;
bestFriend; // toad
secondBestFriend; // crow
```

#### Rest Items

```js
let friends = ["toad", "crow", "snail"];
let [ bestFriend, ...otherFriends ] = friends;
bestFriend; // toad
otherFriends; // [ "crow", "snail" ]
```

#### Cloning Arrays

In ECMASCript5

```js
let friends = ["toad", "crow", "snail"];
let clonedFriends = friends.concat();
```

In ECMAScript6

```js
let friends = ["toad", "crow", "snail"];
let [ ...clonedFriends ] = friends;
```

NB: rest items must be the last item in the destructured array. You can't follow them with a comma.

### Mixed Destructuring (Objects and Arrays)

```js
let node = {
  loc: {
    start: {
      line: 1,
      column: 2
    },
    end: {
      line: 1,
      column: 4
    }
  },
  range: [0, 3]
};

let {
  loc: { start },
  range: [ startIndex ]
} = node;

start; // { line: 1, column: 2}
startIndex; // 0
```

### Destructuring in parameters

```js
function setCookie(name, value, options) { ... }
```

^ options is hidden– reading the function definition doesn't tell you know what
you should pass. (That isn't necessarily a bad thing. I think abstracting long
argument lists into an object is actually good, but in the JS case it is often
used in such a way that the object passed in isn't obvious. end of tangent)

```js
function setCookie(name, value, { secure, path, domain, expires }) { ... }
```

Note that this would throw an error if nothing was passed in for the optional arguments

```js
setCookie("Andy", 5); // error
```

missing argument evaluates to undefined, and since

```js
let { thing } = undefined;
```

throws an error, this breaks.

You can fix this by providing a default value

```js
function setCookie(name, value, { secure, path, domain, expires} = {}) { ... }
setCookie("Andy", 5); // Works ✨
```

## Symbols and Symbol Properties

Originally, symbols were introduced as a way to provide private object members.
Properties with a string name are easy to access and so symbols were used to created non-string property names. Private
names could not be detected using the usual means.

The goal of privacy was dropped, but symbols still add non-string propery names.

Symbols are a primitive type (along with strings, numbers, booleans, null, and undefined)

### Creating symbols

Symbols don't have a literal form (true for booleans, 42 for numbers)

```js
let name = Symbol();
let person = {};
person[name] = "Andy";
person[name]; // Andy
```

You can't call `new Symbol();` because it's a primitive value

Descriptions make things more readable (questionable with a good variable name) and are good practice

```js
let name = Symbol("first name");
name; // Symbol(first name)
```

Symbol's descriptions are stored internally in the `[[Description]]` property
`[[Description]]` is accessed via `.toString()` on the symbol

### Identifying Symbols

ECMAScript6 extends `typeof` to return "symbol" for symbols.

```js
let symbol = Symbol("swanky");
typeof symbol; // symbol
```

### Using Symbols

They can be used where you would use a computed property name

```js
let firstName = Symbol("first name");

let person = {
  [firstName]: "Andy"
};

// make the property read-only
Object.defineProperty(person, firstName, { writable: false });

let lastName = Symbol("last name");

// another way to make the property read-only
Object.defineProperties(person, {
  [lastName]: {
    value: "Stabler",
    writable: false
  }
});
person[firstName]; // Andy
person[firstName] = "Max";
person[firstName]; // Andy – it's read-only! 😄
```

### Sharing Symbols

To use symbols effectively you need a way of sharing them throughout your code base. The following for example
doesn't work as you'd expect.

```js
let firstName = Symbol("first name");

let person = {
  [firstName]: "Andy"
};

function getFirstName(person) {
  let firstName = Symbol("first name");
  return person[firstName];
}

let result = getFirstName(person);
typeof result; // undefined
```

✨Introducing✨

```js
Symbol.for();
```

This looks up the symbol in the global symbol registry. If it doesn't exist, then it's created and returned.

The following now works:

```js
let firstName = Symbol.for("first name"); // creates the symbol

let person = {
  [firstName]: "Andy"
};

function getFirstName(person) {
  let firstName = Symbol.for("first name"); // looks up the symbol
  return person[firstName];
};

getFirstName(person); // Andy
```

The global symbol registry is a shared environment, so make sure you add a namespace to symbols to prevent conflicts

### Symbol Coercion

Symbols can't be coerced into strings or integers – this prevents them being used accidentally used as properties
that would otherwise be expected to behave as symbols.

```js
let name = Symbol.for("name");
console.log(`symbol is ${name}`); // TypeError: Cannot convert a Symbol value to a string
```

This attempts to coerce the symbol into a string and so the error is raised.

Same thing happens if you try to coerce the symbol into a number:

```js
let name = Symbol.for("name");
name / 1; // TypeError: Cannot convert a Symbol value to a number
```

Errors are thrown for arithmetic operators, but not for logical ones. Symbols are non-empty and so are considered
to be truthy (just like every other non-empty value in JS)

```js
let name = Symbol.for("name");
name ? "truthy" : "falsy"; // truthy
```

### Retrieving symbol properties

```js
Object.getOwnPropertyNames(); // returns all properties, regardless of their enumerability.
                              // **Does not** return the symbol properties for an object
Object.getOwnPropertySymbols(); // returns array of own property symbols

let age = Symbol.for("age");

let human = {
  name: "Andy",
  [age]: 25
};

Object.getOwnPropertyNames(human); // [name]
Object.getOwnPropertySymbols(human); // [Symbol(age)]
```

### Exposing Internal Operations with Well-Known Symbols

Well-known symbols: predefined symbols representing behaviours that were previously considered internal-only

Well-known symbols are properties on the `Symbol` object, e.g. `Symbol.match`

The well-known symbols are these:

1. `Symbol.hasInstance`
2. `Symbol.isConcatSpreadable`
3. `Symbol.iterator`
4. `Symbol.match`
5. `Symbol.replace`
6. `Symbol.search`
7. `Symbol.species`
8. `Symbol.split`
9. `Symbol.toPrimitive`
10. `Symbol.toStringTag`
11. `Symbol.unscopables`

#### Symbol.hasInstance

Used by `instance of` to determine if `x` is an instance of `y`.

```js
let obj = [];
obj instanceof Array;
// equivalient to
Array[Symbol.hasInstance](obj);
```

We know that `hasInstace` is a symbol property on the Array object, so passing in the well-known `Symbol.hasInstance`
returns the `hasInstance` function.

ECMAScript6 redfines `instanceof` as an alias to `hasInstance`

It's a function so you can override it!

```js
function Slimey() {}

Object.defineProperty(Slimey, Symbol.hasInstance, {
  value: function(v) {
    return (["snail", "toad"].includes(v.name))
  }
});

let bestFriend = {
  name: "toad"
};

let secondBestFriend = {
  name: "hedgehog"
};

bestFriend instanceof Slimey; // true
secondBestFriend instanceof Slimey; // false
```

`Symbol.hasInstance` is nonwritable, nonconfigurable, and nonenumerable, so to extend it you must use
`Object.defineProperty`. (It's _open for extension, but closed more modification_)

#### Symbol.isConcatSpreadable

Array#concat concatenates two arrays. If the argument passed in is not an array, then it appends it to the end.

```js
let arr1 = [1, 2, 3, 4];
arr1.concat([5, 6, 7]); // [1, 2, 3, 4, 5, 6, 7]
arr1.concat("hiya"); // [1, 2, 3, 4, "hiya"]
```

Arrays are automatically split up into theit individual elements. In ECMAScript 6 this functionality is exposed
through the `isConcatSpreadable` symbol property.

`Symbol.isConcatSpreadable` returns a Boolean that indicates:

1. an object has a `length` property
2. an object has numeric keys
3. an object's numeric property values should be added indivudually to result of `concat`

The method isn't implemented by default on standard objects.

```js
let things = {
  0: "button",
  1: "some string",
  length: 2,
  [Symbol.isConcatSpreadable]:  true,
};

let myPocket = ["snacks"].concat(things);
myPocket; // [ 'snacks', 'button', 'some string' ]
```

#### Regex Well-known symbols

```js
Symbol.match
Symbol.replace
Symbol.search
Symbol.split
```

Default implementations for all of these symbol properties are defined on `Regexp.prototype`.

You can define these symbol properties on any objects so pattern matching can work on things other than strings!

```js
let isEgg = {
  [Symbol.match]: function(value) {
    return value === "egg" ? value : null;
  }
};

"egg".match(isEgg); // "egg"
"spoon".match(isEgg); // null

Object.defineProperty(isEgg, Symbol.replace, {
  value: function(value, replacement) {
    return value == "egg" ? "spoon" : value;
  }
});

"egg".replace(isEgg); // "spoon"
"two eggs".replace(isEgg); // "two eggs"
```

#### Symbol.toPrimitive

It's common to convert objects to a primitive value, e.g. a comparison using `==`

Previously, the primitive value was kept to the JS internals. Now it's exposed and open for extension.

`Symbol.toPrimitive` is a prototype on each standard type

`Symbol.toPrimitive` accepts one argument called `hint`, whose value is either `"number"`, `"string"`, or
`"default"`. Each defining a preference (or lack of) for the return type.

Standard behaviour is as follows:

If "number" is passed in, return `valueOf()` if it's a primitive, otherwise `toString()` if it's a primitive,
otherwise raise an error

If "string" is passed in, return `toString()` if it's a primtive, otherwise 'valueOf()` if it's a primtive,
othwesie raise an error

Here's an example of overriding `Symbol.toPrimitive`:

```js
function Temperature(degrees) {
  this.degrees = degrees;
}

Temperature.prototype[Symbol.toPrimitive] = function(hint) {
  switch(hint) {
    case "string":
      return this.degrees + "c";
    case "number":
      return this.degrees;
    case "default":
      return this.degrees + " degrees";
  }
};

new String(new Temperature(42)); // [String: '42c'] (uses string)
new Temperature(42) / 2; // 21 (uses number)
new Temperature(42) + "!"; // '42 degrees!' (uses default)
```

#### Symbol.toStringTag

Previously, passing objects between environments (between page and an iframe) resulted in knowledge of the object type was lost.
This was the reason some libraries introduced functions like:

```js
function isArray(value) {
  return Object.prototype.toString.call(value) === "[object Array]"
}
```

This was a work around. Calling `toString` on `Object.prototype` and not the array itself meant that the
*internally* defined method `[[Class]]` was called. This value could be used to determine
what an object's type was even when the object was passed between different environments.

The method `Object.prototype.toString.call(value)` can now be used to defined for custom
objects using the `Symbol.toStringTag` symbol property.

```js
function Andy() {}

let andy1 = new Andy();
Object.prototype.toString.call(andy1); // [object Object]

Andy.prototype[Symbol.toStringTag] = "Andy";

Object.prototype.toString.call(andy1); // [object Andy]
```

You can set the string tag to whatever you like, which means you can't really trust it anymore is `Andy` _really_
an `Array`?


#### Symbol.unscopables

```js
let values = [1,2,3,4];
let friends = ["toad", "crow"];

with(friends) {
  push(...values);
}

console.log(friends); // [ 'toad', 'crow', 1, 2, 3, 4 ]
```

ECMAScript 6 introduced a `values` method, and so you might expect `values` to refer to the Array's values method
and not our local variable, which would cause errors. To prevent these errors, ECMAScript 6 introduced the
`Symbol.unscopables` property.

`Symbol.unscopables` indicates which properties should not create bindings inside the `with` statement.

```js
Array.prototype[Symbol.unscopables];
// {
//   copyWithin: true,
//   entries: true,
//   fill: true,
//   find: true,
//   findIndex: true,
//   includes: true,
//   keys: true,
//   values: true
// }
```

## Sets And Maps

### Sets

Sets don't coerce values, so `5` and `"5"` can both appear in a set

Internally, Sets use `Object.is` to determine if two values are equal

```js
let mySet = new Set();

let obj1 = {};
let obj2 = {};

mySet.add(obj1);
mySet.add(obj2);

mySet.size; // 2
```

In the past, when folk created their own sets using objects this wouldn't have worked in the way you migh expect.
This is because object keys must be stored as strings, and objects converted to string are `[object Object]`.

```js
let mySet = Object.create(null);

let obj1 = { a: 1};
let obj2 = { b: 2};

mySet[obj1] = "Andy";
mySet[obj2] = "Cake";

Object.keys(mySet)
// [ '[object Object]' ]
```

Multiple calls to `.add` with a vaue that already exists in the set are ignored

```js
let mySet = new Set();

mySet.add(5);
mySet.add(5);
mySet.add("5");

mySet; // Set { 5, '5' }
```

You can pass in an array to the Set constructor if you want to remove duplicates:

```js
let mySet = new Set([1, 1, 2, 1, 5, 5, 2, 3]);
mySet; // Set { 1, 2, 5, 3 }
```

The Set constructor accepts any iteratable object as an argument (arrays, sets, and maps are all iterable by default).

```js
let mySet = new Set([1, 2, 3]);
mySet.has(1); // true

mySet.delete(1); // true
mySet; // Set { 2, 3 }
mySet.has(1); // false

mySet.clear();
mySet; // Set {}
```

#### forEach

```js
let mySet = new Set([1, 2, 3, 5]);

mySet.forEach(function(value, key, ownerSet){
  console.log(`value: ${value}, key: ${key}, owner set: ${ownerSet}`);
});

// value: 1, key: 1, owner set: [object Set]
// value: 2, key: 2, owner set: [object Set]
// value: 3, key: 3, owner set: [object Set]
// value: 5, key: 5, owner set: [object Set]
```

Note that `value` and `key` are both equal. This is to keep the function interface matching `forEach`s existing
one used for Arrays and Maps. The two values `value` and `key` are equal here because a set's key _is_ its value.

You can pass `this` into `forEach` (just like you can with arrays)

```js
function Andy() {};

Andy.prototype.communicate = function(person) {
  console.log("BARK! at " + person);
};

Andy.prototype.socialise = function(people) {
  people.forEach(function(person) {
    this.communicate(person);
  }, this);
};



let people = new Set(["Adam", "Sarah", "James", "Molly", "Adam"]);
let andy = new Andy();
andy.socialise(people);
// BARK! at Adam
// BARK! at Sarah
// BARK! at James
// BARK! at Molly


// this also could be written using an arrow function, where `this` wouldn't need to be passed in
Andy.prototype.socialise = function(people) {
  people.forEach((person) => this.communicate(person));
};

andy.socialise(people);
// BARK! at Adam
// BARK! at Sarah
// BARK! at James
// BARK! at Molly
```

### Converting a Set to an Array

```js
let friends = ["toad", "crow", "hedgehog", "toad", "crow"];

let uniqueFriends = [...new Set(friends)];
uniqueFriends; // [ 'toad', 'crow', 'hedgehog' ]
```

### Weak Sets

Weak Sets only store _weak object references_– they can only reference objects, not primitives.

_Weak Object References_ do not prevent garbage collection if they are the only remaining reference.

Strong sets do, however. The previous Set examples are all strong Sets

```js
let friends = new Set();
let toad = { name: "toad" };

friends.add(toad);

friends.size; // 1

toad = null;

friends.size; // 1

// gets original toad reference!
[...friends][0]; // { name: 'toad' }
```

Here's the same example, but with a Weak Set:

```js
let friends = new WeakSet();

let toad = { name: "toad" };

friends.add(toad);

friends.has(toad); // 1

toad = null;

friends.size; // undefined – you can't determine the size of a weakset
friends.has(toad); // false
```

Differences between Sets and WeakSets:

1. `WeakSet#add`, `#has`, `#delete` all throw an error when passed a non-object (primitives)
2. Weak Sets aren't iterable – forEach willnae work
3. Weak Sets don't expose iterators like `keys()` and `values()` so it isn't possible to determine a set's contents
4. Weak Sets don't have a `size` property

Weak Set has limited functionality to properly deal with memory.

Use a Weak Set if you just need to track object references.

### Maps

* Maps are ordered lists of key-value pairs
* The key and value can be any type
* Key equivalence uses `Object.is`, so keys of `5` and `"5"` can store different values
  * Different from using objects as maps, since they use type coercion

```js
let textures = new Map();
textures.set("toad", "slimey");
textures.set("crow", "smooth");

console.log(`toad is ${textures.get("toad")}`);
console.log(`crow is ${textures.get("crow")}`);

textures.get("non existing key"); // undefined
```

Objects can be used as keys– not possible when using objects as maps

```js
let subjects = new Map();

let andy = { name: "Andy" };

subjects.set(andy, ["Maths", "English", "Science"]);

console.log(`Andy teaches ${subjects.get(andy).join(", ")}`);
```

```js
let myMap = new Map();

let key1 = {};
let key2 = {};

// keys are not coerced– the're considered unique
myMap.set(key1, "value1");
myMap.set(key2, "value2");

console.log(`value1 is ${myMap.get(key1)}`); // value1
console.log(`value2 is ${myMap.get(key2)}`); // value2
```

```js
let textures = new Map();
textures.set("toad", "slimey");
textures.set("crow", "smooth");

textures.has("toad"); // true

textures.delete("toad");

textures.has("toad"); // false

textures.get("toad"); // undefined

textures.clear();

textures.size; // 0
```


#### Map initialisation

```js
let textures = new Map([["toad", "slimey"], ["crow", "smooth"], [42, "interesting"]]);
textures.get("toad"); // slimey
textures.get("crow"); // smooth
textures.get(42); // interesting
```

An array is used to initialize a map with multiple key-values. An array is necessary here as the keys in the map can
be any type and arrays don't do any type coercion on the keys, so the map is created correctly.

#### for each

The callback passed is called for each element in the map _int the order the elements were added to the map_, which
is slightly different behaviour to arrays (where they're iterated based on the numerical order of the index)

```js
let textures = new Map([["toad", "slimey"], ["crow", "smooth"], [42, "interesting"]]);
textures.forEach(function(value, key, ownerMap) {
  console.log(`${key}:${value}`);
  console.log(textures === ownerMap);
});
// toad:slimey
// true
// crow:smooth
// true
// 42:interesting
// true
```

### Weak Maps

Good for associating data with DOM elements – garbage collection cleans up the map
when DOM elements are no longer referenced

* They're a way to store weak object references.
* Every key must be an object
* The object references are held weakly (they don't interfere with garbage collection)
* When there are no references to a weak map key, the key-value pair is removed from the map
* Weak maps don't store weak references to the map's values
  * A weak map value that is unreferenced outside the map will prevent garbage collection
* Unordered list of key-value pairs
* Keys are non-null objects
* Values can be any type

has `set` and `get` methods

```js
let DOMObjects = new WeakMap();
let element = document.getElementById("header");
DOMObjects.set(element, "the header");
DOMObjects.get(element); // the header

// removing the element from the dom and setting the variable reference to null removes the key-value from the map
document.parentNode.removeChild(element);
element = nulll;

// weak map is empty – there is no size property on the weak map, so no way to verify :)
// there are no references to the element object, so when the garbage collector runs the memory taken by it will
// be freed
```

has `has` and `delete` methods
  * no `clear` method as that would involve enumerating the keys

```js
let toad = {name: "toad"};
let textures = new WeakMap([[toad, "slimy"]]);

textures.has(toad); // true
textures.delete(toad); // true

textures.has(toad); // false
```

## Iterators and Generators

### Iterators

Iterators increase readability and redue risk of errors by reducing the duplication of boilerplate code.

Iterators have a `next` method that returns an object. That object has a `value` and a `done` property. `done` is
true when there are no more objects left to iterate over.

```js
function createIterator(items) {
  let i = 0;

  return {
    next: function() {
      let done = i >= items.length;
      let value = done ? undefined : items[i++];
      return {
        done: done,
        value: value
      };
    }
  };
};

let numberIterator = createIterator([1,2,3,4,5]);
numberIterator.next(); // { done: false, value: 1 }
numberIterator.next(); // { done: false, value: 2 }
numberIterator.next(); // { done: false, value: 3 }
numberIterator.next(); // { done: false, value: 4 }
numberIterator.next(); // { done: false, value: 5 }
numberIterator.next(); // { done: true, value: undefined }
numberIterator.next(); // { done: true, value: undefined }
```

### Generators

Generators are functions that return an iterator

```js
function *createIterator() {
  yield 1;
  yield 2;
  yield 3;
};

let numberIterator = createIterator();
numberIterator.next(); // { value: 1, done: false }
numberIterator.next(); // { value: 2, done: false }
numberIterator.next(); // { value: 3, done: false }
numberIterator.next(); // { value: undefined, done: true }

function *createIterator(items) {
  for(let i = 0; i < items.length; i++) {
    yield items[i];
  }
};

let numberIterator2= createIterator([1,2,3]);
numberIterator2.next(); // { value: 1, done: false }
numberIterator2.next(); // { value: 2, done: false }
numberIterator2.next(); // { value: 3, done: false }
numberIterator2.next(); // { value: undefined, done: true }
```

Note: yield is only valid in the generator function– anywhere else and it's a syntax error. It's invalid
even in a nested function in the generator. yield can't cross function boundaries.

```js
function *createIterator(items) {
  items.forEach(function(item) {
    yield item; // SYNTAX ERROR
  });
};

let numberIterator3 = createIterator([1,2,3]);
numberIterator3.next();
numberIterator3.next();
numberIterator3.next();
numberIterator3.next();
```

The other syntax is

```js
let createIterator = function *(items) {
  // ...
}
```

This is a generator function expression instead of a function declaration. The function expression is anonymous.

### Iterables

An iterable is an object with a `Symbol.iterator` property set.

`Symbol.iterator` is a well-known symbol.

All collection objects (strings, arrays, sets, etc) are iterables in ECMAScript 6.

### Accessing the default iterator

You can access the default iterator for an object through `Symbol.iterator`:

```js
let name = "Andy";

let iterator = name[Symbol.iterator]();
iterator.next(); // { value: 'A', done: false }
iterator.next(); // { value: 'n', done: false }
iterator.next(); // { value: 'd', done: false }
iterator.next(); // { value: 'y', done: false }
iterator.next(); // { value: undefined, done: true }
```

### Creating Iterables

Define the `Symbol.iterator` property on an object. Objects are not iterable by default.

```js
let collection = {
  items: [],
  [Symbol.iterator]: function *() {
    // relying on the items iterator to do the work
    for(let item of this.items) {
      yield item;
    }
  }
};

collection.items.push(5);
collection.items.push(3);
collection.items.push(1);

for(let item of collection) {
  console.log(item);
}
// 5
// 3
// 1
```

## Built-in Iterators

ECMAScript 6 has a bunch of built in-operators to use.

### Collection iterators

ECMAScript 6 has the following built-in iterators for collection objects (arrays, maps, and sets):

1. `entries`- an iterator containing the key-value pairs of the collection
2. `keys` - an iterator containing the keys of the collection
3. `values` - an iterator containing the values of the collection

The default iterator for arrays and sets is `values`
The default iterator for maps is `entries`


Notes: you can use `destructuring` in `for-of` loops:

```js
let textures = new Map([["toad", "slimy"], ["crow", "smooth"]]);

for(let [animal, texture] of textures) {
  console.log(`${animal} is ${texture}`);
}
// toad is slimy
// crow is smooth
```

### String Iterators

Strings are similar to arrays in that you can use bracket notation to access characters

```js
let name = "Andy";
let first = name[0]; // A
```

However, bracket notation uses code units, and so won't work for double-byte characters:

```js
let name = "👩";
name.length; // 2

for(let i = 0; i < name.length; i++) {
  console.log(name[i]);
}
// �
// �
```

ECMAScript 6 attempts to solve this issue with the default iterator for Strings that fully supports Unicode.

```js
let name = "👩";
name.length; // 2

for(let char of name) {
  console.log(char);
}
// 👩
```

## Spread Operator and Nonarray Iterables

Using the spread operator interanlly uses the default iterator for the collection you are accessing.

It works on any iterable.

```js
let set = new Set([1, 2, 3, 4, 5, 5, 5, 6, 7, 7]);
let array = [...set]; // 1, 2, 3, 4, 5, 6, 7
```

The values are read and inserted into the array in the order in which the values are returned from the iterator

Because the spread operator can be used on any iterable it's a dandy way of convert an iterable to an array
  * Strings to arrays of code points
  * NodeList objects into an array of nodes

## Advanced Iterator Functionality

### Passing arguments to Iterators

Arguments can be passed in to the `next` method to alter the internal state of the generator.

The first time `yield` is called, the argument passed in will not be used. This is because the value passed into
`next` will be used as the result of the last `yield` expression. There is no `yield` before the first one, so the
first `yield` will not use the argument.

```js
function *createIterator() {
  let first = yield 1;
  let second = yield first + 2;
  yield second + 3;
};

let iterator = createIterator();

iterator.next(); // { value: 1, done: false }
iterator.next(5); // { value: 7, done: false }
iterator.next(2); // { value: 5, done: false }
```

### Throwing errors in Iterators

```js
function *createiterator() {
  let first = yield 1; // 1
  let second = yield first + 2; // 7. error is raised before second is assigned
  yield second + 3; // never reached
};

let iterator = createiterator();

iterator.next(); // { value: 1, done: false }
iterator.next(5); // { value: 7, done: false }
iterator.throw(new Error("boom")); // Error: boom
```

Knowing when the error is raised allows us to catch it.

```js
function *createiterator() {
  let first = yield 1;
  let second;
  try {
    second = yield first + 2; // second not assigned since error is raised
  } catch(ex) {
    second = 6;
  }
  yield second + 3;
};

let iterator = createiterator();

iterator.next(); // 1
iterator.next(5); // 7
iterator.throw(new Error("boom")); // 9
```

Note that `throw` still yields the result in this case

### Return statement in Generators

You can return early using `return` statement.

All `yields` after `return` are ignored, and once the `return` is hit, the `done` boolean is set to true on the
iterator.

```js
function *createIterator() {
  yield 1;
  return 3;
  yield 2; // never reached
};

let iterator = createIterator();
iterator.next(); // { value: 1, done: false }
iterator.next(); // { value: 3, done: true}
iterator.next(); // { value: undefined, done: true}
```


### Delegating Generators

Generators can delegate to other generators and the `yields` will be executed sequentially:

```js
function *createFriendIterator() {
  yield "Max";
  yield "Barney";
};

function *createFoodIterator() {
  yield "Soup";
  yield "Pudding";
};

function *createGoodThingsIterator() {
  yield *createFriendIterator();
  yield *createFoodIterator();
};

let iterator = createGoodThingsIterator();
iterator.next(); // { value: "Max", done: false }
iterator.next(); // { value: "Barney", done: false }
iterator.next(); // { value: "Soup", done: false }
iterator.next(); // { value: "Pudding", done: false }
iterator.next(); // { value: undefined, done: true }
```

You can use `yield *` to delegate to string iterators:

```js
function *createNameIterator() {
  yield * "Andy";
  yield * "Stabler";
};

let iterator = createNameIterator();
iterator.next(); // { value: "A", done: false }
iterator.next(); // { value: "n", done: false }
...

iterator.next(); // { value: "r", done: false }
iterator.next(); // { value: undefined, done: true }
```

## Classes

Old way:

```js
// approach called creating a custom type
// constructor function that creates a `name` property type
var Person = function(name) {
  this.name = name;
};

// sayName is assigned to the prototype – same functions shared by all instances of Person
Person.prototype.sayName = function() {
  console.log(this.name);
};

// new instance created – new object is an instance of Person and Object through prototypal inheritance
new Person("Andy");
```

New way:

```js
class Person {
  // same as Person constructor above
  constructor(name) {
    this.name = name;
  }

  // same as sayName above
  sayName() {
    console.log(this.name);
  }
}
```

* Class methods use concise method syntax, so there's no need for the `function` keyword
* Class declarations are just syntactic sugar on top of the existing custom type declarations
  * Person class creates a function, whose body is the `constructor`'s
  * The `sayName` method ends up on the Person.prototype
  * This means custom type approach and Class declarations can be mixed without too much hassle
* Class declarations are not hoisted. They're like `let` declarations and exist in the TDZ until execution reaches
  the declaration
* Code in classes is run in strict-mode – no way to opt-out
* All methods in classes are non-enumerable (won't show up in `for...in` loops
* No class methods have an internal `[[Construct]]` method, so will raise an error if called with `new`
* Calling class constructor without `new` raises an error
* You can't overwrite the class name within a class method

### Anonymous class expressions

These are functionally equivalent to class declarations

```js
let Person = class {

  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}

let person = new Person("andy");
person.sayName(); // andy
```

### Named class expressions

Like function expressions, you can name class expressions

```js
// Person2 is only known about inside the class– referencing Person2 outside the scope of the class
// will result in an error
let Person = class Person2 {
  ...
}

let person = new Person2(); // Bang
```

### Classes as first-class citizens

* can be passed into function
* returned from a function
* assigned to a variable

Like functions– they're first-class citizens

```js
function toObj(classDef) {
  return new classDef();
};

let classDef = class {
  constructor(name) {
    this.name;
  }

  sayName() {
    console.log(`Hey, ${this.name}`);
  }
}

let obj = toObj(classDef);
```

### Accessor Properties

getters and setters are now part of the language

```js
class Friend {

  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(name) {
    this._name = name;
  }
}
```


### Computed method names

Just like object literals, classes can have computed method names

```js
let methodName = "getName";

class Person {

  constructor(name) {
    this.name = name;
  }

  [methodName]() {
    return this.name;
  }
}

new Person("Andy").getName(); // Andy
```

### Generator Methods

Prepending an asterisk before a method name creates a generator (just like with object literals).

Any method can become a generator.

Useful when you have an object that represents a collection of values that you want to iterate over easily. For
example, Arrays, Sets, Maps all have several generator methods to cater to the different ways developers interact
with the collection's elements.

```js
class Person {
  *createIterator() {
    yield 1;
    yield 2;
    yield 3;
  }
}

let iterator = new Person().createIterator();
iterator.next(); // { value: 1, done: false }
iterator.next(); // { value: 2, done: false }
iterator.next(); // { value: 3, done: false }
iterator.next(); // { value: undefined, done: true }
```

### Static Members

Static members are **not** available from within an instance-- they must be accessed on the class directly.

ECMAScript 5

```js
var Person = function(name) {
  this.name = name;
};

Person.create = function(name) {
  return new Person(name);
};

var p = Person.create("Andy"); // Person { name: 'Andy' }
```

ECMAScript 6

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }

  static create(name) {
    return new Person(name);
  }
}

let person = Person.create("Andy"); // Person { name: 'Andy' }
```

### Inheritance with Derived Classes

* Use `extends` keyword to make a class inherit from another
* Use `super` in a method to call the super class's method
* Constructors must call `super` in derived classes.
  * If there is no constructor in the derived class then super will be called automatically with all arguments
  passed to the derived class's constructor.
* `super` must be called before `this` in a constructor

```js
class Rectangle {
  constructor(length, width) {
    this.length = length;
    this.width = width;
  }

  getArea() {
    return this.length * this.width;
  }
}

class Square extends Square {
  constructor(length) {
    super(length, length);
  }
}
```

### Shadowing (Overriding) Methods

```js
class Square extends Rectangle {
  constructor(length) {
    super(length, length);
  }

  // overriding/shadowing the getArea method from the Rectangle class
  getArea() {
    return this.length * this.length;
  }
}
```

### Inherited Static Members

Static members are inherited by the derived class.


```js
class Rectangle {
  ...

  static create(length, length) {
    return new Rectangle(length, length);
  }
  ...
}

class Square extends Rectangle {
  constsructor(length) {
    super(length, length);
  }
  ...
}

let square = Square.create(3, 4);
console.log(square instanceof Square); // false
console.log(square instanceof Rectangle); // true
```

### Derived Classes from Expressions

`extends` accepts an expression on its right hand side. So the value passed can be dynamic
`extends` accepts any expression that evalutes to a function with a `[[Construct]]` and a prototype.

```js
function Rectangle(length, width) {
  this.length = length;
  this.width = width;
}

Rectangle.prototype.getArea = function() {
  return this.length * this.width;
};

function getBaseClassName() {
  return Rectangle;
}

class Square extends getBaseClassName() {
  constructor(length) {
    super(length, length);
  }
}

let square = new Square(5);
square.getArea(); // 25
```

Mixins allow for a mixin approach:


```js
let SerializableMixin = {
  serialize() {
    return JSON.stringify(this);
  }
};

let AreaMixin = {
  getArea() {
    this.length * this.width;
  }
};

function mixin(...mixins) {
  let base = function() {};
  Object.assign(base.prototype, ...mixins);
  return base;
}

class Square extends mixin(SerializableMixin, AreaMixin) {
  constructor(length) {
    super();
    this.length = length;
    this.width = length;
  }
}

let x = new Square(3);
x.getArea(); // 9
x.serialize(); // '{"length":3,"width":3}'
```

### Inheriting from Built-Ins

In ECMAScript 5 `this` was set in the derived class first, then the base class constructor was called.
`this` started out as an instance of the derived class and was then decorated with properties from the base class.

In ECMAScript 6 the value of `this` is first created by the base class and then modified by the derived class
constructor. `this` has all the functionality of the base class.

```js
class MyArray extends Array {
  sayElements() {
    for(let i = 0; i < this.length; i++) {
      console.log(this[i]);
    }
  }
}

let colours = new MyArray();
colours[0] = "Cyan";
colours.length; // 1
colours.sayElements();
// Cyan
// undefined
colours.length = 0;
colours.sayElements(); // undefined
```

### The Symbol.species property

Symbol.species is a way to identify the current class.

Symbol.species always returns the current constructor function.

If you extend a class and call a method inherited from the base class that creates a new object, the new
object will have the same type as the derived class.

Use the `Symbol.species` property when you need to use `this.constructor` in a class method. Derived classes can
then override the return type easily.

```js
class MyArray extends Array {}

let array1 = new MyArray(1, 2, 3, 4);
array1 instanceof Array; // true
array1 instanceof MyArray; // true

array1.slice(1, 3) instanceof MyArray; // true
array1.slice(1, 3) instanceof Array; // true

class MyOtherArray extends Array {
  static get [Symbol.species]() {
    Array;
  }
}

let array2 = new MyOtherArray(1, 2, 3, 4);
array2 instanceof Array; // true
array2 instanceof MyOtherArray; // true
array2.slice(1, 3) instanceof MyOtherArray; // false
```

### new.target (in a class constructor)

`new.target` used to indicate how a class is being invoked.

`new.target` is set to the class that is being instantiated

```js
class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    this.length = length;
    this.width = width;
  }
}

new Rectangle(5, 8); // true

class Square extends Rectangle {
  constructor(length) {
    super(length, length);
  }
}

new Square(5); // false
```

Useful for creating abstract classes:


```js
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error("Abstract class must be implemented");
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
  }
}

new Rectangle(5, 8);
new Shape(); // Error: Abstract class must be implemented
```

## Arrays

Before ECMAScript 6 there were two ways to create arrays, the literal syntax (`[1,2,3]`) and the Array constructor
 `new Array(1, 2, 3)`.

`Array.of` and `Array.from` make it easier to create arrays in ECMAScript 6

### Array.of

The Array constructor is whacky!

```js
new Array(2); // creates an array of length 2, with no elements
new Array("2"); // creates an array containing "2"
new Array(1, 2); // creates an array with two elements (1 and 2 respectively)
```

To simplify the creation of Arrays, the `Array.of` method was introduced.

It creates an array containing the elements passed to the `of` method.

```js
Array.of(2); // [ 2 ]
Array.of("2"); // [ "2" ]
Array.of(1, 2); // [ 1, 2]
```

### Array.from

Allows you to create an array from an array-like object (`arguments` and DOM elements)

The ECMAScript 5 way:

```js
function makeArray(arrayLikeObj) {
  return Array.prototype.slice.call(arrayLikeObj);
}
```

The ECMAScript 6 way:

```js
Array.from(arrayLikeObj);
```

Array.from uses `this` inside the constructor to determine the type of array to return

#### Mapping

```js
function timesTwo() {
  return Array.from(arguments, (element) => element * 2);
}

timesTwo(1,2,3,4); // 2,  4, 6, 8
```

You can also pass in a `this` value that represents the context on which the mapping function should be executed:

```js
helper = {
  timesTwo(element) {
    return this.multiply(element, 2);
  },

  multiply(op1, op2) {
    return op1 * op2;
  }
}

function timesTwo() {
  return Array.from(arguments, helper.timesTwo, helper);
}

timesTwo(1, 2, 3, 4); // 2, 4, 6, 8
```

#### Iterables

`Array.from` can convert any object with a `Symbol.iterator` property.

```js
numbers = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  }
};

Array.from(numbers, (num) => num * 2); // 2, 4, 6
```

### New methods on Arrays

#### find

```js
[11,22,33].find((element) => element % 2 == 0); // 22
```

#### findIndex

```js
[11,22,33].findIndex((element) => element % 2 == 0); // 1
```

`find` and `findIndex` are good for finding an element that matches a condition. If you know the value you're looking
for though, stick with `IndexOf` and `lastIndexOf`

#### fill

```js
[0, 1, 2, 3].fill(4); // [4, 4, 4, 4]

// pass a start index
[1, 2, 3, 4].fill(4, 1); // [1, 4, 4, 4]

// pass an exclusice end index
[1, 2, 3, 4].fill(6, 1, 3); // [1, 6, 6, 4]

// pass a negative index to look backwards from the end of the arrau
[1, 2, 3, 4, 5].fill(6, -2); // [1, 2, 3, 6, 6]
```

#### copyWithin

Like fill, but the element to fill is

```js
[1, 2, 3, 4, 5, 6,  7].copyWithin(2, 0); // [1, 2, 1, 2, 3, 4, 5]
[1, 2, 3, 4, 5, 6,  7].copyWithin(2, 0, 2); // [1, 2, 1, 2, 5, 6, 7]
```

`fill` and `copyWithin` are useful for typed arrays

### Typed Arrays

* Designed to work with numeric types
* Introduced to give fast bitwise arithmetic
  * arithmetic was slow as numbers stored as 64 bit floating point and converted to 32 bit ints as needed
* Any number treated as an array of bits and use the familiar methods available on arays

### Numeric data types

* JS numbers are stored in IEEE 754 format – 64 bits store floating representation of the number
  * This format used to represent integers _and_ floats in JS
* Typed arrays allow storage and manipulation of eight numeric types:
  1. int8
  2. uint8
  3. int16
  4. uint16
  5. int32
  6. uint32
  7. float32
  8. float64

* Storing an int8 as a normal JS number wastes 56 bits
  * Storing numbers more efficiently is a case for _typed arrays_
* To use operations on typed arrays you'll first need to create an _Array Buffer_ to store the data

### Array Buffer

* Foundation for all typed arrays
* Memory location that contains a specified number of bytes
* Like calling `malloc` in C – allocate memory without specifying what it contains
* You can change the data in the buffer, but not the size of it once it's created

```js
let buffer = new ArrayBuffer(10); // buffer with 10 bytes
buffer.byteLength; // 10

let buffer2 = buffer.slice(4, 6); // creates a new buffer with data from bytes 4 and 5 of buffer
buffer2.byteLength; // 2
```

### Manipulating Array Buffers with Views

* Operate on (or a subset of) an array buffer
* They read/write in one of the numeric types
* `DataView` is a generic view on array buffers
* Multple views for a single array buffer means using single memory location for entire application
  instead of dynamically allocating space as needed

```js
let buffer = new ArrayBuffer(10),
    view = new DataView(buffer), // view has access to all 10 bytes
    restrictedView = new DataView(buffer, 5, 2); // access to bytes 5 and 6
```

#### Retrieving View information

Following read-only properties are available:

* `buffer` - the array buffer the view is tied to
* `byteOffset` - second argument to the view constructor (0 by default)
* `byteLength` - third argument to the view constructor (buffer's byteLength by default)

#### Reading/Writing Data

Following methods are available for each numeric data type

_Little endian format is where the least significant bit is stored first_

```js
get<dataType>(byteOffset, littleEndian) – Read a <data_type> starting at byteOffset
set<dataType>(byteOffset, value, littleEndian) - Sets a <data_type> starting at byteOffset
```

```js
let buffer = new ArrayBuffer(2); // 2 byte array
let view = new DataView(buffer);

view.setInt8(0, 5); // setting first byte to 5
view.setInt8(1, -1); // setting second byte to -1

view.getInt8(0); // 5
view.getInt8(1); // -1

// other data type methods are available through the view too:

view.getInt16(0); // (byte 1) 00000101 (byte 2) 11111111 (all together now) 0000010111111111 = 1535
```

Although other data type methods are available on `DataViews` if you just need to use one data type, then the type
specific views might be more appropriate.

#### Typed Arrays are views

* Instead of using a generic `DataView` object to interact with an array buffer, you can use a typed array when
you want to enforce the data type.

* Theres a typed array for each of the numeric types (and one extra for uint8).
  * Uint8ClampedArray - this converts numbers less than 0 to 0 and numbers greater than 255 to 255

#### Creating Type Specific Views

```js
// giving the view a buffer
let buffer = new ArrayBuffer(10),
    view1 = new Int8Array(buffer),
    view2 = new Int8Array(buffer, 5, 2);

view1.buffer === view2.buffer; // true

// giving the view the number of elements to allocate in a buffer
let ints = new Int16Array(2); // creates a view and a buffer with 2 elements (total of 4 bytes)
let floats = new Float32Array(5); // create a view and a buffer with 5 elements (total of 20 bytes)

ints.byteLength; // 4
floats.byteLength; // 20

// Pass an object to the view
let oldArray = new Int8Array(5);
let newArray = new Int8Array(oldArray); // passing a typed array to the constructor
oldArray.buffer === newArray.buffer; // false – a clone is created

// you can pass:
//   * a typed array (^)
//   * an iterable
//   * an array (or an array like object, where it behaves the same as an aray)
// a new typed array is created in each of these instances
```

Each typed array has the `BYTES_PER_ELEMENT` constant defined on the constructor and the instance:

```js
let array = new Int8Array(5);
array.BYTES_PER_ELEMENT; // 1

Float32Array.BYTES_PER_ELEMENT; // 4
```


### Similarities Between Typed and Regular Arrays

* Can access elements using an index `new Int8Array([1,2,3,4])[0]; // 1`
* A lot of the same methods are available to Typed Arrays and Arrays
  * When an array is usually returned (e.g. `array.map`) a typed array will be returned. This is because of
  `Symbol.species`
* The same iterators are available (`entries()`, `keys()`, and `values()`). This means the spread operator and
`for-loop`s are available, just like with regular arrays.
* `from` and `to` methods are available, e.g. `Int8Array.of(1, 2, 3, 4, 5); Int8Array.from([1,2,3,4,5]);`

### Differences between Typed and Regular Arrays

* Typed arrays' size cannot be altered – the `length` property is non-writable
* Typed arrays don't inherit from `Array`, e.g. `Array.isArray(new Int8Array()); // false`
* Typed arrays use type checking to make sure only the correct data is stored in the array
  * 0 is used in place of invalid elements: `new Int8Array(["Hiya"])[0]; // 0`
* Following methods are not available for typed arrays
  * `concat` - result of a concat could be uncertain (concatting arrays of different types)
  * `shift` - can change size of array
  * `pop` - can change size of array
  * `splice` - can change size of array
  * `push` - can change size of array
  * `unshift` - can change size of array
* Following extra methods are available for typed arrays
  * `set` – sets some elements of the typed array
    ```js
    let ints = new Int8Array(4);
    ints.set([2,3]);
    ints.set([5, 8], 2); // 2 is the offset
    ints.toString(); // [2, 3, 5, 8]
    ```
  * `subarray` – the opposite of `set`. It extracts part of a typed array into another typed array
    ```js
    let ints = new Int8Array([1, 3, 5, 8, 13]);
    ints.subarray(); // Int8Array [ 1, 2, 3, 4 ]
    ints.subarray(2); // Int8Array [ 5, 8, 13]
    ints.subarray(2, 3); // Int8Array [ 5 ] – the end index is exclusive
    ```

## Promises and Asynchronous Programming

* Promises are good because they mean you can avoid _callback hell_, which sounds a bit scary.
* 3 methods for asynchronous programming:

  1. event based – `button.onClick = theMethod;`
  2. callback pattern – `readFile("example.txt", function(err, contents) { ... });
  3. promises

### Promise Basics

* Function returns a promise – instead of subscribing to an event, or passing callback to a function

#### The Promise Life Cycle

Three states that promises can be in:

1. Pending (_unsettled_) – async operation hasn't completed yet
2. Fulfilled (_settled_) – async operation completed successfully
3. Rejected (_setled_) – asyn oepration failed

* An internal `[[PromiseState]]` stores the promise's state. This is internal though and can't be accessed.
* You can take some action when a promise _changes state_ using the `then()` method.
* `then()` available on all promises
  * objects that implement `then()` are called a _thenable_
  * All promises are _thenable_ s, but not all thenables area promises.

```js
let promise = readFile("example.txt");

promise.then(successFunction, errorFunction);
promise.then(successFunction);
promise.then(null, errorFunction); // this and the example below are the same
promise.catch(errorFunction);
```
* This approach clearly indicates whether the operation succeeded
  * Events don't always fire when there's an error
  * Callbacks must always check the error argument (leads to lots of duplication when in _callback hell_
* Fulfillment/rejection handlers are still executed even if it's added to the job queue after the promise has
_settled_:
  ```js
  let promise = readFile("example.txt");

  promise.then(function(contents) {
    console.log(contents);

    // even though the promise is setled, this will still be executed
    promise.then(function(contents) {
      console.log(contents);
    });
  });
  ```
* `then()` and `call()` are handlers that are put on the job queue that is strictly reserved for promises.

#### Unsettled Promises

* Create promises with the `Promise` constructor
  * Function takes a single _executor_ argument. This is the code to initialise the promise.
* An _executor_ is a function that takes two arguments– `resolve()` and `reject()`
  * `resolve()` is called when the executor finished successfully
  * `reject()` is called when the executor fails

```js
const fs = require("fs");

function readFile(filename) {
  return new Promise(function(resolve, reject) {
    // this runs immediately. When reject or resolve are hit, a job is put onto the job queue to reject/resolve
    // the promise.
    fs.readFile(filename, { encoding: "utf-8" }, function(err, contents) {
      if (err) {
        reject(err);
        return;
      }

      resolve(contents);
    });
  });
}

let promise = readFile("example.txt");
// then is called asynchronously– a job is put onto the job queue for execution later
promise.then(function(contents) {
  console.log(contents);
}, function(err) {
  console.error(err.message);
});
```

#### Settled Promises

* `Promise.resolve` takes a single argument and returns a promise in the fulfilled state
* No job scheduling takes place
* A handler must be addeded to the promise to retrieve the value
* A rejection handler added to a promise created with `resolve()` would never be called

```js
let promise = Promise.resolve(42);

promise.then(function(value) {
  console.log(value);
});
```

* `Promise.reject` does the same thing but returns a promise in the rejected state
* A resoleve handler would never be called for this promise

```js
let promise = Promise.reject(42);

promise.catch(function(value) {
  console.error(value);
});
```

##### Non-promise Thenables

* `Promise.resolve()` and `Promise.reject()` can take non-promise thenables as arguments
  * These methods create a new promise called after the `then()` function

```js
// a non-promise thenable
let thenable = {
  then(resolve, reject) {
    resolve(42);
  }
};

// this calls `thenable.then()` so a promise state can be determined
// promise state for thenable is fulfilled because `then()` calls `resolve(42)`
let promise = Promise.resolve(thenable);

promise.then(function(value) {
  console.log(value);
});

// the same applies for creating promises in a rejected state
let thenableRejection = {
  // when then is executed it creates a new promise in the rejected state
  then(resolve, reject) {
    reject(42);
  }
};

let promiseRejection = Promise.resolve(thenableRejection);
promiseRejection.catch(function(value){
  console.error(value);
});
```

* When you pass a promise to `Promise.resolve()` or `Promise.reject()` it is returned without being changed
  * This is useful for when you aren't sure if an object is a promise. Pass it to `Promise.resolve()` or
  `Promise.reject()` and it will pass through unchanged

#### Executor Errors

* All executors have an implicit try-catch, so that errors are caught and passed to the rejection handler
* If a rejection handler isnt't given, then errors are ignored - this is deprecated functionality in node.js

```js
let promise = new Promise(function(resolve, reject) {
  throw new Error("Whoops!");
});

promise.catch(function(error) {
  console.log(error.message); // Explosion
});
```

### Rejection Handling

* In node.js you can use the following event handlers:

```js
process.on("unhandledRejection", function(reason, promise) { ... });
process.on("rejectionHandled", function(promise) { ... });
```

* In the browser you can use the following event handlers:

```js
window.onunhandledrejection = function(event) {
  console.log(event.type);
  console.log(event.reason.message);
  console.log(event.promise);
}

window.onrejectionjandled = function(event) {
  console.log(event.type);
  console.log(event.reason.message);
  console.log(event.promise);
};
```

### Chaining Promises

* Chaining promises allows for complex asynchronous behaviour
* Each call to `then()` and `catch()` creates and returns another promise.
  * The new promise is resolved when the first one has been fulfilled or rejected
  * The second `then()` fulfillment handler is called after the first promise has been resolved
  ```js
  let promise = new Promise(function(resolve, reject) {
    resolve(42);
  });

  promise.then(function(value) {
    console.log("First promise " + value);
  }).then(function(){
    console.log("Finished");
  });
  ```

#### Catching Errors

```js
let promise = new Promise(function(resolve, reject) {
  resolve(42);
});

promise.then(function(value) {
  throw new Error("Whoops!");
}).catch(function(error) {
  console.error(error.message); // Whoops
});


let promise2 = new Promise(function(resolve, reject) {
  throw new Error("Whoops!");
});

promise2.then(function(value) {
  console.log(value);
}).catch(function(error) {
  console.log(error.message);
});
```

* It's a good idea to have a rejection handler at the end of a promise chain

#### Returning Values in Promise Chains

* You can pass values along in a promise chain using `return value`
* A rejection handler can take care of an error and let the rest of the chain continue

```js
let promise = new Promise(function(resolve, reject) {
  resolve(42);
});

promise.then(function(value) {
  console.log(value); // 42
  return value + 1;
}).then(function(value) {
  console.log(value); // 43
});


// same applies for rejections

let promise2 = new Promise(function(resolve, reject){
  reject(42);
});

promise2.catch(function(value) {
  console.log(value); // 42
  return value + 1;
}).then(function(value) {
  console.log(value); // 43
});
```

#### Returning Promises in Promise Chains

* Note that the second fulfillment handler is not added to p2, but to a new promise.
  * This is important because if the first fulfillment handler fails, then the second fulfillment handler
  will not be called.

```js
let p1 = new Promise(function(resolve, reject) {
  resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
  resolve(43);
});

p1.then(function(value) {
  // then() returns a new promise, whose fulfillment handler returns p2
  // then() does not return p2 here
  console.log(value); // 42
  return p2;
}).then(function(value) {
  console.log(value); // 43
});

// this is the same as

let p3 = p1.then(function(value) {
  console.log(value); // 42
  return p2;
});

p3.then(function(value) {
  console.log(value); // 43
});
```

* You can return a promise from a fulfillment handler is you only want it to execute
when another promise has successfully completed:

```js
// here the first and second fulfillment handlers are executed immediately after calling resolve(42)
let p1 = new Promise(function(resolve, reject) {
  // big calculation here
  resolve(42);
});

p2 = p1.then(function(value) {
  // first fulfillment handler
  console.log(42);
});

p2.then(function(value) {
  // second fulfillment handler
  console.log("Finished");
});

// here the second fulfillment handler is only executed after the first has completed
let p1 = new Promise(function(resolve, reject) {
  resolve(42);
});

p1.then(function(value) {
  console.log(value);
  let p2 = new Promise(function(resolve, reject) {
    resolve("After p1 has completed");
  });
  return p2;
}).then(function(value) {
  // this won't execute until p2 is fulfilled
  console.log(value);
});
```

### Responding to Multiple Promises

* Sometimes it's useful to monitor progress of multiple promises
* ECMAScript 6 offers two methods to keep track of multiple promises:
  * `Promise.all()`
  * `Promise.race()`

#### Promise.all()

* Takes an iterable of promises
* Returns a new promise that is resolved only when every promise passed in is resolved

```js
let p1 = new Promise(function(resolve, reject) {
  resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
  resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
  resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);
p4.then(function(value) {
  console.log(Array.isArray(value)); // true
  console.log(value[0]); // 42
  console.log(value[1]); // 43
  console.log(value[2]); // 44
});
```

* If any promise passed to `Promise.all()` is rejected, then the returned promise is rejected without waiting for
the others to complete:

```js
let p1 = new Promise(function(resolve, reject) {
  resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
  reject(43);
});

let p3 = new Promise(function(resolve, reject) {
  resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);
p4.catch(function(value) {
  console.log(Array.isArray(value)); // false
  console.log(value); // 43
});
```

#### Promise.race()

* Takes an iterable of promises
* Returns a promise that is settled as soon as the first of any of the promises is settled
* If the first promise to settle is fulfilled, then the returned promise is fulfilled
* If the first promise to settle is rejected, then the returned promise is rejected

```js
let p1 = Promise.resolve(42); // fulfilled promise

let p2 = new Promise(function(resolve, reject) {
  resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
  resolve(44);
});

let p4 = Promise.race([p1, p2, p3]);
p4.then(function(value) {
  console.log(value); // 42
});
```
