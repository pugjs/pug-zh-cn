---
title: 模板继承 Inheritance
template: generic
id: language/inheritance
---

# 模板继承 Inheritance ~~ Template Inheritance

Pug 支持使用 `block` 和 `extends` 关键字进行模板的继承。一个称之为“块”（block）的代码块，可以被子模板覆盖、替换。这个过程是递归的。

Pug 的块可以提供一份默认内容，当然这是可选的，见下述 `block scripts`、`block content` 和 `block foot`。

```pug
//- layout.pug
html
  head
    title 我的站点 - #{title}
    block scripts
      script(src='/jquery.js')
  body
    block content
    block foot
      #footer
        p 一些页脚的内容
```

现在我们来扩展这个布局：只需要简单地创建一个新文件，并如下所示用一句 `extends` 来指出这个被继承的模板的路径。您现在可以定义若干个新的块来覆盖父模板里对应的“父块”。值得注意的是，因为这里的 `foot` 块 *没有* 被重定义，所以会依然输出“一些页脚的内容”。

在 Pug v1 里，如果没有给出文件扩展名，会自动加上 `.pug`。但是这个特性在 Pug v2 中 *这是不赞成使用的*。

```pug
//- page-a.pug
extends layout.pug

block scripts
  script(src='/jquery.js')
  script(src='/pets.js')

block content
  h1= title
  - var pets = ['猫', '狗']
  each petName in pets
    include pet.pug
```

```pug
//- pet.pug
p= petName
```

同样，也可以覆盖一个块并在其中提供一些新的块。如下面的例子所展示的那样，`content` 块现在暴露出两个新的块 `sidebar` 和 `primary` 用来被扩展。当然，它的子模板也可以把整个 `content` 给覆盖掉。

```pug
//- sub-layout.pug
extends layout.pug

block content
  .sidebar
    block sidebar
      p 什么都没有
  .primary
    block primary
      p 什么都没有
```

```pug
//- page-b.pug
extends sub-layout.pug

block content
  .sidebar
    block sidebar
      p 什么都没有
  .primary
    block primary
      p 什么都没有
```

## 块内容的添补 append / prepend ~~ Block append / prepend

Pug 允许您去替换（默认的行为）、`prepend`（向头部添加内容），或者 `append`（向尾部添加内容）一个块。 假设您有一份默认的脚本要放在 `head` 块中，而且希望将它应用到 *每一个页面*，那么您可以这样做：

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

现在假设您有一个页面，那是一个 JavaScript 编写的游戏。您希望把一些游戏相关的脚本也像默认的那些脚本一样放进去，那么您只要简单地 `append` 这个块：

```pug
//- page.pug
extends layout.pug

block append head
  script(src='/vendor/three.js')
  script(src='/game.js')
```

当使用 `block append` 或者 `block prepend` 时，`block` 关键字是可省略的：

```pug
//- page.pug
extends layout

append head
  script(src='/vendor/three.js')
  script(src='/game.js')
```

## 常见错误 ~~ Common mistakes

Pug 模板继承是一个非常强大的功能，您可以借助它将复杂的页面模板拆分成若干个小而简洁的文件。然而，如果您将大量的模板继承、链接在一起，那么有可能会反而把事情弄得更加复杂。

值得注意的是，在扩展模板中，顶级元素（没有缩进的一级）**只能是带名字的块，或者是混入的定义**。理解这一点非常重要，因为父模板定义了整个页面的总体布局和结构，而扩展的模板只能为其特定的块添补或者替换内容。不妨假设我们创建了一个子模板，尝试在已有的块之外再添加内容。很明显，Pug 将没法知道这个内容最终应该在页面的何处出现。

这也包括[不输出的代码][unbuffered code]，因为它们有可能包含生成标记的代码。如果您需要在子模板中定义变量，有这样一些方法：
* 向 Pug 选项添加变量，或者在父模板中用不输出的代码定义。子模板将会继承这些变量。
* 在子模板的*一个块中*定义变量。扩展模板至少要有一个块，否则它将会是空的。所以在这里面定义变量就好了。

基于同样的原因，Pug 的[带输出的注释][buffered comments]也不能出现在扩展模板的顶层：在最终的 HTML 中，它们输出的 HTML 注释找不到地方放。

[unbuffered code]: code.html#unbuffered-code
[buffered comments]: comments.html
