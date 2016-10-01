---
title: Extends
template: language
id: language/extends
---

# 拓展 &ndash; 模板继承

`extends` 关键字允许一个模板拓展一个布局或父模板。之后它就可以覆盖某些预定义的内容。

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

::: float info Note
您可以通过做出多重继承来创建强大的模板层次关系。
:::
