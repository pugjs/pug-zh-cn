---
title: Mixin
template: language
id: language/mixins
---

# 混入 Mixin ~~ Mixins

混入是一种允许您在 Pug 中重复使用一整个代码块的方法。

```pug-preview
//- 定义
mixin list
  ul
    li foo
    li bar
    li baz
//- 使用
+list
+list
```

它们会被编译成函数形式，您可以传递一些参数：

```pug-preview
mixin pet(name)
  li.pet= name
ul
  +pet('猫')
  +pet('狗')
  +pet('猪')
```

## 混入的块 ~~ Mixin Blocks

混入也可以把一整个代码块像内容一样传递进来：

```pug-preview
mixin article(title)
  .article
    .article-wrapper
      h1= title
      if block
        block
      else
        p 没有提供任何内容。

+article('Hello world')

+article('Hello world')
  p 这是我
  p 随便写的文章
```

## 混入的属性 ~~ Mixin Attributes

混入也可以隐式地，从“标签属性”得到一个参数 `attributes`：

```pug-preview
mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class href=href)= name

+link('/foo', 'foo')(class="btn")
```

::: float info 提示
`attributes` 里的值已经被（作为标签属性）转义了，所以您可能需要用 `!=` 的方式赋值以避免发生二次转义（详细解释可以查阅 [不转义的属性][unescaped attributes]）。
:::

您也可以直接用 [`&attributes`] 方法来传递 `attributes` 参数：

```pug-preview
mixin link(href, name)
  a(href=href)&attributes(attributes)= name

+link('/foo', 'foo')(class="btn")
```

## 剩余参数 ~~ Rest Arguments

您可以用剩余参数（Rest Arguments）语法来表示参数列表最后传入若干个长度不定的参数，比如：

```pug-preview
mixin list(id, ...items)
  ul(id=id)
    each item in items
      li= item

+list('my-list', 1, 2, 3, 4)
```

[`&attributes`]: attributes.html#attributes
[unescaped attributes]: attributes.html#unescaped-attributes
