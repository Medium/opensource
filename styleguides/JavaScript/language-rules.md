# JavaScript Language Rules #

## var ##

**Declarations with var: Always**

When you fail to specify var, the variable gets placed in the global context, potentially clobbering
existing values. Also, if there's no declaration, it's hard to tell in what scope a variable lives
(e.g., it could be in the Document or Window just as easily as in the local scope). So always
declare with var.

Mulit-line `var` statements are acceptable, but avoid complex logic.

```js
var stack = []
  , readCursor = 0
  , writeCursor = 0
  , root = this.getParentElement()
```


## Constants ##

**Use NAMES_LIKE_THIS for constants**

For simple primitive value constants, the naming convention is enough:

```js
/**
 * The number of seconds in a minute.
 * @type {number}
 */
example.SECONDS_IN_A_MINUTE = 60
```

For non-primitives, use the @const annotation.

```js
/**
 * The number of seconds in each of the given units.
 * @type {Object.<number>}
 * @const
 */
example.SECONDS_TABLE = {
  minute: 60,
  hour: 60 * 60
  day: 60 * 60 * 24
}
```

This allows the compiler to enforce constant-ness.

Avoid the `const` keyword, Internet Explorer doesn't parse it, so don't use it.

`obv.namespace.ClassName.CONSTANT = 'foo'` is preferred over
`obv.namespace.ClassName.prototype.CONSTANT = 'foo'` since the compiler can collapse the first down
to a couple of characters, where as the latter would always be referred to as `this.ab` once
compiled.


## Semicolons ##

**Only use semicolons when absolutely necessary**

According to the spec, statements in JavaScript should be delimited with a semi-colon, however the
spec also outlines a error-recovery mode known as "Automatic Semicolon Insertion".  A growing
community of JS developers have chosen to make use of ASI to reduce visual clutter in code, Obvious
is following this approach.  You can [read more about ASI here](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding).

There are a couple places where missing semicolons are particularly dangerous, either avoid these
cases or prefix lines with a `(` or `[` with a semi:

```js
// 1.
MyClass.prototype.myMethod = function() {
  return 42
}  // No semicolon here.

(function() {
  // Some initialization code wrapped in a function to create a scope for locals.
})()

var x = {
    'i': 1
  , 'j': 2
}  // No semicolon here.

// 2.  Trying to do one thing on Internet Explorer and another on Firefox.
// I know you'd never write code like this, but throw me a bone.
[normalVersion, ffVersion][isIE]()


var THINGS_TO_EAT = [apples, oysters, sprayOnCheese]  // No semicolon here.

// 3. conditional execution a la bash
-1 == resultOfOperation() || die()
```


## Nested functions ##

**Yes**

Nested functions can be very useful, for example in the creation of continuations and for the task
of hiding helper functions. Feel free to use them.


## Function Declarations Within Blocks ##

**No**

Do not do this:

```js
if (x) {
  function foo() {}
}
```

or this:

``` js
for (var i = 0; i < len; i++) {
  function foo() {}
}
```

While most script engines support Function Declarations within blocks it is not part of ECMAScript
(see ECMA-262, clause 13 and 14). Worse implementations are inconsistent with each other and with
future EcmaScript proposals. ECMAScript only allows for Function Declarations in the root statement
list of a script or function. Instead use a variable initialized with a Function Expression to
define a function within a block:

```js
if (x) {
  var foo = function() {}
}
```


## Exceptions / Errors ##

**Yes**

You basically can't avoid exceptions if you're doing something non-trivial. Go for it.


## Custom exceptions ##

**Absolutely, Yes**

Without custom exceptions, returning error information from a function that also returns a value can
be tricky, not to mention inelegant. Bad solutions include passing in a reference type to hold error
information or always returning Objects with a potential error member. These basically amount to a
primitive exception handling hack. Feel free to use custom exceptions when appropriate.


## Standards features ##

**Always preferred over non-standards features**

For maximum portability and compatibility, always prefer standards features over non-standards
features. For example `string.charAt(3)` over `string[3]` and element access with DOM functions instead of
using an application-specific shorthand. Or `document.getElementById('x')` over `$('#x')`


## Wrapper objects for primitive types ##

**No**

There's no reason to use wrapper objects for primitive types, plus they're dangerous:

```js
var x = new Boolean(false)
if (x) {
  alert('hi')  // Shows 'hi'.
}
```

Don't do it!

However type casting is fine.

```js
var x = Boolean(0)
if (x) {
  alert('hi')  // This will never be alerted.
}
typeof Boolean(0) == 'boolean'
typeof new Boolean(0) == 'object'
```

This is very useful for casting things to number, string and boolean.


## Inheritance ##

**Yes**

Use Closure's `goog.inherits` in client code and Node's `util.inherits` for server code.  See
separate doc on [JS inheritance](inheritance.md) for further explanation.


## Method definitions ##

While there are several methods for attaching methods and properties to a constructor, the preferred style is:

```js
Foo.prototype.bar = function () {
  /* ... */
}
```


## Closures ##

**Yes, but be careful**

