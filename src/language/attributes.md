---
title: 属性 Attribute
template: language
id: language/attributes
---

# 属性 Attribute ~~ Attributes

标签属性和 HTML 语法非常相似，但它们的值就是普通的 JavaScript 表达式。您可以用逗号作为属性分隔符，不过不加逗号也是允许的。

```pug-preview
a(href='baidu.com') 百度
= '\n'
a(class='button' href='baidu.com') 百度
= '\n'
a(class='button', href='baidu.com') 百度
```

所有 JavaScript 表达式都能用：

```pug-preview
//- 已登录
- var authenticated = true
body(class=authenticated ? 'authed' : 'anon')
```

## 多行属性 ~~ Multiline Attributes

如果您有很多属性，您可以把它们分几行写：

```pug-preview
input(
  type='checkbox'
  name='agreement'
  checked
)
```

如果您有一个很长属性，并且您的 JavaScript 运行时引擎支持 ECMAScript 6 [模板字符串][template strings]（包括 Node.js 和 io.js v1.0.0 和更新的版本），您可以使用它来写属性值：

```pug-preview (features=['templatestrings'])
input(data-json=`
  {
    "非常": "长的",
    "数据": true
  }
`)
```

## 用引号括起来的属性 ~~ Quoted Attributes

如果您的属性名称中含有某些奇怪的字符，并且可能会与 JavaScript 语法产生冲突的话，请您将它们使用 `""` 或者 `''` 括起来。您还可以使用逗号来分割不同的属性。这种属性的例子有 Angular 2 中经常用到的 `[]` 和 `()`。

```pug-preview
//- 在这种情况下， `(click)` 会被当作函数调用而不是
//- 属性名称，这会导致一些稀奇古怪的错误。
div(class='div-class' (click)='play()')
```

```pug-preview
div(class='div-class', (click)='play()')
div(class='div-class' '(click)'='play()')
```

## 属性嵌入 ~~ Attribute Interpolation

::: float danger 注意
在 Pug / Jade 的旧版本中支持一种嵌入语法：

```pug
a(href="/#{url}") Link
```

这种语法 **已经不再被支持！** 它的相关替代可以在本文档的下面部分找到。另外请您参考我们的 [迁移指南][migration guide] 以获取更多 Pug v2 与之前版本的不兼容情形。
:::

如果您想要在您的属性当中使用变量的话，您可以这样做：

1. 直接使用 JavaScript 写那个属性：

   ```pug-preview
   - var url = 'pug-test.html';
   a(href='/' + url) 链接

   = '\n'

   - url = 'https://example.com/'
   a(href=url) 另一个链接
   ```

2. 如果您的 JavaScript 运行时支持 ECMAScript 6 [模板字符串][template strings] (包含在 Node.js/io.js 1.0.0 以及更新的版本当中)，那么您还可以使用这种方式来简化您的属性值：

   ```pug-preview (features=['templatestrings'])
   - var btnType = 'info'
   - var btnSize = 'lg'
   button(type='button' class='btn btn-' + btnType + ' btn-' + btnSize)
   = '\n'
   button(type='button' class=`btn btn-${btnType} btn-${btnSize}`)
   ```

## 不转义的属性 ~~ Unescaped Attributes

在默认情况下，所有的属性都经过转义 (即把特殊字符转换成转义序列) 来防止诸如跨站脚本攻击之类的攻击方式。如果您非要使用特殊符号，您需要使用 `!=` 而不是 `=`。

```pug-preview
div(escaped="<code>")
div(unescaped!="<code>")
```

::: float danger 危险
未经转义的缓存代码十分危险。您必须正确处理和过滤用户的输入来避免 [跨站脚本攻击][cross-site scripting]。
:::

## 布尔值属性 ~~ Boolean Attributes

在 Pug 中，布尔值属性是经过映射的，这样布尔值 (`true` 和 `false`) 就能接受了。当没有指定值的时候，默认是 `true` 。

```pug-preview
input(type='checkbox' checked)
= '\n'
input(type='checkbox' checked=true)
= '\n'
input(type='checkbox' checked=false)
= '\n'
input(type='checkbox' checked=true.toString())
```

如果 doctype 是 `html` 的话，Pug 就不会去映射属性，而是使用缩写样式 (terse style)(所有浏览器都应该支持)。

```pug-preview
doctype html
= '\n'
input(type='checkbox' checked)
= '\n'
input(type='checkbox' checked=true)
= '\n'
input(type='checkbox' checked=false)
= '\n'
input(type='checkbox' checked=true && 'checked')
```

## 样式 (style) 属性 ~~ Style Attributes

`style` (样式) 属性可以是一个字符串 (就像其他普通的属性一样) 还可以是一个对象，这在部分样式是由 JavaScript 生成的情况下非常方便。


```pug-preview
a(style={color: 'red', background: 'green'})
```

## 类 (class) 属性 ~~ Class Attributes

`class` (类) 属性可以是一个字符串 (就像其他普通的属性一样) 还可以是一个包含多个类名的数组，这在类是由 JavaScript 生成的情况下非常方便。

```pug-preview
- var classes = ['foo', 'bar', 'baz']
a(class=classes)
= '\n'
//- the class attribute may also be repeated to merge arrays
a.bang(class=classes class=['bing'])
```

它还可以是一个将类名映射为 true (真) 或 false (假) 值的对象，这在使用条件性类 (conditional classes) 的时候非常有用

```pug-preview
- var currentUrl = '/about'
a(class={active: currentUrl === '/'} href='/') Home
= '\n'
a(class={active: currentUrl === '/about'} href='/about') About
```

## 类的字面值 ~~ Class Literal

类可以使用 `.classname` 语法来定义：

```pug-preview
a.button
```

考虑到使用 `div` 作为标签名这种行为实在是太常见了，所以如果您省略掉标签名称的话，它就是默认值：

```pug-preview
.content
```

## ID 的字面值 ~~ ID Literal

ID 可以使用 `#idname` 语法来定义：

```pug-preview
a#main-link
```

考虑到使用 `div` 作为标签名这种行为实在是太常见了，所以如果您省略掉标签名称的话，它就是默认值：

```pug-preview
#content
```

## &attributes

读作 "and attributes"，`&attributes` 语法可以将一个对象转化为一个元素的属性列表。

```pug-preview
div#foo(data-bar="foo")&attributes({'data-foo': 'bar'})
```

这个对象不一定必须是一个字面值，它同样也可以是一个包含值的对象（参见 [Mixin 属性][Mixin Attributes]）。

```pug-preview
- var attributes = {};
- attributes.class = 'baz';
div#foo(data-bar="foo")&attributes(attributes)
```

::: float danger 危险
使用 `&attributes` 赋值的属性并不会经过自动转义过程。您必须正确处理和过滤用户的输入来避免 [跨站脚本攻击][cross-site scripting] (XSS)。 但是如果您是从一个 mixin 调用中使用 `attributes` 的话，这个过程就是自动的。
:::

[template strings]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Template_strings
[mixin attributes]: mixins.html#mixin-attributes
[cross-site scripting]: https://en.wikipedia.org/wiki/Cross-site_scripting
[migration guide]: ../api/migration-v2.html
