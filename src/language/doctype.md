---
title: Doctype
template: language
id: language/doctype
---

# Doctype

```pug-preview
doctype html
```

## Doctype Shortcuts

There are shortcuts for commonly used doctypes:

```doctypes
```

## Custom Doctypes

You can also use your own literal custom doctype:

```pug-preview
doctype html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN"
```

## Doctype Option

In addition to being buffered in the output, a doctype in Pug can affect compilation in other ways. For example, whether self-closing tags end with `/>` or `>` depends on whether HTML or XML is specified. The output of [boolean attributes] could be affected as well.

If, for whatever reason, it is not possible to use the `doctype` keyword (e.g. rendering HTML fragments), but you would still like to specify the doctype of the template, you could do so via the [`doctype` option].

```js
var pug = require('./');

var source = 'img(src="foo.png")';

pug.render(source);
// => '<img src="foo.png"/>'

pug.render(source, {doctype: 'xml'});
// => '<img src="foo.png"></img>'

pug.render(source, {doctype: 'html'});
// => '<img src="foo.png">'
```

[boolean attributes]: attributes.html#boolean-attributes
[`doctype` option]: ../api/reference.html#options
