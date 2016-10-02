---
title: 模板继承 Inheritance
template: generic
id: language/inheritance
---

# 模板继承 Inheritance

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

<span id="to-do" />
## 块内容的添补 append / prepend

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
