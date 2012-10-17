# JS Inheritance #

**Obvious JS should use Node's inheritance functions on the server and Closure's inheritance
functions on the client.**

What follows is an explanation of how prototypical inheritance works in JavaScript, how Node and
Closure handle inheritance, and a discussion of alternative approaches.

## Background ##

JavaScript includes a prototype based inheritance model as a language feature.  This is rather
powerful, though most people restrict themselves and follow a more traditional class-based
inheritance approach.  For example, when was the last time you saw an object change its behavior?

```js
function Car() {}
Car.prototype.move = function (x, y) {
  alert('Just driving along in 2D')
}

function Plane() {}
Plane.prototype.move = function (x, y, z) {
  alert('I can fly!!')
}

var batMobile = new Car()
batMobile.move(1, 2)

batMobile.__proto__ = new Plane()
batMobile.move(2, 3, 10)
```

Traditional class-based inheritance leads to systems where the taxonomy and relationships between
different entities are easier to understand.  Code is also easier to reason about; if, at any point,
you can say “The Bat Mobile is a car” you don't have to worry about checking it's type before
calling operations on it (though Duck-Typing can be useful in some situations).

## Native Inheritance ##

If you wanted to make use of prototypical inheritance directly you need to jump through a few hoops.
The simplest way to define a "class" hierarchy is as so:

```js
function BatMobile() {}
BatMobile.prototype = new Car()
```

This is all very well, but what happens if `Car`'s constructor has non-trivial, instance specific
logic in?  Yes, the BatMobile could get messed up.  So to get around this we'd need to create a
temporary constructor:

```js
function BatMobile() {}
function TempClass() {}
TempClass.prototype = Car.prototype
BatMobile.prototype = new TempClass()
```

This sets up a prototype chain without executing `Car`'s constructor.

In order to call a super-classes implementation of a function you can use `.call` or `.apply` to
execute the super class's method in the right scope:

```js
BatMobile.prototype.doBreak = function () {
  Car.prototype.doBreak.call(this)
  this._deployChute()
}
```

The downsides of doing inheritance natively are additional boiler plate when setting up
subclasses, also note how each method calling a super method needs to know the actual super-class.
This can make maintaining or renaming classes annoying.

Luckily for us both Node and Closure have affordances built in to make this process simpler, while
still delegating to native prototypes.


## Node.js ##

Node's `util` package contains an `inherits` function which sets up the prototype chain and adds a
static reference to the constructor.  Typical usage is as follows:

```js
function BatMobile(arg) {
  Car.call(this, arg)
}
util.inherits(BatMobile, Car)

BatMobile.prototype.go = function () {
  BatMobile.super_.go.call(this)
  this.fireRockets()
}
```

## Closure ##

Closure has a similar helper, that predates node, but is basically the same.  Closure includes a
magical function called `goog.base` which the compiler understands and will optimize.

```js
function BatMobile(arg) {
  goog.base(this, arg)
}
goog.inherits(BatMobile, Car)

BatMobile.prototype.go = function () {
  goog.base(this, 'go')
  this.fireRockets()
}
```

## Other Systems ##

There are a multitude of different class systems for JavaScript.  Most of them introduce a DSL that
involves passing object literals to helper methods, though the underlying implementations vary.

Some libraries use a "mixin" approach, which will break the `instanceof` operator.  Other libraries
will actually extend the prototype.  But unless you are familiar with the library and read the
implementation it can be unclear what is happening.

Often this style works well for small classes, but when you have a large class with hundreds of
lines maintenance can become an issue.  For example, if you drop into the file it is not immediately
clear whether the function is being included as a `method` or a `static` or something completely
separate to the class.

While more verbose, seeing `ClassName.prototype.instanceMethod` or `ClassName.staticMethod` is self-
documenting and clear.

Another issue that can arise in systems that do more than delegate to native prototypes is that
classes become incompatible.  Sticking close to the language primitives means there is nothing at
all stopping you from doing this:

```js
function Vehicle() {}

function Car() {}
util.inherits(Car, Vehicle)

function BatMobile() {}
goog.inherits(BatMobile, Car)
```
