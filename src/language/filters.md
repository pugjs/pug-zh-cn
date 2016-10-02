---
title: 过滤器 Filter
template: language
id: language/filters
extraScripts:
  - /js/filters.js
---

# 过滤器 Filter

过滤器可以让您在 Pug 中使用其他的语言。它们接受传入一个文本块的内容。向过滤器传递参数，只需要将参数如同 [标签属性][tag attributes] 一样，放在括号里即可，如：`:less(ieCompat=false)`

所有的 [JSTransformer 模块][JSTransformer modules] 都可以被用作 Pug 的过滤器。有名的过滤器比如 `:babel`、`:uglify-js`、`:scss` 和 `:markdown-it`。阅读这些 JSTransformer 的文档来了解它们具体支持什么选项。如果您找不到一个您所期望的过滤器，您也可以自己写 [自定义过滤器][custom filter]。

举一个例子，如果您想要能在您的 Pug 模板中使用 CoffeeScript 语言和 Markdown 语言（使用 Markdown-it 渲染），那么您首先要确定这些东西已经安装好：

```sh
$ npm install --save jstransformer-coffee-script
$ npm install --save jstransformer-markdown-it
```

现在可以渲染下面这个模板了。

~~~pug-preview
:markdown-it(linkify langPrefix='highlight-')
  # Markdown

  Markdown document with http://links.com and

  ```js
  var codeBlocks;
  ```
script
  :coffee-script
    console.log 'This is coffee script'
~~~

::: float warning Warning
过滤器在 Pug 编译的时候被渲染，这意味着它们可以很快呈现出来，但是同时也意味着它们不支持动态的内容和选项。

默认情况下，基于 JSTransformer 的过滤器在浏览器上编译的时候也是不可用的，除非那个 JSTransformer 模块被明确地封装、引入了，并且通过一个 CommonJS 平台，比如 Browserify 或者 Webpack，使之可以在浏览器上执行。事实上，您现在正在阅读的这个页面就使用了 Browserify 使得过滤器能够在浏览器上执行。

把模板放在服务器上编译渲染则不存在这个限制。
:::

<span id="to-do" />
## 行内语法

如果过滤器的内容很短，我们甚至可以像一个 HTML 标签一样去使用它：

```pug-preview
p
  :markdown-it(inline) **加粗文字**

p.
  在漫无边际的无聊纯文本构成的垃圾文字的海洋上，
  突然一只野生的 #[:markdown-it(inline) *Markdown*]
  出现在了我的视野。
```

<span id="to-do" />
## 嵌套过滤器

可以在一行里同时指定多个过滤器来对一个文本块进行处理。文本首先被传递到最后的过滤器，然后它的结果会被传到倒数第二个过滤器作为输入，以此类推。

在下面的例子里，内容将首先被 `babel` 过滤器处理，然后是 `cdata-js`。

```pug-preview
script
  :cdata-js:babel(presets=['es2015'])
    const myFunc = () => `这是一行在 CD${'ATA'} 里的 ES2015 代码`;
```

<span id="to-do" />
## 自定义过滤器

您可以通过 [`filters` 选项][options] 向 Pug 提供您自定义的过滤器。

```pug-preview-readonly demo
\\\\\\\\\\ options.js <
options.filters = {
  'my-own-filter': function (text, options) {
    if (options.addStart) text = '始\n' + text;
    if (options.addEnd)   text = text + '\n终';
    return text;
  }
};
\\\\\\\\\\ index.pug <
p
  :my-own-filter(addStart addEnd)
    过滤
    正文
\\\\\\\\\\ output.html >
<p>
  始
  过滤
  正文
  终
</p>
```

[tag attributes]: attributes.html
[options]: ../api/reference.html#options
[JSTransformer modules]: https://www.npmjs.com/browse/keyword/jstransformer
[custom filter]: #custom-filters
