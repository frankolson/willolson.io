---
layout:     post
title:      What are JavaScript Modules?
date:       2022-03-31 09:00:00 -0400
categories: JavaScript, Web Development
---

A JavaScript module is just a JavaScript file. Modules allow a developer to break their script up into multiple files and exchange functionality between them.

JavaScript didn't have this functionality natively for some time because, in the beginning, scripts added to websites were small and manageable. This changed with the rise in popularity of larger modern web applications and server-side applications using technologies like [React](https://reactjs.org/) and [Node.js](https://nodejs.org/en/). The need for better code organization and easy use of third party libraries prompted the development of tools to support the concept of modules:
- Asynchronous Module Definition (AMD) from [require.js](https://requirejs.org/)
- CommonJS for Node.js
- [UMD](https://github.com/umdjs/umd) that supported both AMD and CommonJS

Since then, ECMAScript Modules were introduced in 2015 as the language standard and have been adopted by most major browsers.

Modules utilize `import` and `export` statements to share functionality between files. There are two types of exports, named and default.

**Named**
```javascript
// named inline
export const animal = 'dog';

// named all-at-once
const age = 5;
const color = 'auburn';

export {
  age,
  color
};
```

**Default**
```javascript
function eat() {
  const animal = 'dog';
  const name = 'Smalls';
  return name + ' ate their ' + animal + ' food.';
};

export default greeting;
```

The `import` statement handles these types of `export` statements slightly differently:
```javascript
import { animal, age, color } from './inline.js';
import greeting from './default';
```

## Further reading

- [Modules, introduction - javascript.info](https://javascript.info/modules-intro)
- [JavaScript Modules - W3Schools](https://www.w3schools.com/js/js_modules.asp)
- [Why webpack - Webpack](https://webpack.js.org/concepts/why-webpack/)
- [Demystifying Code Bundling with JavaScript Modules](https://www.simplethread.com/javascript-modules-and-code-bundling-explained/)
