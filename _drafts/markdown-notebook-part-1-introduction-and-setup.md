---
layout:     post
title:      A Markdown Notebook app - Part 1 Introduction and Setup
# date:       2022-03-19 18:00:00
summary:    TBD...
categories: typescript react electron markdown
---

## Table of Contents

- Part 1 - Introduction and setup (current)
- Part 2 - Creating a File Tree
- Part 3 - Previewing Files
- Part 3 - Creating and Updating Files
- Part 4 - Deleting Files

## Initializing with electron-forge

- electron forge docs: https://www.electronforge.io/templates/typescript-+-webpack-template
  - Use the `TypeScript + Webpack` template:

```
npx create-electron-app file-explorer-demo --template=typescript-webpack
cd file-explorer-demo
```

### Why TypeScript and Webpack?

**TypeScript** is JavaScript with added features for static typing. Languages that are statically require that variable types are known at compile time instead of discovered at run time, like in dynamically typed languages. This makes it perfect for mission-critical software applications, though the trade-off is that it often adds complexity to projects. However with that complexity comes:

- an easier time catching type errors
- easier to refactor code
- an easier time building more stable large and complex projects

This makes it a great choice for larger and complex JavaScript projects like desktop applications. With Typescript you also get:

- Bleeding edge JavaScript features that can be transpiled back to a version of JavaScript supported by most versions
- Tight editor integration and tooling like linting and IntelliSense
- Self documenting code and data
- Gradual adoption, plain JavaScript is still valid TypeScript

There are some drawbacks, the main two so far being:

- Added complexity and setup, so it might be suited for smaller projects
- Not all JavaScript libraries have created TypeScript types, but most popular ones have and can be found in the DefinitelyTypes repository on GitHub

**Webpack** is a JavaScript static module bundler. It combines all a website's javascript and its dependencies (libraries, images, styles, etc.) and combines them into a single, or small number of, static file(s) speeding up website load times. Bundling by itself is not so important for desktop applications because the dependencies will most likely be packaged with the rest of the application. But webpack also comes with:

- Tree-shaking: removes unused library code which shrinking the size of your application.
- Hot module replacement: drastically speeds up development time by updating views as you are developing.

### Add React

```
npm install --save react react-dom
npm install --save-dev @types/react @types/react-dom
```

By following the [framework integration guide](https://www.electronforge.io/guides/framework-integration/react-with-typescript) on the electron-forge wwbsite, we are told we need two things:
1. `src/App.tsx` file (I made some updates to the recommendation to make it feel more react-y)
2. updated `src/render.tsx` file

```typescript
// src/App.tsx
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
    return(
        <h2>Hello from React!</h2>
    )
}

ReactDOM.render(, document.body);
```

```typescript
// src/renderer.ts
import './app';
```

---

Source code for this post: [GitHub Repository - tag: setup]
