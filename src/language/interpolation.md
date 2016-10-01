---
title: 嵌入 Interpolation
template: language
id: language/interpolation
---

<!--
.panel-heading Hey!
.panel-body
  p.
    The Pug source code displayed in this, and many of the other pages
    in these docs, is interactive. Edit it and see what happens!
-->

# 嵌入 Interpolation

Pug 提供了若干种操作符以满足您不同的嵌入需求。

<span id="string-interpolation-escaped" />
## 字符串嵌入，转义

观察下面例子中的局部变量 `title`、`author` 和 `theGreat` 是如何被嵌入模板的。

```pug-preview
- var title = "On Dogs: Man's Best Friend";
- var author = "enlore";
- var theGreat = "<span>转义!</span>";

h1= title
p #{author} 源于真情的创作。
p 这是安全的：#{theGreat}
```

`title` 被简单地求值；但在 `#{` 和 `}` 之间的文本也会被求值，转义，并最终嵌入到模板的渲染输出中。

这可以是任何的正确的 JavaScript 表达式，您可以自由发挥。

```pug-preview
- var msg = "not my inside voice";
p This is #{msg.toUpperCase()}
```

Pug 足够聪明来分辨到底哪里才是嵌入表达式的结束，所以您不用担心表达式中有 `}`，也不需要额外的转义。

```pug-preview
p 不要转义 #{'}'}！
```

如果您需要表示一个 `#{` 文本，您可以转义它，也可以用嵌入功能来生成（可以，这很元编程）。

```pug-preview
p Escaping works with \#{interpolation}
p Interpolation works with #{'#{interpolation}'} too!
```

<span id="string-interpolation-unescaped" />
## 字符串嵌入，不转义

您当然也 *并不是必须* 要用安全的转义来构造内容。您可以嵌入没有转义的文本进入模板中。

```pug-preview
- var riskyBusiness = "<em>我希望通过外籍教师 Peter 找一位英语笔友。</em>";
.quote
  p 李华：!{riskyBusiness}
```

::: float danger Caution
请您务必清醒地意识到，如果直接使用您的用户提供的数据，未进行转义的内容可能会带来安全风险。不要相信用户的输入！
:::

<span id="tag-interpolation" />
## 标签嵌入

嵌入功能不仅可以嵌入 JavaScript 表达式的值，也可以嵌入用 Pug 书写的标签。它看起来应该像这样：

```pug-preview
p.
  这是一个很长很长而且还很无聊的段落，还没有结束，是的，非常非常地长。
  突然出现了一个 #[strong 充满力量感的单词]，这确实让人难以 #[em 忽视]。
```

您确实可以用行内的 HTML 代码做同样的事情。但问题是，怎么用 Pug 来做这件事情呢？将 Pug 的标签语句用 `#[` 和 `]` 括起来就可以了。它会被求值并嵌入到它原来位置的内容中。

<span id="whitespace-control" />
### 空格的调整

标签嵌入功能，在需要嵌入的位置上前后的空格非常关键的时候，就变得非常有用了。因为 Pug 默认会去除一个标签前后的所有空格。请观察下面一个例子：

```pug-preview
p
  | 如果我不用嵌入功能来书写，一些标签比如
  strong strong
  | 和
  em em
  | 可能会产生意外的结果。
p.
  如果用了嵌入，在 #[strong strong] 和 #[em em] 旁的空格就会让我舒服多了。
```
