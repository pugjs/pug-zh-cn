---
title: API 参考文档
template: generic
id: api/reference
---

# API 参考文档 ~~ API Reference

此页面将详细说明如何 JavaScript API 来对 Pug 代码进行渲染。

::: float info 提示
现在 Pug 可以在您的浏览器控制台上使用！想测试各种 Pug 的 API，您可以尝试在控制台输入：

```js
pug.render('p Hello world!');
```
:::

## 选项 ~~ Options

所有的 API 方法都可以使用以下的选项：

```parameter-list
filename
~ string
~ 要编译的代码的文件名。用于异常信息以及（使用相对路径的）包含（include）和扩展（extend）操作。默认值是 `'Pug'`。
basedir
~ string
~ 模板里所有绝对定位的根目录。
doctype
~ string
~ 如果 doctype 没有出现模板里（比如模板只渲染一个 HTML 片段），那么您可以在这里指定它。在一些需要控制自闭合标签和布尔值属性的代码样式的时候非常有用。您可以阅读 [doctype](../language/doctype.html#doctype-option) 的说明来了解更多细节。
pretty
~ boolean | string
~ 在输出的 HTML 里添加 `'  '` 这样的空格缩进来获得更好的代码可读性。如果这里指定的是一个字符串（比如 `'\t'`），那么将会使用它来作为控制缩进的字符。默认为 `false`。
filters
~ object
~ 存放 [自定义过滤器](../language/filters.html#custom-filters) 的哈希表。默认为 `undefined`。
self
~ boolean
~ 是否使用一个叫做 `self` 的命名空间来存放局部变量。这可以加速编译的过程，但是，相对于原来书写比如 `variable` 来访问局部变量，您将需要改为 `self.variable` 来访问它们。默认为 `false`。
debug
~ boolean
~ 当设置为 `true` 时，编译产生的函数代码会被记录到标准输出。
compileDebug
~ boolean
~ 当设置为 `true` 时，源代码会被包含在编译出来的模板函数中，用于提供更详实的错误信息（这在开发时会有用）。它默认是启用的，除非是在 [Express](https://expressjs.com/) 的生产环境模式中。
globals
~ Array<string>
~ 该数组用于向模板中添加全局对象的名字，编译器将保证它们不被局部作用域影响。
cache
~ boolean
~ 当设置为 `true` 时，编译出来的函数会被缓存下来。此时必须填写 `filename` 选项作为缓存的索引字段。该选项仅用于 `render` 函数。默认为 `false`。
inlineRuntimeFunctions
~ boolean
~ 相对于使用 `require` 来获得公用的运行时函数，是否需要直接嵌入这些运行时函数。在 `compileClient` 函数里默认是 `true`，因此就不需要再手动包含那些函数（从而让其能在浏览器上运行）。在其他的 `compile` / `render` 系列函数中，默认是 `false`。
name
~ string
~ 模板函数的名称。仅用于 `compileClient` 函数。默认值是 `'template'`。
```

## 方法 ~~ Methods

### pug.compile(source, ?options)

把一个 Pug 模板编译成一个可多次使用、可传入不同局部变量渲染的函数。

```parameter-list
source
~ string
~ 需要编译的 Pug 代码
options
~ ?options
~ 存放选项的对象
```

```parameter-list (returns)
returns
~ function
~ 一个可以从包含局部变量的对象渲染生成出 HTML 的函数
```

```js
var pug = require('pug');

// Compile a function
var fn = pug.compile('string of pug', options);

// Render the function
var html = fn(locals);
// => '<string>of pug</string>'
```

### pug.compileFile(path, ?options)

从文件中读取一个 Pug 模板，并编译成一个可多次使用、可传入不同局部变量渲染的函数。

```parameter-list
path
~ string
~ 需要编译的 Pug 代码文件的位置
options
~ ?options
~ 存放选项的对象
```

```parameter-list (returns)
returns
~ function
~ 一个可以从包含局部变量的对象渲染生成出 HTML 的函数
```

```js
var pug = require('pug');

// 编译出一个函数
var fn = pug.compileFile('到 Pug 代码文件的路径', options);

// 渲染它
var html = fn(locals);
// => '从 Pug 生成的 HTML 代码'
```

### pug.compileClient(source, ?options)

把一个 Pug 模板编译成一份 JavaScript 代码字符串，它可以直接用在浏览器上而不需要 Pug 的运行时库。

```parameter-list
source
~ string
~ 需要编译的 Pug 代码
options
~ ?options
~ 存放选项的对象
```

```parameter-list (returns)
returns
~ string
~ 一份 JavaScript 代码字符串
```

