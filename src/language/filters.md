---
title: Filters
template: language
id: language/filters
---

# Filters

Filters let you use other languages within a Pug template.  They take a block of plain text as an input. To pass options to the filter, add them inside parentheses after the filter name, just like one would do with [tag attributes]: `:less(ieCompat=false)`.

All [JSTransformer modules] can be used as Pug filters. Popular filters include `:coffee-script`, `:babel`, `:uglify-js`, `:less`, and `:markdown-it`.

Check out the documentation for the JSTransformer for the options supported for the specific filter.

After you have located which JSTransformer is useful for you as a Pug filter, simply install it through npm in your project directory:

```sh
$ npm install --save jstransformer-coffee-script
$ npm install --save jstransformer-markdown-it
```

Now, you should be able to render the following template:

```pug-preview (serverOnly)
:markdown-it(linkify langPrefix='highlight-')
  # Markdown

  Markdown document with http://links.com and

  \`\`\`js
  var codeBlocks;
  \`\`\`
script
  :coffee-script
    console.log 'This is coffee script'
```

::: float warning Warning
Filters are rendered at compile time, which makes them fast but also means that they cannot support dynamic content or options.

By default, JSTransformer-based filters are also not available during compilation in the browser, unless the JSTransformer modules are explicitly packed and made available through a CommonJS platform like Browserify and Webpack. Templates pre-compiled on the server do not have this limitation.
:::

## Inline Syntax

If the content of the filter is short, one can even use filters as if they are tags:

```pug-preview server-only
script
  :coffee-script alert 'crazy!'
p
  :markdown-it(inline) **BOLD TEXT**
```

## Nested Filters

Multiple filters can be applied to the same block of text by specifying them on the same line. The text is first passed to the last filter, whose result will be transformed by the second last, etc.

In the following example, the script is first transformed by `babel`, and then by `cdata-js`.

```pug-preview (serverOnly)
script
  :cdata-js:babel(preset=['es2015'])
    const myFunc = () => `This is ES2015 in a CD${'ATA'}`;
```

## Custom Filters

You can add your own filters to Pug as well using the [`filters` option][options].

```pug-preview-readonly demo
\\\\\\\\\\ options.js <
options.filters = {
  'my-own-filter': function (text, options) {
    if (options.addStart) text = 'Start\n' + text;
    if (options.addEnd)   text = text + '\nEnd';
    return text;
  }
};
\\\\\\\\\\ index.pug <
p
  :my-own-filter(addStart addEnd)
    Filter
    Body
\\\\\\\\\\ output.html >
<p>
  Start
  Filter
  Body
  End
</p>
```

[tag attributes]: attributes.html
[options]: ../api.html#options
[JSTransformer modules]: https://www.npmjs.com/browse/keyword/jstransformer
