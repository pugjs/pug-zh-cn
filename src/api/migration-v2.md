---
title: 迁移到 Pug v2
template: generic
id: api/migration-v2
---

# 迁移到 Pug v2 ~~ Migrating to Pug 2

Pug v2 已经在 2016 年 8 月发布。为了尽可能改善新版本的体验，我们不得不作出决定，移除、或者不赞成使用一些 API 和未归档的特性。我们努力让这些变更尽可能不具破坏性。当前，这些变更大多数会以控制台输出的方式进行警告。

此页面将详细介绍您应该如何将代码从旧版本迁移到最新版本的 Pug 上。

## 新的名称 ~~ Project Rename

因为商标方面的问题，这个项目的名称已经在 Pug v2 发布之际从“Jade”变更为“Pug”。这也意味着官方支持的文件扩展名从 `.jade` 变为 `.pug`。尽管依然支持 `.jade`，但这是不赞成的，我们建议所有的用户都立刻将其改为 `.pug`。

## 已经移除的语言特性 ~~ Removed Language Features

绝大多数被移除的内容都可以被 [pug-lint] 自动检测出来，这是我们官方提供的代码规范器。

### 传统 Mixin 调用 ~~ Legacy Mixin Call

```pug-preview-readonly
\\\\\\\\\\ old.pug <
//- 旧版本

mixin foo('whatever')
\\\\\\\\\\ new.pug >
//- 新版本

+foo('whatever')
```

我们移除了传统的调用 [mixin][mixins] 的语句，这样可以更容易区分 Mixin 的声明和调用。所有这类旧语句在 Jade v1 里都已经警告。

### 属性嵌入 ~~ Attribute Interpolation

```pug-preview-readonly
\\\\\\\\\\ old.pug <
//- 旧版本

a(href='#{link}')

a(href='before#{link}after')
\\\\\\\\\\ new.pug >
//- 新版本

a(href=link)

//- (在 Node.js/io.js ≥ 1.0.0)
a(href=`before${link}after`)
//- (任何场合)
a(href='before' + link + 'after')
```

我们移除了在标签属性里的嵌入支持。它给实现平添了不必要的复杂度，而且也让用户不易注意到属性的赋值其实可以是任意的 JavaScript 表达式。阅读[属性][attributes]的文档来了解更多关于标签属性的语法。

### 带有前缀的 <code>each</code> 语法 ~~ Prefixed <code>each</code> Syntax

```pug-preview-readonly
\\\\\\\\\\ old.pug <
//- 旧版本

- each a in b
  = a

- for a in b
  = a
\\\\\\\\\\ new.pug >
//- 新版本

each a in b
  = a

for a in b
  = a
```

这里的 `each` 并非 JavaScript 的语法，在 JavaScript 代码行中使用 `each`“关键字”会让人感到非常困惑。没有括号的 `for` 关键字也是同样的情况。

只需要简单地删去 `-`，您的代码应该就能重新工作。

## 已删除的 API ~~ Removed API

以下导出属性和编译选项已经被移除。请确保您的代码没有使用它们。

### 属性 ~~ Properties

#### <code>doctype</code>

此前，未归档的对象 `jade.doctype` 是一个存放 [doctype 缩写][doctype shortcuts]的哈希表。用户可以通过扩充这个对象来自定义额外的 doctype 缩写，或者修改已有的 doctype 缩写。

在 Pug v2 中，这个对象已经从 Pug 分离出来，单独成为一个叫做 [doctypes] 的包。如果您要扩充 doctype 缩写，可以写成一个 `codeGen` 的插件。<!-- TODO -->

#### <code>nodes</code>

此前，未归档的对象 `jade.nodes` 是一个存放（未归档的）Jade 抽象语法树节点的构造函数的哈希表。

在 Pug v2 中，我们弃用了这种做法，取而代之的是使用抽象语法树节点的 `type` 属性实现鸭子类型。

#### <code>selfClosing</code>

此前，未归档的数组 `jade.selfClosing` 可以用于添加或者修改[自闭合标签][self-closing tags]的条目。

在 Pug v2 中，这个数组已经从 Pug 分离出来，单独成为一个叫做 [void-elements] 的包。如果您要修改这个数组，可以写成一个 `codeGen` 的插件。<!-- TODO -->

#### <code>utils</code>

此前，`jade.utils` 对象包含了三个模板引擎内部比较有用的函数：

`utils.merge` 已经从 Pug 中移除并不再使用。其功能大体上可以用 ECMAScript 6 的 <code>[Object.assign]</code> 方法及其他变种来实现。

`utils.stringify` 已经从 Pug 分离出来到叫做 [js-stringify] 的包，同时有额外的跨站脚本攻击保护。建议所有用户都改用此代码包。

`utils.walkAST` 已经分离到一个叫做 [pug-walk] 的包。

#### <code>Compiler</code>, <code>Lexer</code>, <code>Parser</code>

此前，未归档的 Jade 的编译器（Compiler）、词法分析器（Lexer）和解析器（Parser）通过这三个属性导出。用户可以从这些类创建自己的编译器、词法分析器和解析器，从而自定义编译的行为。

Pug v2 允许通过插件自定义编译的行为，同时移除这些导出属性。

对应到 Pug v2 中，这几个类现在已经分为 [pug-code-gen]，[pug-lexer] 和 [pug-parser] 这几个包，并且有各种与旧版本不兼容的变更。

### 选项 ~~ Options

#### <code>compiler</code>, <code>lexer</code>, <code>parser</code>

这些选项曾被用于已经被移除的 [`Compiler`，`Lexer` 和 `Parser` 类](#compiler-lexer-parser)。

#### <code>client</code>

该选项曾用于模板函数的编译。大约从 2013 年起不推荐使用，并用 <code>[compileClient]</code> 函数代替，从那时起已经进行了警告。

[doctypes]: https://www.npmjs.com/package/doctypes
[js-stringify]: https://www.npmjs.com/package/js-stringify
[Object.assign]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
[pug-code-gen]: https://www.npmjs.com/package/pug-code-gen
[pug-lexer]: https://www.npmjs.com/package/pug-lexer
[pug-lint]: https://www.npmjs.com/package/pug-lint
[pug-parser]: https://www.npmjs.com/package/pug-parser
[pug-walk]: https://www.npmjs.com/package/pug-walk
[void-elements]: https://www.npmjs.com/package/void-elements

[attributes]: ../language/attributes.html#attribute-interpolation
[compileClient]: reference.html#pugcompileclientsource-options
[doctype shortcuts]: ../language/doctype.html#doctype-shortcuts
[mixins]: ../language/mixins.html
[self-closing tags]: ../language/tags.html#self-closing-tags
