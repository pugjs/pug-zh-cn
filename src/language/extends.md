---
title: 扩展 Extend
template: language
id: language/extends
---

# 扩展 Extend &ndash; 模板继承 ~~ Extends &ndash; Template Inheritance

`extends` 关键字允许模板去扩展一个布局或父模板，这样它就可以覆盖某些预定义的内容。

```pug-preview (name='extends')
\\\\\\\\\\ index.pug
//- index.pug
extends layout.pug

block title
  title 文章标题

block content
  h1 我的文章
\\\\\\\\\\ layout.pug
//- layout.pug
doctype html
html
  head
    block title
      title 默认标题
  body
    block content
```

::: float info 提示
您还可以通过多层的继承来创建强大的模板层次关系。
:::
