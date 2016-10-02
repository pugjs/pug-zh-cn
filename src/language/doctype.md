---
title: Doctype
template: language
id: language/doctype
---

# Doctype

```pug-preview
doctype html
```

## Doctype 缩写 ~~ Doctype Shortcuts

以下是一些常用的 doctype 的缩写：

```doctypes
```

## 自定义 Doctype ~~ Custom Doctypes

您也可以自定义一个 doctype 字面值：

```pug-preview
doctype html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN"
```

## Doctype 选项 ~~ Doctype Option

Doctype 会影响 Pug 的编译结果。比如自闭合的标签是以 `/>` 还是以 `>` 结束，这取决于指定了是 HTML 还是 XML。[布尔值属性][boolean attributes]也同样会受到影响。

如果因为某些原因，不能在模板里使用 `doctype` 关键字（比如需要渲染的是 HTML 的一个片段），但您依然需要指定 doctype 的时候，您就可以通过 [`doctype` 选项][`doctype` option]来设置了。

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
