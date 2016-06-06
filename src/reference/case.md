# Case

The case statement is a shorthand for JavaScript's `switch` statement and takes the following form:

```pug-demo
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

```pug-demo
- var friends = 0
case friends
  when 0
  when 1
    p you have very few friends
  default
    p you have #{friends} friends
```

## Block Expansion

Block expansion may also be used:

```pug-demo
- var friends = 1
case friends
  when 0: p you have no friends
  when 1: p you have a friend
  default: p you have #{friends} friends
```
