# 1. Scope
Where a variable is available in your code

ES5: global or function scope

ES6: block scope (global, function, if, loops, switch, try catch)

# 2. Data types value
## Primitive
ES5 and ES6: Number, String, Boolean, Null, Undefined 

ES6: Symbol

## Object
ES5 and ES6: Object, Array, Function

## Array and Function are object, they have prototype
var a = {};

var b = function(){};

var c = [];

a.__proto__.

b.__proto__.

c.__proto__.

## Falsy values

if (false)

if (null)

if (undefined)

if (0)

if (NaN)

if ('')

if ("")

if (document.all) [1]

## Truthy values

if (true)

if ({})

if ([])

if (42)

if ("foo")

if (new Date())

if (-42)

if (3.14)

if (-3.14)

if (Infinity)

if (-Infinity)

## Typeof

### Numbers

typeof 37 === 'number';

typeof 3.14 === 'number';

typeof(42) === 'number';

typeof Math.LN2 === 'number';

typeof Infinity === 'number';

typeof NaN === 'number'; // Despite being "Not-A-Number"

typeof Number(1) === 'number'; // but never use this form!


###Strings

typeof '' === 'string';

typeof "bla" === 'string';

typeof (typeof 1) === 'string'; // typeof always returns a string

typeof String('abc') === 'string'; // but never use this form!


### Booleans

typeof true === 'boolean';

typeof false === 'boolean';

typeof Boolean(true) === 'boolean'; // but never use this form!


### Symbols

typeof Symbol() === 'symbol'

typeof Symbol('foo') === 'symbol'

typeof Symbol.iterator === 'symbol'


### Undefined

typeof undefined === 'undefined';

typeof declaredButUndefinedVariable === 'undefined';

typeof undeclaredVariable === 'undefined'; 


### Objects

typeof {a: 1} === 'object';

// use Array.isArray or Object.prototype.toString.call

// to differentiate regular objects from arrays

typeof [1, 2, 4] === 'object';

typeof new Date() === 'object';


### The following is confusing. Don't use!

typeof new Boolean(true) === 'object';

typeof new Number(1) === 'object';

typeof new String('abc') === 'object';


### Functions

typeof function() {} === 'function';

typeof class C {} === 'function';

typeof Math.sin === 'function';

### This stands since the beginning of JavaScript

typeof null === 'object';

typeof document.all === 'undefined';

# 3. Closure

U funkcijama dostupne su sve promenljive iz spoljasnjeg scope-a, a nisu iz unutrasnjeg scope-a.

console.dir(nazivFunkcije) - prikazuje koje sve promenljive su u njenom closure-u

# 4. Partial Function

## Example
function mul(a, b) {

  return a * b;
  
}

let double = mul.bind(null, 2);

alert( double(3) ); // = mul(2, 3) = 6

alert( double(4) ); // = mul(2, 4) = 8

alert( double(5) ); // = mul(2, 5) = 10

The call to mul.bind(null, 2) creates a new function double that passes calls to mul, fixing null as the context and 2 as the first argument. Further arguments are passed “as is”.

That’s called partial function application – we create a new function by fixing some parameters of the existing one.

Please note that here we actually don’t use this here. But bind requires it, so we must put in something like null.


## Why do we usually make a partial function?

Here our benefit is that we created an independent function with a readable name (double, triple). We can use it and don’t write the first argument of every time, cause it’s fixed with bind.

In other cases, partial application is useful when we have a very generic function, and want a less universal variant of it for convenience.

For instance, we have a function send(from, to, text). Then, inside a user object we may want to use a partial variant of it: sendTo(to, text) that sends from the current user.

# 5. Hoisting

Hoisting means “moving to the beginning of a scope.” Function declarations are hoisted completely, variable declarations only partially.
Function declarations are completely hoisted. That allows you to call a function before it has been declared.

ES5: var - hoist

ES6: const, let - temporary dead zone
