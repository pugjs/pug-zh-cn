# Case

The case statement is a shorthand for JavaScript's `switch` statement and takes the following form:

```pug-preview
- var friends = 10
case friends
  when 0
    p you have no friends
  when 1
    p you have a friend
  default
    p you have #{friends} friends
```

## Case Fall Through

You can use fall through just like in a `switch` statement in JavaScript.

```pug-preview
- var friends = 0
case friends
  when 0
  when 1
    p you have very few friends
  default
    p you have #{friends} friends
```

The difference however is that while fall through happens whenever a `break` statement is not explicitly included in JavaScript, in Pug it only happens when a block is completely missing.

If you would like to not output anything in a specific case, add an explicit unbuffered `break`:

```pug-preview
- var friends = 0
case friends
  when 0
    - break
  when 1
    p you have very few friends
  default
    p you have #{friends} friends
```

## Block Expansion

Block expansion may also be used:

```pug-preview
- var friends = 1
case friends
  when 0: p you have no friends
  when 1: p you have a friend
  default: p you have #{friends} friends
```
