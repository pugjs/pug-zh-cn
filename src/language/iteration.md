---
title: 迭代 Iteration
template: language
id: language/iteration
---

# 迭代 Iteration

Pug 目前支持两种主要的迭代方式： `each` 和 `while`。

## each

这是 Pug 的头等迭代方式，让您在模板中迭代数组和对象更为简便：

```pug-preview
ul
  each val in [1, 2, 3, 4, 5]
    li= val
```

您还可以在迭代时获得索引值：

```pug-preview
ul
  each val, index in ['zero', 'one', 'two']
    li= index + ': ' + val
```

Pug 还让您能够迭代对象中的键值：

```pug-preview
ul
  each val, index in {1:'one',2:'two',3:'three'}
    li= index + ': ' + val
```

用于迭代的对象或数组仅仅是个 JavaScript 表达式，因此它可以是变量、函数调用的结果，又或者其他的什么东西。

```pug-preview
- var values = [];
ul
  each val in values.length ? values : ['There are no values']
    li= val
```

您还能添加一个 `else` 块，这个语句块将会在数组与对象没有可供迭代的值时被执行。下面这个例子和上面的例子的作用是等价的：

```pug-preview
- var values = [];
ul
  each val in values
    li= val
  else
    li There are no values
```

您也可以使用 `for` 作为 `each` 的别称。

## while

您也可以使用 `while` 来创建一个循环：

```pug-preview
- var n = 0;
ul
  while n < 4
    li= n++
```
