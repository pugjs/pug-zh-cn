---
title: 注释 Comment
template: language
id: language/comments
---

# 注释 Comment

单行注释和 JavaScript 类似，但是必须独立一行。

```pug-preview
// 一些内容
p foo
p bar
```

Pug 也同样提供了不输出的注释，只需要加上一个横杠。

```pug-preview
//- 这行不会出现在结果里
p foo
p bar
```

<span id="to-do" />
## 块注释

一个格式正确的块注释应该像这样：

```pug-preview
body
  //
    随便写多少字
    都没关系。
```

<span id="to-do" />
## 条件注释

Pug 没有特殊的语法来表示条件注释（conditional comments）。不过因为所有以 `<` 开头的行都会被当作纯文本，因此直接写一个 HTML 风格的条件注释也是没问题的。

```pug-preview
<!--[if IE 8]>
<html lang="en" class="lt-ie9">
<![endif]-->
<!--[if gt IE 8]><!-->
<html lang="en">
<!--<![endif]-->
```
