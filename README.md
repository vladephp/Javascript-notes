# 1. Lexical Scope
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

Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.

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

Hoisting means “moving to the beginning of a scope.” Function declarations are hoisted completely, variable declarations only partially. That allows you to call a function before it has been declared.

ES5: var - hoist

ES6: const, let - temporary dead zone


# 6. This

Every function, while executing, has a reference to its current execution context, called this.

## 4 Rules of using this order by precedence

 - Is the function called with new (new binding)? If so, this is the newly constructed object.

var bar = new foo()

 - Is the function called with call or apply (explicit binding), even hidden inside a bind hard binding? If so, this is the explicitly specified object.

var bar = foo.call( obj2 )

 - Is the function called with a context (implicit binding), otherwise known as an owning or containing object? If so, this is that context object.

var bar = obj1.foo()

 - Otherwise, default the this (default binding). If in strict mode, pick undefined, otherwise pick the global object.

var bar = foo()

# 7. Function

## Named Function Declaration

function sum(num1,num2) {

  return num1 + num2;
  
}

The "classic" declaration, which works well for simple uses. Functions declared like this are "hoisted", meaning, the javascript engine reads all these declarations first before executing any of the rest of the code. The declaration statement stands alone, and cannot be combined with other expressions. Also, notice that this declaration does not need to be followed with a semicolon.

## Function Expressions

### Variable Assignment

// Named

var sum = function add(num1,num2) {

  return num1 + num2;
  
};

// Anonymous

var sum = function(num1, num2) {

  return num1 + num2;
  
};

This is functionally equivalent to the Function Declaration above, except this variation is NOT hoisted. The name of our function (add) cannot be called directly (see below), but it can come in handy when debugging:

sum(1,1); // returns 2

add(1,1); // "add is not defined"

### Immediately invoked

// Named

(function sum(num1, num2) {

  return num1 + num2;
  
}(1, 2));

// Anonymous

(function(num1, num2) {

  return num1 + num2;
  
}(1, 2));

This function is immediately invoked, meaning that it is defined and called at the same time. The function's name is only available within its execution scope (defined by the parentheses), so it cannot be called later in the program.

sum(1,1);// "sum is not defined"

Immediately invoked functions can be used to encapsulate a program, preventing it from polluting the global namespace.

### Assigned and Invoked

// Named

var sum = function add(num1, num2) {

  return num1 + num2;
  
}(1, 2);

// Anonymous

var sum = function(num1, num2) {

  return num1 + num2;
  
}(1, 2);

This is a combination of the variable assignment expression and the immediately invoked function (both demonstrated above). One neat application for the named variety of this is to make recursive functions more readable, by substituting arguments.callee with your function name.

### Property Assignment

// Named

var obj1 = {

  sum: function add(num1, num2) {
  
    return num1 + num2
    
  }
  
};

// Anonymous

var obj2 = {

  sum: function(num1, num2) {
  
    return num1 + num2
    
  }
  
};

By assigning functions (either named or unnamed) to properties of objects, we define methods on those objects. This has many applications in object oriented programming. We can also use this to namespace our functions, and keep them out of the global scope.

Here's how we would call the methods defined in the examples:

obj1.sum(1, 2); // returns 3

obj2.sum(1, 2); // returns 3

### Passed as Argument

// Named

window.setTimeout(<strong>function add() {

  alert(1 + 2);
  
}</strong>, 500);

// Anonymous

window.setTimeout(<strong>function() {

  alert(1 + 2);
  
}</strong>, 500);

Function names in ECMAScript are nothing more than variables, meaning we can pass them around like variables. Many methods (like setTimeout()) take functions as arguments. This is a common pattern for defining callbacks.
In these examples, we define them in line, but we could also pass in predefined functions.

### Returned (closure)

Functions can be returned from other functions. This lets us do clever things like the following (picking up from the example):

var bob = {}, rob = {};

bob.count = counter();

rob.count = counter();

bob.count(); // alerts "0"

bob.count(); // alerts "1"

rob.count(); // alerts "0"

rob.count(); // alerts "1"

Each person can increment their own count variable despite the fact that it originated outside the scope of the count() function. This is called a closure.


## Function with new keyword

When we put new keyword in front any funciton call, turn that function call in consturctor call. Four things occur:

 - a brand new object is created (aka, constructed) out of thin air
 - the newly constructed object is [[Prototype]]-linked
 - the newly constructed object is set as the this binding for that function call
 - unless the function returns its own alternate object, the new-invoked function call will automatically return the newly constructed object.
 
 # 8. For vs For in vs For of vs forEach
 
 ## For
 
 var list = [8, 3, 11, 9, 6];

for (var i = 0; i < list.length; i++) {

  console.log(list[i]);
  
}

There’s nothing wrong with this approach, but it just feels like a lot of code to write these days. We have to keep track of the loop counter variable (i in the above example), tell it to increment by 1 with each iteration (i++), and control when the iteration ends (i < list.length).

var list = [8, 3, 11, 9, 6], length = list.length, i;

for (i = 0; i < length; i++) {

  console.log(list[i]);
  
}



This is because the JavaScript engine will actually hoist i to the top of the function (see the article on block-level scoping for more on variable hoisting). Also in the previous for loop, list.length gets accessed with every iteration of the loop even though it doesn’t change, so storing it in a length variable is a tad bit more efficient. This all seems overkill when all we want to do is iterate over each element of the list
 
 ## For in
 
 Use for objects only. Don't use for arrays.
 
 If you use with arrays, this can happen:
 
 Array.prototype.one = 'one';
 
 var list = [8, 3, 11, 9, 6];
 
 for (var i in list) {
 
    console.log(list[i]);
    
}

One will be show in loop, although not in the list array.

## For of

New in ES6

Use for arrays.

let list = [8, 3, 11, 9, 6];

for (let value of list) {

  console.log(value);
  
}

We get the succinct syntax of for-in, the run-to-completion of forEach, and the ability to break, continue, and return of the simple for loop.

But for-of doesn’t just work for arrays. If it did, it probably wouldn’t have been meaty enough to add to the ES6 specification. Other existing collections like the DOM NodeList object, the arguments object, and strings also work with for-of. Just like with arrays, this makes it a little bit easier to iterate over these non-array objects.

for (var char of 'Hello') {
    console.log(char);
}

// output:

// H

// e

// l

// l

// o

## forEach

var list = [8, 3, 11, 9, 6];

list.forEach(function(value, i) {

  console.log(value);
  
});

With a normal for loop you can break to end the loop early. There isn’t a way to end forEach early. Including break within the forEach callback function will be a syntax error. It is only valid within loops.

Similarly, with a for loop when we return, we are exiting out of the entire function that the for loop is in. However, putting a return within the forEach callback function just exits out of the callback function itself early. It’s actually more or less equivalent to doing continue in a for loop, but far less intuitive. Including continue in the forEach call back function would be the same sort of syntax error we got with break.

The for-in and for-of loops work with const because the loop initializer creates a new binding on each iteration through the loop rather than attempting to modify the value of an existing binding (as was the case with the previous example using for instead of for-in).


# REMARKS

 - With and Eval never use
 - Worker 
