# JavaScript Rules

Enclosed you’ll find stylistic rules and guidelines for JavaScript authors
contributing code back to Medium. Following these rules will make sure
that our codebase is consistent and maintainable.

Ideally, all our code should look like a single person typed it, no matter
how many people contributed. **If you’re editing a file, make sure your
changes are consistent with the rest of the file even if it goes against
some of the rules on this page.**

### Naming ###

- Use `functionNamesLikeThis`, `variableNamesLikeThis`, `ClassNamesLikeThis`,
  `EnumNamesLikeThis`, `methodNamesLikeThis`, and `SYMBOLIC_CONSTANTS_LIKE_THIS`.
- Private properties, variables, and methods (in files or classes) should be
  named with a leading underscore.
- Protected properties, variables, and methods should be named without an
  underscore (like public ones).
- For more information on private and protected, read the section on visibility.

### Namespaces ###

Client code should use namespaces under `obv` that mirror the folder structure.

```js
goog.provide('obv.sloth')

obv.sloth.sleep = function() {
  // ...
}
```

Filenames should should match the namespace of class that they define, e.g. `obv.foo.Class` should
be in `/foo/Class.js` and `obv.bar.baz` should be in `/bar/baz.js`. Filenames should end in .js, and
should contain no punctuation except for - or _. Prefer - to _

### Code formatting ###

* Always start your curly braces on the same line as whatever they’re opening. For example:

```js
if (something) {
  // ...
} else {
  // ...
}
```

* Try not to make your lines go over 100 characters.
* Use 2-char indent for blocks and 4-char indent for wrapped lines.
* Use whitespace around operators.
* Do not put any space after `[`, `{` or before `]`, `}` in single-line array and object initializers.

```js
var arr = [1,2,3]
var obj = {a: 1, b: 2, c: 3}
```

* Format multiline array and object initializers likes this:

```js
var obj = {
  a: 1,
  b: 2,
  c: 3
}
```

* Do not use semicolons unless omiting one will break your code.
* Use single-quotes for strings.

### Types ###

For client code, we use Google Closure Compiler for type checks. Annotate your functions and properties
accordingly. For more information, seeSee the [JS Compiler Docs](https://developers.google.com/closure/compiler/docs/js-for-compiler).
