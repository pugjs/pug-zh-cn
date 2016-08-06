---
title: Getting Started
template: generic
id: api/getting-started
---

# Getting Started

## Installation

Pug is available via [npm]:

```sh
$ npm install pug
```

[npm]: https://www.npmjs.com/

## Overview

The general rendering process of Pug is simple. <code>[pug.compile()]</code> will compile the Pug source code into a JavaScript function that takes a data object (called "locals") as argument. Call that resultant function with your data, and *voilà!* it will return a string of HTML.

The compiled function can be used multiple times, so you won't have to compile the source code again.

```pug
//- template.pug
p #{name}'s Pug source code!
```

```js
const pug = require('pug');

// Compile the source code
const compiledFunction = pug.compileFile('template.pug');

// Render a set of data
console.log(compiledFunction({
  name: 'Timothy'
}));
// "<p>Timothy's Pug source code!</p>"

// Render another set of data
console.log(compiledFunction({
  name: 'Forbes'
}));
// "<p>Forbes's Pug source code!</p>"
```

Pug also provides the <code>[pug.render()]</code> family of functions that combine compiling and rendering into one step. However, the template function will be re-compiled every time `render` is called, which might impact the performance.

```js
const pug = require('pug');

// Compile template.pug, and render a set of data
console.log(pug.renderFile('template.pug', {
  name: 'Timothy'
}));
// "<p>Timothy's Pug source code!</p>"
```

[pug.compile()]: reference.html#pugcompilesource-options
[pug.render()]: reference.html#pugrendersource-options-callback
[caching]: reference.html#options-cache
