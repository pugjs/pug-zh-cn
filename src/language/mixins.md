---
title: Mixins
template: language
---

# Mixins

Mixins allow you to create reusable blocks of Pug.

```pug-preview
//- Declaration
mixin list
  ul
    li foo
    li bar
    li baz
//- Use
+list
+list
```

They are compiled to functions and can take arguments:

```pug-preview
mixin pet(name)
  li.pet= name
ul
  +pet('cat')
  +pet('dog')
  +pet('pig')
```

## Mixin Blocks

Mixins can also take a block of pug to act as the content:

```pug-preview
mixin article(title)
  .article
    .article-wrapper
      h1= title
      if block
        block
      else
        p No content provided

+article('Hello world')

+article('Hello world')
  p This is my
  p Amazing article
```

## Mixin Attributes

Mixins also get an implicit attributes argument taken from the attributes passed to the mixin:

```pug-preview
mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class href=href)= name

+link('/foo', 'foo')(class="btn")
```

::: float note
The values in `attributes` by default are already escaped so you should use `!=` to avoid escaping them a second time (see also [unescaped attributes]).
:::

You can also use mixins with [`&attributes`]:

```pug-preview
mixin link(href, name)
  a(href=href)&attributes(attributes)= name

+link('/foo', 'foo')(class="btn")
```

## Rest Arguments

You can write mixins that take an unknown number of arguments using the "rest arguments" syntax.  e.g.

```pug-preview
mixin list(id, ...items)
  ul(id=id)
    each item in items
      li= item

+list('my-list', 1, 2, 3, 4)
```

[`&attributes`]: attributes.html#attributes
[unescaped attributes]: attributes.html#unescaped-attributes
