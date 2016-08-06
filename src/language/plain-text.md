---
title: Plain Text
template: language
id: language/plain-text
---

# Plain Text

Pug provides three common ways of getting plain text.  They are useful in different situations.

## Piped Text

The simplest way of adding plain text to templates is to prefix the line with a `|` character (pronounced "pipe").

```pug-preview
| Plain text can include <strong>html</strong>
p
  | It must always be on its own line
```

## Inline in a Tag

Since it's a common use case, you can put text in a tag just by adding it inline after a space.

```pug-preview
p Plain text can include <strong>html</strong>
```

## Block in a Tag

Often you might want large blocks of text within a tag.  A good example is with inline scripts or styles.  To do this, just add a `.` after the tag (with no preceding space):

```pug-preview
script.
  if (usingPug)
    console.log('you are awesome')
  else
    console.log('use pug')
```
