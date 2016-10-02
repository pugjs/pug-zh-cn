---
title: 分支 Case
template: language
id: language/case
---

# 分支 Case

`case` 是 JavaScript 的 `switch` 指令的缩写，并且它接受如下的形式：

```pug-preview
- var friends = 10
case friends
  when 0
    p 您没有朋友
  when 1
    p 您有一个朋友
  default
    p 您有 #{friends} 个朋友
```

## 分支传递 (Case Fall Through)

您可以像 JavaScript 中的 `switch` 语句那样使用传递 (Fall Through)。

```pug-preview
- var friends = 0
case friends
  when 0
  when 1
    p 您的朋友很少
  default
    p 您有 #{friends} 个朋友
```

不同之处在于，在 JavaScript 中，传递会在明确地使用 `break` 语句之前一直进行。而在 Pug 中则是，传递会在遇到非空的语法块前一直进行下去。

在某些情况下，如果您不想输出任何东西的话，您可以明确地加上一个原生的 `break` 语句：

```pug-preview
- var friends = 0
case friends
  when 0
    - break
  when 1
    p 您的朋友很少
  default
    p 您有 #{friends} 个朋友
```

## 语法块的展开

您也可以使用语法块展开：

```pug-preview
- var friends = 1
case friends
  when 0: p 您没有朋友
  when 1: p 您有一个朋友
  default: p 您有 #{friends} 个朋友
```
