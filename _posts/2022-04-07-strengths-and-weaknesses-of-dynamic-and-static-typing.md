---
layout:     post
title:      Strengths and Weaknesses of Dynamic and Static Typing
date:       2022-04-07 08:45:00 -0400
categories: Computer Science
---

Often in the world of programming, you will hear arguments over whether a language is good or bad based on whether or not it is strongly or dynamically typed. However, the reality is not so binary, and both type systems have their combination of strengths and weaknesses that make them better or worse for certain situations.

## Dynamic typing

Dynamically typed languages have a combination of strengths and weaknesses that make them better for hacking and prototyping or when you have a set of unknown or changing requirements.

### Strengths
- Lack of overhead from type declarations and definitions
- More concise and can be easier to read
- Duck typing and monkey patching allow for easier hacking (the good kind)
- No up-front design stage is needed, allowing for programming without a complete set of known requirements

### Weaknesses
- Duck typing and monkey patching may add confusion later
- Requires more extensive testing to reduce runtime error risks
- Runtime type checking leads to less performant code like increased memory and CPU usage

## Static typing

Statically typed languages have a combination of strengths and weaknesses that make them better for high performant or mission-critical software applications as well scaling projects that have a lot of contributors.

### Strengths
- Early detection of type errors
- Documentation of value types
- Increase runtime efficiency
- Easier ability to make large code refactors

### Weaknesses
- Often too much added complexity for smaller projects
- Tedious repetitive type definition and declaration work

## Further reading

- [Static Typing Where Possible, Dynamic Typing When Needed](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.69.5966&rep=rep1&type=pdf)
- [Developers Shift to Dynamic Programming Languages](https://sci-hub.se/10.1109/mc.2007.53)
- [Why You Should Choose TypeScript Over JavaScript](https://serokell.io/blog/why-typescript)
- [What is the supposed productivity gain of dynamic typing? - StackExchange Comment](https://softwareengineering.stackexchange.com/a/122212)
- [Static typing - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Static_typing)
- [Dynamic typing - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Dynamic_typing)
