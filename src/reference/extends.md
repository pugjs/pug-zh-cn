# Extends &ndash; Template Inheritance

The `extends` keyword allows a template to extend a layout or parent template. It can then override certain pre-defined blocks of content.

```pug-demo-static
\\\\\\\\\\layout.pug
doctype html
html
  head
    block title
      title Default title
  body
    block content
\\\\\\\\\\index.pug
extends ./layout.pug

block title
  title Article Title

block content
  h1 My Article
\\\\\\\\\\output.html
<!DOCTYPE html>
<html>
  <head>
    <title>Article Title</title>
  </head>
  <body>
    <h1>My Article</h1>
  </body>
</html>
```

:::panel info Note
You can have multiple levels of inheritance, allowing you to create powerful hierarchies of templates.
:::
