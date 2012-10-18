# JavaScript Style Rules #

## Naming ##

In general, use `functionNamesLikeThis`, `variableNamesLikeThis`, `ClassNamesLikeThis`,
`EnumNamesLikeThis`, `methodNamesLikeThis`, and `SYMBOLIC_CONSTANTS_LIKE_THIS`.

### Properties and methods ###

- Private properties, variables, and methods (in files or classes) should be named with a leading underscore.
- Protected properties, variables, and methods should be named without an underscore (like public ones).
- For more information on private and protected, read the section on visibility.

### Method and function parameter ###

Optional function arguments start with `opt_`.

Functions that take a variable number of arguments should have the last argument named `var_args`.
You may not refer to `var_args` in the code; use the `arguments` array.

Optional and variable arguments should also be specified in `@param` annotations.

(Note: these are two exceptions to the variableNamingRules)

### Accessor functions ###

Getters and setters methods for properties are not required. However, if they are used, then getters
must be named `getFoo()` and setters must be named `setFoo(value)`. (For boolean getters, `isFoo()`
is also acceptable, and often sounds more natural.)

### Namespaces ###

Client code should use namespaces under `obv` that mirror the folder structure.

```js
goog.provide('obv.sloth')

obv.sloth.sleep = function() {
  ...
}
```


### Filenames ###

Filenames should should match the namespace of class that they define, e.g. `obv.foo.Class` should
be in `/foo/Class.js` and `obv.bar.baz` should be in `/bar/baz.js`. Filenames should end in .js, and
should contain no punctuation except for - or _

Prefer - to _


## Custom toString() methods ##

**Must always succeed without side effects.**

You can control how your objects string-ify themselves by defining a custom `toString()` method.
This is fine, but you need to ensure that your method (1) always succeeds and (2) does not have
side-effects. If your method doesn't meet these criteria, it's very easy to run into serious
problems. For example, if toString() calls a method that does an assert, assert might try to output
the name of the object in which it failed, which of course requires calling toString().


## Deferred initialization ##

**OK**

It isn't always possible to initialize variables at the point of declaration, so deferred
initialization is fine.


## Explicit scope ##

**Always** ((Debatable))

Always use explicit scope - doing so increases portability and clarity. For example, don't rely on
window being in the scope chain. You might want to use your function in another application for
which window is not the content window.


## Code formatting ##

### Curly Braces ###

Because of implicit semicolon insertion, always start your curly braces on the same line as whatever
they're opening. For example:

```js
if (something) {
  // ...
} else {
  // ...
}
```

### Line Length ###

We don't strictly enforce an line length limit, but be sensible.  Prefer to wrap at 100-chars,
especially for comments and doc blocks, horizontal scrolling sucks.

### Whitespace ###

In general:

- 2-char indent for blocks.
- 4-char indent for wrapped lines (unless stated otherwise below).
- whitespace around operators.

The following sections demonstrate and explain specific cases:

### Array and Object Initializers ###

Single-line array and object initializers are allowed when they fit on a line:

No space after `[` or before `]`

```js
var arr = [1, 2, 3] .
```

No space after `{` or before `}`

``` js
var obj = {a: 1, b: 2, c: 3}
```

Multiline array initializers and object initializers are indented 4, unless there is a comma:

```js
// Object initializer.
var inset = {
    top: 10
  , right: 20
  , bottom: 15
  , left: 12
}

// Array initializer.
this._rows = [
    '"Slartibartfast" <fjordmaster@magrathea.com>'
  , '"Zaphod Beeblebrox" <theprez@universe.gov>'
  , '"Ford Prefect" <ford@theguide.com>'
  , '"Arthur Dent" <has.no.tea@gmail.com>'
  , '"Marvin the Paranoid Android" <marv@googlemail.com>'
  , 'the.mice@magrathea.com'
]

// Used in a method call.
createDom('div', {
    id: 'foo'
  , className: 'some-css-class'
  , style: 'display:none'
}, 'Hello, world!')
```