```js
var pug = require('pug');

// 编译出一个函数
var fn = pug.compileClient('string of pug', options);

// 渲染它
var html = fn(locals);
// => 'function template(locals) { return "从 Pug 生成的 HTML 代码"; }'
```

### pug.compileClientWithDependenciesTracked(source, ?options)

与 <code>[compileClient]</code> 相似，但这个函数返回的是这样一个结构：

```js
{
  'body': 'function (locals) {...}',
  'dependencies': ['filename.pug']
}
```

您应该在需要 `dependencies` 来实现一些诸如“监视 Pug 文件变动”的功能的时候，才使用该函数。

### pug.compileFileClient(path, ?options)

从文件中读取一个 Pug 模板并编译成一份 JavaScript 代码字符串，它可以直接用在浏览器上而不需要 Pug 的运行时库。

```parameter-list
path
~ string
~ 需要编译的 Pug 代码文件的位置
options
~ ?options
~ 存放选项的对象
options.name
~ string
~ 如果您传递了 `.name` 属性给选项对象，那么编译出来的函数名称就会被换成这个属性指定的名称。
```

```parameter-list (returns)
returns
~ string
~ 一份 JavaScript 代码字符串
```

首先，写下我们的模板……

```pug
h1 这是一个 Pug 模板
h2 作者：#{author}
```

然后将 Pug 编译为函数代码字符串。

```js
var fs = require('fs');
var pug = require('pug');

// 将模板编译为一个函数的代码字符串
var jsFunctionString = pug.compileFileClient('/path/to/pugFile.pug', {name: "fancyTemplateFun"});

// 或许您还想要把模板编译到一个叫做 templates.js 的文件里，让浏览器直接使用。
fs.writeFileSync("templates.js", jsFunctionString);
```

您获得的输出的结果可能类似下面的内容（即被写入 `templates.js` 中的代码）：

```js
function fancyTemplateFun(locals) {
  var buf = [];
  var pug_mixins = {};
  var pug_interp;

  var locals_for_with = (locals || {});

  (function (author) {
    buf.push("<h1>这是一个 Pug 模板</h1><h2>作者："
      + (pug.escape((pug_interp = author) == null ? '' : pug_interp))
      + "</h2>");
  }.call(this, "author" in locals_for_with ?
    locals_for_with.author : typeof author !== "undefined" ?
      author : undefined)
  );

  return buf.join("");
}
```

请确保您已经把 Pug 的运行时库（`node_modules/pug/runtime.js`）传给了浏览器，从而让浏览器能够顺利执行您刚才编译出来的函数。

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="/runtime.js"></script>
    <script src="/templates.js"></script>
  </head>

  <body>
    <h1>这是一个神奇的模板。</h1>

    <script type="text/javascript">
      var html = window.fancyTemplateFun({author: "enlore"});
      var div = document.createElement("div");
      div.innerHTML = html;
      document.body.appendChild(div);
    </script>
  </body>
</html>
```

### pug.render(source, ?options, ?callback)

```parameter-list
source
~ string
~ 需要渲染的 Pug 代码
options
~ ?options
~ 存放选项的对象，同时也直接用作局部变量的对象
callback
~ ?function
~ Node.js 风格的回调函数，用于接收渲染结果。**注意：这个回调是同步执行的。**
```

```parameter-list (returns)
returns
~ string
~ 渲染出来的 HTML 字符串
```

```js
var pug = require('pug');

var html = pug.render('string of pug', options);
// => '<string>of pug</string>'
```

### pug.renderFile(path, ?options, ?callback)

```parameter-list
path
~ string
~ 需要渲染的 Pug 代码文件的位置
options
~ ?options
~ 存放选项的对象，同时也直接用作局部变量的对象
callback
~ ?function
~ Node.js 风格的回调函数，用于接收渲染结果。**注意：这个回调是同步执行的。**
```

```parameter-list (returns)
returns
~ string
~ 渲染出来的 HTML 字符串
```

```js
var pug = require('pug');

var html = pug.renderFile('path/to/file.pug', options);
// ...
```

## 属性 ~~ Properties

### pug.filters

这是一个存放 [自定义过滤器][custom filters] 的哈希表。

这个对象和 [`filters` 选项][options] 有着同样的语义，但它是全局地应用在所有 Pug 编译过程的。当一个过滤器名称同时出现在了 `pug.filters` 和 `options.filters` 里，本 `filters` 选项将被优先选择。

::: float warning 不赞成使用
不赞成使用这个属性，应该使用 [`filters` 选项][options] 取而代之。
:::

[compileClient]: #pugcompilesource-options
[options]: #options
[custom filters]: ../language/filters.html#custom-filters
