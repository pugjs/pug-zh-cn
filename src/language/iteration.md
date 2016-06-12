# Iteration

Pug supports two primary methods of iteration, `each` and `while`.

## each

Pug's first-class iteration syntax makes it easier to iterate over arrays and objects within a template:

```pug-preview
ul
  each val in [1, 2, 3, 4, 5]
    li= val
```

You can also get the index as you iterate:

```pug-preview
ul
  each val, index in ['zero', 'one', 'two']
    li= index + ': ' + val
```

Pug also lets you iterate over the keys in an object:

```pug-preview
ul
  each val, index in {1:'one',2:'two',3:'three'}
    li= index + ': ' + val
```

The object or array to iterate over is just plain JavaScript so it can be a variable or the result of a function call or almost anything else.

```pug-preview
- var values = [];
ul
  each val in values.length ? values : ['There are no values']
    li= val
```

One can also add an `else` block that will be executed if the array or object does not contain values to be iterated over. The following is equivalent to the example above:

```pug-preview
- var values = [];
ul
  each val in values
    li= val
  else
    li There are no values
```

You can also use `for` as an alias of `each`.

## while

You can also use `while` to create a loop:

```pug-preview
- var n = 0;
ul
  while n < 4
    li= n++
```
