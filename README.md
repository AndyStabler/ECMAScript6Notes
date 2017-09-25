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
