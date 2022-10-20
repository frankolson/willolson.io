---
layout:     post
title:      What is Hot Module Replacement?
date:       2022-03-28 13:30:00 -0400
categories: JavaScript, Web Development
---

Its predecessor being live reloading, hot module replacement swaps out individual modules in real time during development instead of watching files and reloading the entire site. Because webpages aren't being reloaded, this means that:
- Application state is saved during module replacements
- The page only updates the parts that have changed
- Changes to assets like CSS are fast, almost as fast as making updates directly in the developer tools of the browser

## Further reading

- [webpack - Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/)
