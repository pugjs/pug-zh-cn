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

The doctype affects compilation in some other cases, for example self closing tags and [boolean attributes](attributes.html#boolean-attributes). For this reason, you might sometimes want to specify it manually. You can do this via the `doctype` option.

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
