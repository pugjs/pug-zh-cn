---
title: Migrating to Pug 2
template: generic
id: api/migration-v2
---

# Migrating to Pug 2

Pug 2 was released in August 2016. To make improvements in the new release possible, we had to make the decision of deprecating or removing some APIs and undocumented language features. We made an effort to make these changes as unintrusive as possible, and many of these changes were previously warned against as console warnings.

This article details how you can convert your application to the latest version of Pug.

## Project Rename

Due to a trademark issue, the project name has been changed from "Jade" to "Pug" in conjunction with the release of Pug 2. This also means that we have changed the official supported file extension from `.jade` to `.pug`. Although `.jade` is still supported, it is deprecated, and all users are encouraged to transition to `.pug` immediately.

## Removed Language Features

Most of these removals can be automatically detected by [pug-lint], our official linter.

### Legacy Mixin Call

```pug-preview-readonly
\\\\\\\\\\ old.pug <
//- old

mixin foo('whatever')
\\\\\\\\\\ new.pug >
//- new

+foo('whatever')
```

We removed the legacy syntax for calling a [mixin][mixins] to make it easier to differentiate between declaration and calls. All use of the old syntax were warned against in Jade v1.

### Attribute Interpolation

```pug-preview-readonly
\\\\\\\\\\ old.pug <
//- old

a(href='#{link}')

a(href='before#{link}after')
\\\\\\\\\\ new.pug >
//- new

a(href=link)

//- (on Node.js/io.js ≥ 1.0.0)
a(href=`before${link}after`)
//- (everywhere)
a(href='before' + link + 'after')
```

We removed support for interpolation in attributes since it was unnecessarily complex in implementation and tended to delay users learning that they can just use any JavaScript value in place of attributes. Check our [attribute documentation][attributes] for more information on attribute syntax.

### Prefixed <code>each</code> Syntax

```pug-preview-readonly
\\\\\\\\\\ old.pug <
//- old

- each a in b
  = a

- for a in b
  = a
\\\\\\\\\\ new.pug >
//- new

each a in b
  = a

for a in b
  = a
```

`each` is not part of the JavaScript syntax, so the use of `each` "keyword" in a JavaScript line is confusing as well as hackish (in terms of implementation). The same applies to parentheses-less `for` keyword.

Simply remove `-` and your code should work again.

## Removed API

These exported properties and compilation options have been removed. In your application, make sure you are not using these APIs.

### Properties

#### <code>doctype</code>

Previously, the undocumented `jade.doctype` object contained a hash of [doctype shortcuts]. By extending this object, users could create additional or modify existing doctype shortcuts.

In Pug v2, this object has been split from Pug into the [doctypes] package. To extend doctype shortcuts, you could write a `codeGen` plugin.<!-- TODO -->

#### <code>nodes</code>

Previously, the undocumented `jade.nodes` object held a hash of classes that serve as the constructor of the nodes of the (also undocumented) Jade abstract syntax tree. In Pug v2, we have abandoned this approach in favor of duck typing using the `type` property in AST nodes.

#### <code>selfClosing</code>

Previously, the undocumented `jade.selfClosing` array could used to extend or modify the behavior of [self-closing tags].

In Pug v2, this array has been split from Pug into the [void-elements] package. To modify this array, you could write a `codeGen` plugin.<!-- TODO -->

#### <code>utils</code>

Previously, the undocumented `jade.utils` object contained three functions that are useful for template engine internals.

`utils.merge` has been removed from Pug as it is not used anymore. Its functionality can be roughly replicated using the ES2015 <code>[Object.assign]</code> method, among other variants.

`utils.stringify` has been split from Pug into the [js-stringify] package, with additional protection against possible XSS attacks. All users are advised to use that package.

`utils.walkAST` has been split into the [pug-walk] package.

#### <code>Compiler</code>, <code>Lexer</code>, <code>Parser</code>

Previously, the undocumented Jade compiler, lexer, and parser classes were exported through these properties. Users were allowed to create their own compilers, lexers, and parsers that derive from these classes, in order to customize compilation behaviors.

Pug v2 allows customization of the compilation process through plugins, and these exported properties are now removed.

The Pug v2 equivalent of classes are now part of the [pug-code-gen], [pug-lexer], and [pug-parser] packages, with various incompatible changes.

### Options

#### <code>compiler</code>, <code>lexer</code>, <code>parser</code>

These options were used in conjunction with the removed [`Compiler`, `Lexer`, and `Parser` classes](#compiler-lexer-parser).

#### <code>client</code>

The `client` option was used for client function compilation. It was deprecated in favor of the <code>[compileClient]</code> function almost three years ago, and its use has been warned against since then.

[doctypes]: https://www.npmjs.com/package/doctypes
[js-stringify]: https://www.npmjs.com/package/js-stringify
[Object.assign]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
[pug-code-gen]: https://www.npmjs.com/package/pug-code-gen
[pug-lexer]: https://www.npmjs.com/package/pug-lexer
[pug-lint]: https://www.npmjs.com/package/pug-lint
[pug-parser]: https://www.npmjs.com/package/pug-parser
[pug-walk]: https://www.npmjs.com/package/pug-walk
[void-elements]: https://www.npmjs.com/package/void-elements

[attributes]: ../language/attributes.html#attribute-interpolation
[compileClient]: reference.html#pugcompileclientsource-options
[doctype shortcuts]: ../language/doctype.html#doctype-shortcuts
[mixins]: ../language/mixins.html
[self-closing tags]: ../language/tags.html#self-closing-tags
