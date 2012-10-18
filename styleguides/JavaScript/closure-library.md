# Closure Usage #

We use [Plovr](http://plovr.com/docs.html) to compile and compress our client-side JavaScript.  It
comes bundled with a copy of the (Closure Library)[closure-library.googlecode.com] which we can
`goog.require` seemlessly from anywhere.

Even though we can include any piece of Closure in Medium, we should not, since some of the classes
are very heavy, primarily due to their flexibility and to support for older browsers (e.g. IE6).

As well as anything in [base.js](http://closure-library.googlecode.com/svn/docs/closure_goog_base.js.html)
the following classes are approved:

  * `goog.Disposable`
  * `goog.array`
  * `goog.asserts`
  * `goog.async.Deferred`
  * `goog.crypt`
  * `goog.events.KeyCodes`
  * `goog.labs.net.xhr`
  * `goog.net.XmlHttp`
  * `goog.math`
  * `goog.object`
  * `goog.pubsub.PubSub`
  * `goog.string`
  * `goog.structs`
  * `goog.uri.util`
  * `goog.userAgent`
  * `goog.testing`

New classes should be approved on a case by case basis

## goog.Disposable ##

The correct usage of `dispose` is to override `disposeInternal()` and to always call the base-class.  The reasons is that `dispose()` checks whether the object is already disposed before calling `disposeInternal()`, this can help avoid NPEs and such.  If you implement `dispose()` yourself you'd want to assert that you weren't disposed.  It is very important to call the super class, particularly in the case of screens, as there is a lot of clean-up done higher up that will avoid memory leaks.

```js
MyClass.prototype.disposeInternal = function () {
  unlisten(this.listenerRef)
  goog.base(this, 'disposeInternal')
}
```

In turn, you can call dispose on instances you have ownership over.

``` js
MyClass.prototype.abort = function () {
  goog.dispose(myDisposableClass)
}
```