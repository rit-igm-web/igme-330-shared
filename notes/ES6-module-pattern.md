# ES6 Module Pattern

<details>
  <summary>Table of Contents</summary>
	
- [I. Why do we need modularized code?](#section1)
- [II. ES6 Module Syntax](#section2)
- [III. Try it out!](#section3)
- [IV. Reference](#section4)
- [V. Review Questions](#section5)

</details>

<hr>

<a id="section1">

## I. Why do we need modularized code?
Applications that are written in a **modular** fashion are [loosely coupled](https://en.wikipedia.org/wiki/Loose_coupling), with minimal [dependencies](https://en.wikipedia.org/wiki/Dependency_hell) between modules, which makes the process of designing and maintaining them much easier and less error prone. 

**Modular programming** is the process of subdividing a computer program into separate sub-programs. Modules have the following characteristics:
- enforce logical boundaries between components and improve maintainability
- are implemented through *interfaces* (i.e. publicly available methods or properties)
- are designed in such a way as to minimize *dependencies* between different modules

Writing modular code has many benefits:
- it allows many programmers to collaborate on the same application because a small team deals with only a small part of the entire code
- the same code can be used in many applications
- errors can easily be identified, as they are localized to a file, class or function
- the scoping of variables can be easily controlled

The above is adapted from [https://www.techopedia.com/definition/25972/modular-programming](https://www.techopedia.com/definition/25972/modular-programming)

We have been getting away with writing "non modular" JavaScript code so far because our programs have been fairly small. But as the size of the program increases, and if more than one developer works on an application, a modular programming architecture becomes essential.

**Today we will apply ES6 module syntax to an application and see these benefits in action.**



<hr>

 <a id="section2">

## II. ES6 Module Syntax

[Exploring ES6](https://exploringjs.com/es6/ch_modules.html#sec_overview-modules) has a nice overview of ES6 modules:

- JavaScript has had modules for a long time - however, they were implemented via libraries, not built into the language
- ES6 is the first time that JavaScript has built-in modules
- ES6 modules are stored in files
- ***There is exactly one module per file and one file per module***
- ***All the variables and functions in a file are private to that file, unless explicitly exported***

### II-A. `export` and `import`
[export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) is used when creating JavaScript modules to export functions, objects, or primitive values from the module so they can be used by other programs with the import statement.

[import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) is used to import *bindings* (to functions, objects or primitive values) which are exported by another module.

<a id="important-restrictions"/>

### II-B. **Important Restrictions**

ES6 modules have specific restrictions that you need to be aware of:

1. **Browser Compatibility**: As of 2023, ES6 modules are supported by all major browsers from the last five years. For up-to-date compatibility information, refer to the following:
   - [MDN Compatibility](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#Browser_compatibility)
   - [Can I use ES6 modules?](https://caniuse.com/es6-module)

2. **Serving Modules**: Modules need to be served over HTTP/HTTPS and will not function correctly when opened directly from your file system due to browser security restrictions (CORS policies). Here’s how to get around this:
   - **VSCode Live Server**: This is the simplest method for most users. Follow this guide to set it up: [VSCode Live Server Guide](https://ritwickdey.github.io/vscode-live-server/).
   - **Python HTTP Server**: If using Python 3, start a server by running:
     ```bash
     python -m http.server
     ```
   - **Node.js and `http-server`**: Install and run with:
     ```bash
     npm install -g http-server
     http-server -c-1
     ```
   - **Other IDEs**: Most modern IDEs have a live preview feature that launches a local server.

<hr>

<a id="section3">
	
## III. Try it out!
	
- A zipped version is here --> [statz.zip](_files/statz.zip)
  - Remember than ES6 modules will only run off of a web server (VSCode's "Live Server" is the easiest way to do this)
  - Utilizing ES6 modules means you have less code floating around in global or "script" scope
    - The "no module" version of Statz has 13 variables and functions in *script scope*
    - But the "module" version of Statz has ONLY 6 variables and functions in *module scope*
- Now try out converting an app to ES6 modules --> [doubleIt.zip](_files/doubleIt.zip)

<hr>

<a id="section4">
  
## IV. Reference

### Key Concepts
- **Namespace**: A namespace is a container that holds a set of identifiers and allows the disambiguation of items that may otherwise have the same name. Learn more here: [Namespace](https://en.wikipedia.org/wiki/Namespace).

### ES6 Module Resources
- [MDN: `import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [MDN: `export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
- [Exploring ES6 Modules](https://exploringjs.com/es6/ch_modules.html#sec_mixing-named-and-default-exports)
- [Caniuse: ES6 Modules](https://caniuse.com/es6-module) - Check module compatibility with different browsers.
- [Introduction to ES Modules](https://jakearchibald.com/2017/es-modules-in-browsers/) - A practical guide on ES Modules in browsers.


<hr>

<a id="section5" />

## V. Review Questions
1. Define the software development term [*loosely coupled*](https://en.wikipedia.org/wiki/Loose_coupling).
2. Give 3 advantages of using modules when coding substantial JavaScript applications.
3. Is the `"use strict"` declaration necessary for ES6 modules to run in strict mode?

