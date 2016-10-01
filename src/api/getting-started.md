---
title: 入门指南
template: generic
id: api/getting-started
---

# 入门指南

<span id="installation" />
## 安装

Pug 可以通过 [npm] 获得:

```sh
$ npm install pug
```

[npm]: https://www.npmjs.com/

<span id="overview" />
## 概要

Pug 的渲染操作一般来说是相当简单的。 <code>[pug.compile()]</code> 会把 Pug 代码编译成一个 JavaScript 函数，并且这个函数有一个参数可用于传入数据（局部变量，`locals`）。调用这个编译出来的函数，并且传入您的数据， *很好！* 这时返回的就是用您提供的数据渲染的 HTML 字符串了。

这个编译出来的函数可以被重复使用，也可以传入不同的数据。

```pug
//- template.pug
p #{name} 的 Pug 代码！
```

```js
const pug = require('pug');

// 编译这份代码
const compiledFunction = pug.compileFile('template.pug');

// 渲染一组数据
console.log(compiledFunction({
  name: 'Timothy'
}));
// "<p>Timothy 的 Pug 代码！</p>"

// 渲染另外一组数据
console.log(compiledFunction({
  name: 'Forbes'
}));
// "<p>Forbes 的 Pug 代码！</p>"
```

Pug 也提供了 <code>[pug.render()]</code> 系列的函数，它们把编译和渲染两个步骤合二为一。当然，在每次执行 `render` 的时候，这样一个模板函数都需要被重新编译一遍，这会在一定程度上影响性能。但同时，您也可以在执行 `render` 的时候配合使用 <code>[cache]</code> 选项，它将会把编译出来的函数自动存储到内部缓存中。

```js
const pug = require('pug');

// 编译并使用一组数据渲染 template.pug
console.log(pug.renderFile('template.pug', {
  name: 'Timothy'
}));
// "<p>Timothy 的 Pug 代码！</p>"
```

[pug.compile()]: reference.html#pugcompilesource-options
[pug.render()]: reference.html#pugrendersource-options-callback
[cache]: reference.html#options-cache
