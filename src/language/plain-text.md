---
title: 纯文本 Plain Text
template: language
id: language/plain-text
---

# 纯文本 Plain Text ~~ Plain Text

Pug 提供了四种方法来放置*纯文本*，换句话说，任何的代码或者文字都将几乎不经过任何处理，直接输出到 HTML 中。它们在不同的情况下会派上不同的用途。纯文本中依然可以使用标签和字符串的[嵌入][interpolation]操作，不过每行开头就不再是 Pug 标签。同样，因为纯文本不经处理，因此您也可以在这里面包含原始 HTML 代码。

一个很常见的坑就是控制渲染出的 HTML 中那些空格。我们将会在本页最后讨论这个话题。

## 标签中的纯文本 ~~ Inline in a Tag

要添加一段*行内*的纯文本，这是最简单的方法。这行内容的第一项就是标签本身。标签与一个空格后面接着的任何东西，都是这个标签的文本内容。

```pug-preview
p 这是一段纯洁的<em>文本</em>内容.
```

## 原始 HTML ~~ Literal HTML

如果一行的开头是左尖括号（`<`），那么整行都会被当作纯文本。这在一些书写 HTML 反而更方便的场合下会很有用，一个经典的例子就是[条件注释][conditional comments]。因为原始 HTML 标签不会被处理，因此它们不像 Pug 的标签，不会自动闭合。

```pug-preview
<html>

body
  p 缩进 body 标签没有意义，
  p 因为 HTML 本身对空格不敏感。

</html>
```

## 管道文本 ~~ Piped Text

另外一种向模板添加纯文本的方法就是在一行前面加一个管道符号（`|`），这个字符在类 Unix 系统下常用作“管道”功能，因此得名。该方法在混合纯文本和行内标签时会很有用，我们稍后在空格控制章节中将会深入探讨。

```pug-preview
p
  | 管道符号总是在最开头，
  | 不算前面的缩进。
```

## 标签中的纯文本块 ~~ Block in a Tag

有的时候您可能想要写一个非常大的纯文本块。一个典型的例子就是用 `script` 和 `style` 内嵌 JavaScript 和 CSS 代码。为此，只需要在标签后面紧接一个点 `.`，在标签有[属性][attributes]的时候，则是紧接在闭括号后面。在标签和点之间不要有空格。块内的纯文本内容必须缩进一层：

```pug-preview
script.
  if (usingPug)
    console.log('这是明智的选择。')
  else
    console.log('请用 Pug。')
```

您也可以在父块内，创建一个“点”块，跟在某个标签的*后面*。

```pug-preview
div
  p This text belongs to the paragraph tag.
  br
  .
    This text belongs to the div tag.
```

## 空格控制 ~~ Whitespace Control

控制输出 HTML 中的空格，可能是学习 Pug 的过程中最为艰难的部分之一，但别担心，您很快就能掌握它。

关于空格，只需记住两个要点。当编译渲染 HTML 的时候：
1. Pug 删掉*缩进*，以及所有*元素间*的空格。
    * 因此，一个闭标签会紧挨着下一个开标签。这对于块级元素，比如段落，通常不是个问题，因为它们被浏览器分别作为一个一个段落在页面上渲染（除非您改变了它们的 CSS `display` 属性）。而当您需要在两个元素间插入空格时，请看下面的方法。
2. Pug *保留*符合以下条件的*元素内*的空格：
    * 一行文本之中所有中间的空格；
    * 在块的缩进后的开头的空格；
    * 一行末尾的空格；
    * 纯文本块、或者连续的管道文本行之间的换行。

因此，Pug 会丢掉标签之间的空格，但保留内部的空格。这将有助于完全掌握应该如何操作标签和纯文本，甚至可以让您在一个单词中间插入一个标签。

```pug-preview
a ……用一个链接结束的句子
| 。
```

如果您需要*添加*空格，有几个选择：

### 推荐方案 ~~ Recommended Solutions

您可以添加一个甚至更多的管道文本行——只有空格或者什么都没有的管道文本。这将会在渲染出来的 HTML 中插入空格。

```pug-preview
| 千万别
|
button#self-destruct 按
|
| 我！
```

如果您需要插入的标签并不需要太多的属性，那么您或许可以试试在一个*纯文本块*内使用更简单的标签嵌入或者原始 HTML。

```pug-preview
p.
  使用常规的标签可以让您的代码行短小精悍，
  但使用嵌入标签会使代码变得更 #[em 清晰易读]。
  ——如果您的标签和文本之间是用空格隔开的。
```

### 不推荐方案 ~~ Not recommended

您可以在文本的开头（在块的缩进、管道符号、或者标签后）或者*末尾*，按照您的需求直接添加空格。

**注意**此处的行尾和行前空格：

```pug-preview
| 嘻嘻，快看 
a(href="http://example.biz/kitteh.png") 这张照片
|  这是我的橘猫
```

上述方案工作正常，但不得不承认有些危险：很多代码编辑器默认都会*移除*行尾的多余空格。您和其他所有的代码贡献者，可能都必须要配置编辑器，使得不会自动移除行尾空格。

[interpolation]: interpolation.html
[conditional comments]: comments.html#conditional-comments
[attributes]: attributes.html
