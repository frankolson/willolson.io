---
layout:     post
title:      What is Tree Shaking?
date:       2022-03-28 12:00:00 -0400
categories: JavaScript, Web Development
---

Tree shaking is where javascript bundlers like [webpack](https://webpack.js.org/) analyze the dependencies of a project and remove any unused features prior to bundling. For example, [React Router](react-router-dom) has a lot of functionality but a developer might only want to use one feature from it (e.g. `import { Route } from 'react-router-dom';`). Bundlers implementing tree shaking can recognize this and only include the used features in the final bundle, drastically reducing its size.

The downside is that tree shaking can only be used if the dependencies are configured properly or are written using ECMAScript modules. Without this, bundlers wouldn't know what to remove without side effects.

## Further reading

- [Demystifying Code Bundling with JavaScript Modules](https://www.simplethread.com/javascript-modules-and-code-bundling-explained/)
- [webpack - Tree Shaking](https://webpack.js.org/guides/tree-shaking/)
