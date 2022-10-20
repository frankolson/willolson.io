---
layout:     post
title:      What is Module Lazy Loading?
date:       2022-03-28 13:00:00 -0400
categories: JavaScript, Web Development
---

Lazy loading is a pattern where code is only loaded or initialized the first time it is needed. Bundlers like [webpack](https://webpack.js.org) allow for module lazy loading, which uses this pattern to reduce the size of bundles by not including rarely used dependencies in the final bundle. Instead, references to those dependencies are wrapped in [JavaScript promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that are triggered only when used by the client.

## Further reading

- [Demystifying Code Bundling with JavaScript Modules](https://www.simplethread.com/javascript-modules-and-code-bundling-explained/)
- [webpack - Lazy Loading](https://webpack.js.org/guides/lazy-loading/)
