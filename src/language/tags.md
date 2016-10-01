---
title: Tags
template: language
id: language/tags
---

# 标签

在默认情况下，在每行文本的开头 (或者紧跟白字符的部分) 书写这个 HTML 标签的名称。缩进了的标签会被认为是嵌套关系，这样会构建一个类似于 HTML 代码的树状结构。

```pug-preview
ul
  li Item A
  li Item B
  li Item C
```

Pug 还知道哪个元素是自闭合的：

```pug-preview
img
```

## 语法块的展开

为了节省空间， Pug 为嵌套标签提供了一种内联式语法。

```pug-preview
a: img
```

## 自闭合标签

诸如 `img`, `meta`, 和 `link` 之类的标签都是自动闭合的 (除非您使用 XML 类型的 doctype)。 您也可以通过在标签后加上 `/` 来显式声明此标签是自闭合的。 请您仅在您知道您自己在做什么的情况下使用。

```pug-preview
foo/
foo(bar='baz')/
```
