# Comments

Single line comments look the same as JavaScript comments and must be placed on their own line:

```pug-demo
// just some paragraphs
p foo
p bar
```

Pug also supports unbuffered comments, by simply adding a hyphen

```pug-demo
//- will not output within markup
p foo
p bar
```

## Block Comments

A block comment is legal as well:

```pug-demo
body
  //
    As much text as you want
    can go here.
```

## Conditional Comments

Pug does not have any special syntax for conditional comments. But since all lines beginning with `<` are treated as plain text, normal HTML style conditional comments will do fine.

```pug-demo
<!--[if IE 8]>
<html lang="en" class="lt-ie9">
<![endif]-->
<!--[if gt IE 8]><!-->
<html lang="en">
<!--<![endif]-->
```
