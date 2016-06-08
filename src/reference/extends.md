# Extends &ndash; Template Inheritance

The `extends` keyword allows a template to extend a layout or parent template. It can then override certain pre-defined blocks of content.

```pug-preview demo=extends
\\\\\\\\\\ index.pug
//- index.pug
extends layout.pug

block title
  title Article Title

block content
  h1 My Article
\\\\\\\\\\ layout.pug
//- layout.pug
doctype html
html
  head
    block title
      title Default title
  body
    block content
```

::: float note
You can have multiple levels of inheritance, allowing you to create powerful hierarchies of templates.
:::