The ability to create closures is perhaps the most useful and often overlooked feature of JS. Here
is [a good description of how closures work](http://jibbering.com/faq/faq_notes/closures.html).

One thing to keep in mind, however, is that a closure keeps a pointer to its enclosing scope, which
can be the source of memory leaks.


## eval() ##

**Avoid if possible, use JSON where supported**

eval() makes for confusing semantics and is dangerous to use if the string being eval()'d contains
user input. There's usually a better, more clear, safer way to write your code, so its use is
generally not permitted. However eval makes deserialization considerably easier than the non-eval
alternatives, so its use is acceptable for this task (for example, to evaluate RPC responses).


## with() {} ##

**No**

Using `with` clouds the semantics of your program. Because the object of the with can have properties
that collide with local variables, it can drastically change the meaning of your program. For
example, what does this do?

```js
with (foo) {
  var x = 3
  return x
}
```

Answer: anything. The local variable x could be clobbered by a property of foo and perhaps it even
has a setter, in which case assigning 3 could cause lots of other code to execute. Don't use with.


## this ##

**Only in object constructors, methods, and in setting up closures**

The semantics of this can be tricky. At times it refers to the global object (in most places), the
scope of the caller (in eval), a node in the DOM tree (when attached using an event handler HTML
attribute), a newly created object (in a constructor), or some other object (if function was
`call()`ed or `apply()`ed).

Because this is so easy to get wrong, limit its use to those places where it is required:

- in constructors
- in methods of objects (including in the creation of closures)


## for-in loop ##

**Only for iterating over keys in an object/map/hash**

for-in loops are often incorrectly used to loop over the elements in an Array. This is however very
error prone because it does not loop from 0 to length - 1 but over all the present keys in the
object and its prototype chain. Here are a few cases where it fails:

```js
function printArray(arr) {
  for (var key in arr) {
    print(arr[key])
  }
}

printArray([0, 1, 2, 3])  // This works.

var a = new Array(10)
printArray(a)  // This is wrong.

a = document.getElementsByTagName('*')
printArray(a)  // This is wrong.

a = [0, 1, 2, 3]
a.buhu = 'wine'
printArray(a)  // This is wrong again.

a = new Array
a[3] = 3
printArray(a)  // This is wrong again.
```

Always use normal for loops when using arrays.

```js
function printArray(arr) {
  var l = arr.length
  for (var i = 0; i < l; i++) {
    print(arr[i])
  }
}
```


## Associative Arrays ##

**Never use Array as a map/hash/associative array**

Associative Arrays are not allowed... or more precisely you are not allowed to use non number
indexes for arrays. If you need a map/hash use Object instead of Array in these cases because the
features that you want are actually features of Object and not of Array. Array just happens to
extend Object (like any other object in JS and therefore you might as well have used Date, RegExp or
String).


## Multiline string literals ##

**No**

Do not do this:

```js
var myString = 'A rather long string of English text, an error message \
                actually that just keeps going and going -- an error \
                message to make the Energizer bunny blush (right through \
                those Schwarzenegger shades)! Where was I? Oh yes, \
                you\'ve got an error and all the extraneous whitespace is \
                just gravy.  Have a nice day.'
```

The whitespace at the beginning of each line can't be safely stripped at compile time; whitespace
after the slash will result in tricky errors; and while most script engines support this, it is not
part of ECMAScript.

Use string concatenation instead:

```js
var myString = 'A rather long string of English text, an error message ' +
    'actually that just keeps going and going -- an error ' +
    'message to make the Energizer bunny blush (right through ' +
    'those Schwarzenegger shades)! Where was I? Oh yes, ' +
    'you\'ve got an error and all the extraneous whitespace is ' +
    'just gravy.  Have a nice day.'
```

Or better yet a template.



## Array and Object literals ##

**Yes**

Use Array and Object literals instead of Array and Object constructors.

Array constructors are error-prone due to their arguments.

```js
// Length is 3.
var a1 = new Array(x1, x2, x3)

// Length is 2.
var a2 = new Array(x1, x2)

// If x1 is a number and it is a natural number the length will be x1.
// If x1 is a number but not a natural number this will throw an exception.
// Otherwise the array will have one element with x1 as its value.
var a3 = new Array(x1)

// Length is 0.
var a4 = new Array()
```

Because of this, if someone changes the code to pass 1 argument instead of 2 arguments, the array might not have the expected length.

To avoid these kinds of weird cases, always use the more readable array literal.

```js
var a = [x1, x2, x3]
var a2 = [x1, x2]
var a3 = [x1]
var a4 = []
```

Object constructors don't have the same problems, but for readability and consistency object literals should be used.

```js
var o = new Object()

var o2 = new Object()
o2.a = 0
o2.b = 1
o2.c = 2
o2['strange key'] = 3
```

Should be written as:

```js
var o = {}

var o2 = {
    a: 0
  , b: 1
  , c: 2
  , 'strange key': 3
}
```


## Modifying prototypes of builtin objects ##

**No**

Modifying builtins like Object.prototype and Array.prototype are strictly forbidden. Modifying other
builtins like Function.prototype is less dangerous but still leads to hard to debug issues in
production and should be avoided.


## Internet Explorer's Conditional Comments ##

**No**

Don't do this:

```js
var f = function () {
    /*@cc_on if (@_jscript) { return 2* @*/  3; /*@ } @*/
}
```

Conditional Comments hinder automated tools as they can vary the JavaScript syntax tree at runtime.
