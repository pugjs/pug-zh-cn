---
title: Includes
template: language
---

# Includes

Includes allow you to insert the contents of one Pug file into another.

```pug-preview
\\\\\\\\\\ index.pug
//- index.pug
doctype html
html
  include includes/head.pug
  body
    h1 My Site
    p Welcome to my super lame site.
    include includes/foot.pug
\\\\\\\\\\ includes/head.pug
head
  title My Site
  script(src='/javascripts/jquery.js')
  script(src='/javascripts/app.js')
\\\\\\\\\\ includes/foot.pug
//- includes/foot.pug
footer#footer
  p Copyright (c) foobar
```

The path of the included file, if specified absolutely (e.g. `include /root.pug`), is resolved by prepending `options.basedir` to the file name provided. Otherwise, the path is relative to the current file being compiled.

In Pug v1, if no file extension is given, `.pug` is automatically appended to the file name, but in Pug v2 this is behavior is removed.

## Including Plain Text

Including files that are not Pug just includes the raw text.

```pug-preview
\\\\\\\\\\ index.pug
//- index.pug
doctype html
html
  head
    style
      include style.css
  body
    h1 My Site
    p Welcome to my super lame site.
    script
      include script.js
\\\\\\\\\\ style.css
/* style.css */
h1 {
  color: red;
}
\\\\\\\\\\ script.js
// script.js
console.log('You are awesome');
```

## Including Filtered Text

You can combine filters with includes to filter things as you include them.

```pug-preview-readonly
\\\\\\\\\\ index.pug <
//- index.pug
doctype html
html
  head
    title An Article
  body
    include:markdown-it article.md
\\\\\\\\\\ article.md <
# article.md

This is an article written in markdown.
\\\\\\\\\\ output.html >
<!DOCTYPE html>
<html>
  <head>
    <title>An Article</title>
  </head>
  <body>
    <h1>article.md</h1>
    <p>This is an article written in markdown.</p>
  </body>
</html>
```
