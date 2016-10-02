---
title: 代码 Code
template: language
id: language/code
---

# 代码 Code

Pug 为您在模板中嵌入 JavaScript 提供了可能。这里有三种类型的代码。

<span id="to-do" />
## 不输出的代码

用 `-` 开始一段不直接进行输出的代码，比如：

```pug-preview
- for (var x = 0; x < 3; x++)
  li item
```

Pug 也支持把它们写成一个块的形式：

```pug-preview
-
  list = ["Uno", "Dos", "Tres",
          "Cuatro", "Cinco", "Seis"]
each item in list
  li= item
```

<span id="to-do" />
## 带输出的代码

用 `=` 开始一段带有输出的代码，它应该是可以被求值的一个 JavaScript 表达式。为安全起见，它将被 HTML 转义：

```pug-preview
p
  = '这个代码被 <转义> 了！'
```

也可以写成行内形式，同样也支持所有的 JavaScript 表达式：

```pug-preview
p= '这个代码被 <转义> 了！'
```

<span id="to-do" />
## 不转义的、带输出的代码

用 `!=` 开始一段不转义的，带有输出的代码。这将不会做任何转义，所以用于执行用户的输入将会不安全：

```pug-preview
p
  != '这段文字 <strong>没有</strong> 被转义！'
```

同样也可以写成行内形式，支持所有的 JavaScript 表达式：

```pug-preview
p!= '这段文字' + ' <strong>没有</strong> 被转义！'
```

::: float danger Caution
不转义的输出可能是危险的，您必须确保任何来自用户的输入都是安全可靠的，以防止发生 [跨站脚本攻击][cross-site scripting]（XSS）。
:::

[cross-site scripting]: https://en.wikipedia.org/wiki/Cross-site_scripting
