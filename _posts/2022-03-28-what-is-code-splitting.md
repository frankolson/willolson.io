---
layout:     post
title:      What is Code Splitting?
date:       2022-03-28 12:30:00 -0400
categories: JavaScript, Web Development
---

Code splitting allows javascript bundlers like [webpack](https://webpack.js.org) to create multiple bundles for a single code base. There are two main use-cases for this, both of which reduce the amount of static file downloading clients have to do, increasing the speed of the websites.

The first use case is creating separate bundles for different parts of a web application. For example, the landing page portion of a website could have a different bundle than the admin section. This means clients won't have to download any the javascript only used in the admin sections of the site, when visiting the landing pages.

The second use case takes advantage of browser caching. A vendor.js bundle can be created, containing all of the javascript and dependencies that developers don't think will be changed very often. A client's browser can then download it once, and not need to download it again for a while. Every following request of the site from that client would then just download the smaller javascript files that are more likely to change.

## Further reading

- [Demystifying Code Bundling with JavaScript Modules](https://www.simplethread.com/javascript-modules-and-code-bundling-explained/)
- [webpack - Code Splitting](https://webpack.js.org/guides/code-splitting/)
