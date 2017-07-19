# ECMAScript 6 notes

A lot (all?) of these notes are taken from [Nicholas C. Zakas](https://twitter.com/slicknet)'s amazing book [Understanding ECMAScript 6](http://amzn.eu/iA7V2fr).


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

```
var x = function(condition) {
  // actually results in var x; here
  if (condition) {
    var x = 1;
  } else {
  }
}
```

Can't redeclare in same block using `let`

```
var x = 5;
var x = 5; // OK

var x = 5;
let x = 10; // Duplicate delcaration
```

Declaring variable in new block is fine

```
var x = 5;
if (true)
  let x = 10; // OK
```
## Constants

```
const value = 5; // can't be reset
```

must be initialized on declaration, since they can't be reset

```
const value; // syntax error
```

constants are *not* hoisted and are only available in the block they are declared in.

```
if (thing) { const x = 5;} // x not available outside of if block
```

can't redeclare a variable as a constant

```
var x = 5;
const x = 10; // error: x is read-only
```

constants cannot be changed, but the underlying object can

```
const obj = { a: 1 }
obj.a = 2 // OK
```

### Temporal Dead Zone (TDZ)

variables defined using let can only be accessed after they're declared.

```
for ( .. ) {
  // TDZ
  console.log(typeof x); // throws an error
  let x = 1;
}
```

```
console.log(typeof x); // undefined - Not in TDZ (not in the same block, and above the declaration) - doesn't throw error
for ( .. ) {
  let x = 1;
}
```

Use block level bindings for for loops

```
for (let x = 0; x < 10; x++) { ... }

console.log(x); // x is not defined
```
using var, the variable is hoisted so is available outside of the for block

```
for (var i = 0; i < 10; i++) { .. }

console.log(i); // prints 9
```

## functions in loops

functions in loops have access to the same reference of the variables in the loop

```
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

```
var funcs3 = [];

for (let x = 0; x < 10; x++) {
  funcs3.push(function(){ console.log(x); }); // this is pulled into another function
}

funcs3.forEach(function(func){ func(); }); // prints 0..9
```

## Global Block Bindings

```
var x = "hello";
```

in a global scope, sets `window.x` to "hello". This would overwrite any value that existed on the window already for `x`.

```
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

```
const myText = "𠜎"; // < this is just one chinese character

console.log(myText.length); // 2
console.log(myText.charAt(0)); // "�"
console.log(myText.charAt(1)); // "�"
console.log(myText.charCodeAt(0)); // 55361
console.log(myText.charCodeAt(1)); //  57102
```

in ECMAScript 6 you can use

```
codePointAt(0) > 0xFFFF
```

to determine if the character is a surrogate pair.

codePointAt returns the full code point even though the code point spans multiple code units

```
console.log(myText.codePointAt(0)); // 132878

console.log(myText.codePointAt(0) > 0xFFFF); // true
console.log("andy".codePointAt(0) > 0xFFFF); // false
```

```
String.fromCodePoint(codePoint); // gives you the full character represented by the code point

console.log(String.fromCodePoint(132878)); // 𠜎
```

### Canonical equivalence

two sequences of code points are considered interchangeable

### Compatability

two compatible sequences of codes look different, but can be used interchangeabley in some situations (ae and æ)

Normalise strings before sorting/comparing them

```
"string".normalize(); // NFC, NFD, NFKC, NFKD are available forms of normalisation
```

## Regular Expressions

JS expects strings in regex to be 16 bit code units, where each represents a single character

u flag fixes this in ECMAScript 6

u flag stops regex treating surrogate pairs like separate characters

```
/^.$/.test("𠜎") // false
/^.$/u.test("𠜎") // true
```

compares characters instead of codes

### Methods for identifying substrings

```
includes(string, optional_index)
startsWith(string, optional_index)
endsWith(string, optional_index)
repeat(times) // "x".repeat(4) -> "xxxx"
```

## backticks

Used to create multiline strings

```
const text = `hello
world`;
console.log(text);
```

String substitutions

```
const quantity = 3, price = 2.3;
console.log(`That will be ${(price * quantity).toFixed(2)}. Thanks!`);
```

## Tagged Template Literals

```
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

```
console.log(`hello\nWorld`);
// "hello
// world"
console.log(String.raw`hello\nworld`);
// "hello\\nworld"
```

literals array has a `raw` property, so can mimic this `String.raw` inside template:

```
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

```
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

```
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

```
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

```
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

```
let one = two
let two = 1
```

and since `let` declarations aren't "hoisted" two is undefined

default parameter value cannot access variables defined in the function body

## Rest Parameters

```
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

```
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

```
var sum = new Function("first", "second = first", "return first + second");
var pickFirst = new Function("...args", "return args[0]");

console.log(sum(1,2)); // 3
console.log(pickFirst(1,2,3)); // 1
```

Same capabilities as the declarative method to create functions

## Spread Operator

reduce an array into individual arguments for a function

```
let myNumbers = [1,2,3,5];
let max = Math.max(...myNumbers);

console.log(max); // 5
```
can put the spread operator at any position

```
let max = Math.max(...myNumbers, 0); // incase there are negative numbers
```

## Name property

All functions have a name property, which makes debugging easier than in ECMAScript 5

```
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
let objectLiteral = () => ({ a: 1})'
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

```js
var person = {
  name: "Andy",
  sayName: function() { console.log(`Hiya, ${this.name}`); }
};
```

```js
var person = {
  name: "Andy",
  sayName() { console.log(`Hiya, ${this.name}`); }
};
```

concise methods are able to use `super`.