Long identifiers or values present problems for aligned initialization lists, so always prefer non-aligned initialization. For example:

```js
CORRECT_Object.prototype = {
    a: 0
  , b: 1
  , lengthyName: 2
}
```

Not like this:

```js
WRONG_Object.prototype = {
    a          : 0
  , b          : 1
  , lengthyName: 2
}
```


### Function Arguments ###

When possible, all function arguments should be listed on the same line. If doing so would exceed
a reasonable line length, the arguments must be line-wrapped in a readable way. The indentation may
be either four spaces, or aligned to the parenthesis.

Below are the most common patterns for argument wrapping:

```js
// Four-space, wrap at 100.  Works with very long function names, survives
// renaming without reindenting, low on space.
example.foo.bar.doThingThatIsVeryDifficultToExplain = function(
    veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
    tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
  // ...
}

// Four-space, one argument per line.  Works with long function names,
// survives renaming, and emphasizes each argument.
example.foo.bar.doThingThatIsVeryDifficultToExplain = function(
    veryDescriptiveArgumentNumberOne,
    veryDescriptiveArgumentTwo,
    tableModelEventHandlerProxy,
    artichokeDescriptorAdapterIterator) {
  // ...
}

// Parenthesis-aligned indentation, wrap at 100.  Visually groups arguments,
// low on space.
function foo(veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
             tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
  // ...
}

// Parenthesis-aligned, one argument per line.  Visually groups and
// emphasizes each individual argument.
function bar(veryDescriptiveArgumentNumberOne,
             veryDescriptiveArgumentTwo,
             tableModelEventHandlerProxy,
             artichokeDescriptorAdapterIterator) {
  // ...
}
```

When the function call is itself indented, you're free to start the 4-space indent relative to the
beginning of the original statement or relative to the beginning of the current function call. The
following are all acceptable indentation styles.

```js
if (veryLongFunctionNameA(
        veryLongArgumentName) ||
    veryLongFunctionNameB(
    veryLongArgumentName)) {
  veryLongFunctionNameC(veryLongFunctionNameD(
      veryLongFunctioNameE(
          veryLongFunctionNameF)))
}
```


### Passing Anonymous Functions ###

When declaring an anonymous function in the list of arguments for a function call, the body of the
function is indented two spaces from the left edge of the statement, or two spaces from the left
edge of the function keyword. This is to make the body of the anonymous function easier to read
(i.e. not be all squished up into the right half of the screen).

```js
prefix.something.reallyLongFunctionName('whatever', function(a1, a2) {
  if (a1.equals(a2)) {
    someOtherLongFunctionName(a1)
  } else {
    andNowForSomethingCompletelyDifferent(a2.parrot)
  }
})

var names = prefix.something.myExcellentMapFunction(
    verboselyNamedCollectionOfItems,
    function(item) {
      return item.name
    })
```


### More Indentation ###

In fact, except for array and object initializers , and passing anonymous functions, all wrapped
lines should be indented four spaces, not indented two spaces.

```js
thisIsAVeryLongVariableName =
    hereIsAnEvenLongerOtherFunctionNameThatWillNotFitOnPrevLine()

thisIsAVeryLongVariableName = 'expressionPartOne' + someMethodThatIsLong() +
    thisIsAnEvenLongerOtherFunctionNameThatCannotBeIndentedMore()

someValue = this.foo(
    shortArg,
    'Some really long string arg - this is a pretty common case, actually.',
    shorty2,
    this.bar())

if (searchableCollection(allYourStuff).contains(theStuffYouWant) &&
    !ambientNotification.isActive() && (client.isAmbientSupported() ||
                                        client.alwaysTryAmbientAnyways())) {
  ambientNotification.activate()
}
```


### Blank lines ###

Use newlines to group logically related pieces of code. For example:

```js
doSomethingTo(x)
doSomethingElseTo(x)
andThen(x)

nowDoSomethingWith(y)

andNowWith(z)
```


### Binary and Ternary Operators ###

Always put the operator on the preceding line, so that you don't have to think about implicit
semi-colon insertion issues.

