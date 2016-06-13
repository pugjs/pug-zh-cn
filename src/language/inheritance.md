---
title: Template Inheritance
template: language
---

# Template Inheritance

Pug supports template inheritance via the `block` and `extends` keywords. A block is simply a "block" of Pug that may be replaced within a child template. This process is recursive.

Pug blocks can provide default content if desired, however optional as shown below by `block scripts`, `block content`, and `block foot`.

```pug
//- layout.pug
html
  head
    title My Site - #{title}
    block scripts
      script(src='/jquery.js')
  body
    block content
    block foot
      #footer
        p some footer content
```

Now to extend the layout, simply create a new file and use the `extends` directive as shown below, giving the path. You may now define one or more blocks that will override the parent block content, note that here the `foot` block is *not* redefined and will output "some footer content".

In Pug v1, if no file extension is given, `.pug` is automatically appended to the path, but in Pug v2 this is behavior is deprecated.

```pug
//- page-a.pug
extends layout.pug

block scripts
  script(src='/jquery.js')
  script(src='/pets.js')

block content
  h1= title
  - var pets = ['cat', 'dog']
  each petName in pets
    include pet.pug
```

```pug
//- pet.pug
p= petName
```

It's also possible to override a block to provide additional blocks, as shown in the following example where `content` now exposes a `sidebar` and `primary` block for overriding, or the child template could override `content` all together.

```pug
//- sub-layout.pug
extends layout.pug

block content
  .sidebar
    block sidebar
      p nothing
  .primary
    block primary
      p nothing
```

```pug
//- page-b.pug
extends sub-layout.pug

block content
  .sidebar
    block sidebar
      p nothing
  .primary
    block primary
      p nothing
```

## Block append / prepend

Pug allows you to `replace` (default), `prepend`, or `append` blocks. Suppose for example you have default scripts in a "head" block that you wish to utilize on *every* page, you might do this:

```pug
//- layout.pug
html
  head
    block head
      script(src='/vendor/jquery.js')
      script(src='/vendor/caustic.js')
  body
    block content
```

Now suppose you have a page of your application for a JavaScript game, you want some game related scripts as well as these defaults, you can simply `append` the block:

```pug
//- page.pug
extends layout.pug

block append head
  script(src='/vendor/three.js')
  script(src='/game.js')
```

When using `block append` or `block prepend` the `block` is optional:

```pug
//- page.pug
extends layout

append head
  script(src='/vendor/three.js')
  script(src='/game.js')
```
