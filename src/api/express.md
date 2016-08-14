---
title: Express Integration
template: generic
id: api/express
---

# Express Integration

Pug fully integrates with [Express], a popular Node.js web framework, as a supported view engine. Check out Express's excellent [guide][Express guide] for how to integrate Pug with Express.

## Production Defaults

In Express, the environmental variable `NODE_ENV` is designed to inform the web application of the execution environment: whether it is in development or in production. Express and Pug automatically modifies the defaults of a few options in production environment, to provide a better out-of-the-box experience for users. Specifically, when `process.env.NODE_ENV` is set to `'production'`, and Pug is used with Express, the <code>[compileDebug]</code> option is `false` by default, while the <code>[cache]</code> option is `true`.

To override the defaults for `compileDebug` and `cache`, you can set the respective property in `app.locals` or `res.locals` objects to `true` or `false`. The `cache` option can also be overriden through Express's `app.disable`/`enable('view cache')`. For more details, check Express's [API reference][Express API].

[compileDebug]: reference.html#options
[cache]: reference.html#options
[Express]: https://expressjs.com/
[Express API]: https://expressjs.com/en/api.html
[Express guide]: https://expressjs.com/en/guide/using-template-engines.html
