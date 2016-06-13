---
title: Code
template: language
id: language/code
---

# Code

Pug makes it possible to write inline JavaScript code in your templates. There are three types of code.

## Unbuffered Code

Unbuffered code starts with `-` does not add any output directly, e.g.

```pug-preview
- for (var x = 0; x < 3; x++)
  li item
```

Pug also supports block unbuffered code:

```pug-preview
-
  list = ["Uno", "Dos", "Tres",
          "Cuatro", "Cinco", "Seis"]
each item in list
  li= item
```

## Buffered Code

Buffered code starts with `=` and outputs the result of evaluating the JavaScript expression in the template. For security, it is first HTML escaped:

```pug-preview
p
  = 'This code is <escaped>!'
```

It can also be written inline with attributes, and supports the full range of JavaScript expressions:

```pug-preview
p= 'This code is' + ' <escaped>!'
```

## Unescaped Buffered Code

Unescaped buffered code starts with `!=` and outputs the result of evaluating the JavaScript expression in the template. This does not do any escaping, so is not safe for user input:

```pug-preview
p
  != 'This code is <strong>not</strong> escaped!'
```

It can also be written inline with attributes, and supports the full range of JavaScript expressions:

```pug-preview
p!= 'This code is' + ' <strong>not</strong> escaped!'
```

::: float danger
Unescaped buffered code can be dangerous. You must be sure to sanitize any user
inputs to avoid [cross-site scripting] (XSS).
:::

[cross-site scripting]: https://en.wikipedia.org/wiki/Cross-site_scripting
