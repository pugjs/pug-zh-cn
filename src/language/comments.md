---
title: 注释 Comment
template: language
id: language/comments
---

# 注释 Comment ~~ Comments

带输出的注释和 JavaScript 的单行注释类似，它们像标签，能生成 *HTML* 注释，而且必须独立一行。

```pug-preview
// 一些内容
p foo
p bar
```

Pug 也同样提供了不输出的注释，只需要加上一个横杠。这些内容仅仅会出现在 Pug 模板之中，*不会*出现在渲染后的 HTML 中。

```pug-preview
//- 这行不会出现在结果里
p foo
p bar
```

## 块注释 ~~ Block Comments

一个格式正确的块注释应该像这样：

```pug-preview
body
  //-
    给模板写的注释
    随便写多少字
    都没关系。
  //
    给生成的 HTML 写的注释
    随便写多少字
    都没关系。
```

## 条件注释 ~~ Conditional Comments

Pug 没有特殊的语法来表示条件注释（conditional comments）。条件注释是一种用于分辨 Internet Explorer 新老版本的特殊标记。不过因为所有以 `<` 开头的行都会被当作[纯文本][plain text]，因此直接写一个 HTML 风格的条件注释也是没问题的。

```pug-preview
doctype html

<!--[if IE 8]>
<html lang="en" class="lt-ie9">
<![endif]-->
<!--[if gt IE 8]><!-->
<html lang="en">
<!--<![endif]-->

body
  p Supporting old web browsers is a pain.

</html>
```

[plain text]: plain-text.html