```js
var x = a ? b : c  // All on one line if it will fit.

// Indentation +4 is OK.
var y = a ?
    longButSimpleOperandB : longButSimpleOperandC

// Chained calls can put the period at the start
this.doSomething()
    .doSomethingElse()
    .andNext()
```

Long lines often look bad no matter what you do, so if all else fails, just _try_ to make it look
as good as you can.


## Parentheses ##

**Only where required**

Use sparingly and in general only where required by the syntax and semantics.

Never use parentheses for unary operators such as `delete`, `typeof` and `void` or after keywords
such as `return`, `throw` as well as others (`case`, `in` or `new`).


## Strings ##

**Prefer ' over "**

For consistency single-quotes (') are preferred to double-quotes ("). This is helpful when creating
strings that include HTML:

```js
var msg = 'This is <span class="html">some</span> HTML'
```


## Visibility (private and protected fields) ##

**Encouraged, use JSDoc annotations @private and @protected**

We recommend the use of the JSDoc annotations @private and @protected to indicate visibility levels
or classes, functions, and properties.

@private global variables and functions are only accessible to code in the same file.

Constructors marked @private may only be instantiated by code in the same file and by their static
and instance members. @private constructors may also be accessed anywhere in the same file for their
public static properties and by the `instanceof` operator.

Global variables, functions, and constructors should never be annotated @protected.


## JavaScript Types ##

**Encouraged and (eventually) enforced by the compiler for client code**

When documenting a type in JSDoc, be as specific and accurate as possible. The types we support are
JS2 style types and JS1.x types.

See the [JS Compiler Docs](https://developers.google.com/closure/compiler/docs/js-for-compiler) for
a full reference.


## Comments ##

**Use JSDoc**

All files, classes, methods and properties should be documented with JSDoc comments.

Inline comments should be of the `//` variety and have a space before the first character.

Avoid sentence fragments. Start sentences with a properly capitalized word, and end them with punctuation.

### Comment Syntax ###

The JSDoc syntax is based on JavaDoc . Many tools extract metadata from JSDoc comments to perform
code validation and optimizations. These comments must be well-formed.

```js
/**
 * A JSDoc comment should begin with a slash and 2 asterisks.
 * Inline tags should be enclosed in braces like {@code this}.
 * @desc Block tags should always start on their own line.
 */
 ```

### TODOs ###

TODOs should descriptively declare how a piece of code should change in the future.
TODOs are specified in // style and specify a person who has more information about it.
It's preferred to keep TODOs out of jsdocs and near the applicable code itself.
For critical TODOs, it's best to also file a tracking issue.

```js
// TODO(ev): Use a better sorting algorithm than bubble sort here.
```


### JSDoc Indentation ###

If you have to line break a block tag, you should treat this as breaking a code statement and indent
it four spaces.

```js
/**
 * Illustrates line wrapping for long param/return descriptions.
 * @param {string} foo This is a param with a description too long to fit in
 *     one line.
 * @return {number} This returns something that has a description too long to
 *     fit in one line.
 */
project.MyClass.prototype.method = function(foo) {
  return 5
}
```

You should not indent the @fileoverview command.


### Top/File-Level Comments ###

The top level comment is designed to orient readers unfamiliar with the code to what is in this
file. It should provide a description of the file's contents, its author(s), and any dependencies or
compatibility information. As an example:

```js
// Copyright 2012 The Obvious Corporation.

/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 */
```


### Class Comments ###

Classes must be documented with a description, and appropriate type tags.

```js
/**
 * Class making something fun and easy.
 * @param {string} arg1 An argument that makes this more interesting.
 * @param {Array.<number>} arg2 List of numbers to be processed.
 * @constructor
 * @extends {goog.Disposable}
 */
project.MyClass = function(arg1, arg2) {
  // ...
}
goog.inherits(project.MyClass, goog.Disposable)
```

### Method and Function Comments ###

A description must be provided along with parameters. Method descriptions should start with a
sentence written in the third person declarative voice.

```js
/**
 * Operates on an instance of MyClass and returns something.
 * @param {project.MyClass} obj Instance of MyClass which leads to a long
 *     comment that needs to be wrapped to two lines.
 * @return {boolean} Whether something occured.
 */
function PR_someMethod(obj) {
  // ...
}
```

For simple getters that take no parameters and have no side effects, the description can be omitted.

### Property Comments ###

```js
/**
 * Maximum number of things per pane.
 * @type {number}
 */
project.MyClass.prototype.someProperty = 4
```

## Inner Classes and Enums ##

**Should be defined in the same file as the top level class**

Inner classes and enums defined on another class should be defined in the same file as the top level
class. goog.provide statements are only necessary for the top level class.



# Tips and Tricks #

## JavaScript tidbits ##

### True and False Boolean Expressions ###

The following are all false in boolean expressions:

```
null
undefined
'' the empty string
0 the number
```

But be careful, because these are all true:

```
'0' the string
[] the empty array
{} the empty object
This means that instead of this:
```

while (x != null) {
you can write this shorter code (as long as you don't expect x to be 0, or the empty string, or false):

while (x) {
And if you want to check a string to see if it is null or empty, you could do this:

if (y != null && y != '') {
But this is shorter and nicer:

if (y) {

Caution: There are many unintuitive things about boolean expressions. Here are some of them:

```
Boolean('0') == true
'0' != true
0 != null
0 == []
0 == false
Boolean(null) == false
null != true
null != false
Boolean(undefined) == false
undefined != true
undefined != false
Boolean([]) == true
[] != true
[] == false
Boolean({}) == true
{} != true
{} != false
```

### Conditional (Ternary) Operator (?:) ###

Instead of this:

```js
if (val != 0) {
  return foo()
} else {
  return bar()
}
```

you can write this:

```js
return val ? foo() : bar()
```

The ternary conditional is also useful when generating HTML:

```js
var html = '<input type="checkbox"' +
    (isChecked ? ' checked' : '') +
    (isEnabled ? '' : ' disabled') +
    ' name="foo">'
```

## && and || ##

These binary boolean operators are short-circuited, and evaluate to the last evaluated term.

"||" has been called the 'default' operator, because instead of writing this:

```js
/** @param {*=} opt_win */
function foo(opt_win) {
  var win
  if (opt_win) {
    win = opt_win
  } else {
    win = window
  }
  // ...
}
```

you can write this:

```js
/** @param {*=} opt_win */
function foo(opt_win) {
  var win = opt_win || window
  // ...
}
```

"&&" is also useful for shortening code. For instance, instead of this:

```js
if (node) {
  if (node.kids) {
    if (node.kids[index]) {
      foo(node.kids[index])
    }
  }
}
```

you could do this:

```js
if (node && node.kids && node.kids[index]) {
  foo(node.kids[index])
}
```

or this:

```js
var kid = node && node.kids && node.kids[index]
if (kid) {
  foo(kid)
}
```

However, this is going a little too far:

```js
node && node.kids && node.kids[index] && foo(node.kids[index])
```

and can cause problems due to our usage of ASI.

### Iterating over Node Lists ###

Node lists are often implemented as node iterators with a filter. This means that getting a property like length is O(n), and iterating over the list by re-checking the length will be O(n^2).

```js
var paragraphs = document.getElementsByTagName('p')
for (var i = 0; i < paragraphs.length; i++) {
  doSomething(paragraphs[i])
}
```

It is better to do this instead:

```js
var paragraphs = document.getElementsByTagName('p')
for (var i = 0, paragraph; paragraph = paragraphs[i]; i++) {
  doSomething(paragraph)
}
```

This works well for all collections and arrays as long as the array does not contain things that are treated as boolean false.

In cases where you are iterating over the childNodes you can also use the firstChild and nextSibling properties.

```js
var parentNode = document.getElementById('foo')
for (var child = parentNode.firstChild; child; child = child.nextSibling) {
  doSomething(child)
}
```
