# Conditionals

Pug's first-class conditional syntax allows for optional parenthesis, and you may now omit the leading `-` otherwise it's identical, still just regular javascript:

```pug-preview
- var user = { description: 'foo bar baz' }
- var authorised = false
#user
  if user.description
    h2.green Description
    p.description= user.description
  else if authorised
    h2.blue Description
    p.description.
      User has no description,
      why not add one...
  else
    h2.red Description
    p.description User has no description
```

Pug also provides a negated version `unless` (the following are therefore equivalent):

```pug-preview-advanced readonly
\\\\\\\\\\ a.pug <
unless user.isAnonymous
  p You're logged in as #{user.name}
\\\\\\\\\\ b.pug >
if !user.isAnonymous
  p You're logged in as #{user.name}
```
