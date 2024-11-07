# HW-3 - Web App refactor

<details>
<summary>Table of Contents</summary>
  
1. [Overview](#overview)
2. [Choose a Web App to Refactor](#i-choose-a-web-app-to-refactor)
3. [Refactor the App's HTML](#ii-refactor-the-apps-html-10)
4. [Refactor the JS](#iii-refactor-the-js-10)
5. [Refactor the CSS with Bulma and Add an about.html Page](#iv-refactor-the-css-with-bulma-and-add-an-abouthtml-page-35)
6. [Choose a Language to Code In](#v-choose-a-language-to-code-in-0-15)
7. [Bundle the Code](#vi-bundle-the-code-15)
8. [The App Still Works](#vii-the-app-still-works-15)
9. [Documentation](#viii-documentation)
10. [Submission](#ix-submission)

</details>

---

## Overview
- For this HW you will choose a web app you previously created in IGME-235 or IGME-330, and will update the UI and refactor the code

---

## I. Choose a web app to refactor
- There are two "pre-approved" web apps you may use:
  - IGME-330's [HW-2 - Audio Visualizer - Ultimate Version](../hw/hw-2.md)
  - IGME-235's [Project 2 - Web Service Application](https://github.com/rit-igm-web/igme-235-shared/blob/main/projects/project-2.md)
- ***If there is a different project you would rather refactor, ask (well) in advance of the due date***
  - you must DM me in Slack to document this permission
- Put everything in a folder named ***coder-a*-hw3-refactor** (with ***coder-a*** replaced by your actual last name and first initial)
  
--- 

## II. Refactor the app's HTML (10%)
- The app's HTML file shall be named **index.html**
- Make sure that the HTML inside **index.html** follows our course naming standards ("kebab-case" for `id` and `class` names, etc)
- The HTML must pass the validator at: https://validator.w3.org/
- All files (.html, .js, .css etc) must follow [course naming standards](../notes/code-style-required-330.md) (kebab-case, no spaces etc)
- ***If you are refactoring HW-2 - Audio Visualizer, this will not take too long***
---

## III. Refactor the JS (10%)
- All regular functions (including anonymous functions) will be refactored to utilize `const` and arrow function syntax
- Variable and function names must follow course naming standards
- All code will be located in ES6 module files that use `import` and/or `export` as we have been doing all semester
  - if needed, refactor the code to "separate concern" into specific logical modules (meaning, separate .js files that utilize `export` and/or `import`)
  - ES6 `class` declarations ***MUST*** be in their own distinct file (ex. **CircleSprite.js** or **RectangleSprite.js** etc...)
- ***If you are refactoring HW-2 - Audio Visualizer, this will not take too long***
  
---

## IV. Refactor the CSS with Bulma and add an about.html page (35%)
- Use [PE-10 - Bulma I - Intro to Bulma](../pe/pe-10.md) as a "starter" page
- Reimplement the app UI in the starter page
- See [HW3 - Bulma Requirements](hw3-bulma-requirements.md)

---

## V. Choose a language to code in (0-15%)

- **Option #1** - ES6 "Vanilla" JavaScript (max grade for this option is 0%)
  - just do what we've been doing all semester (ES6 modules, arrow functions, `let` & `const` etc)
- **Option #2** - TypeScript (max grade for this option is 15%)
  - Review:
    - [Intro to TypeScript](../notes/intro-typescript.md)
    - [Checkoff - TypeScript Practice](../checkoffs/typescript-practice.md)
    - Make ***substantial*** use of TypeScript in your code: interfaces, enums and strongly typed variables, function parameters, and function return types where needed
    - ***This walks through converting the Audio Visualizer over to TypeScript: [HW-3 - Converting an existing project to TypeScript](hw3-typescript-notes.md)***
- **Option #3** - React (max grade for this option is 15%)
  - you probably want to start out by bootstrapping a new project with Vite:
    - [Intro to React#v-bootstrapping-a-react-project](../notes/react-intro.md#v-bootstrapping-a-react-project-the-easy-way)
  - recommendtions:
    - if you want to try out this option (React), consider doing so on the Web Service Application, NOT the Audio Visualizer
    - start out by first trying to get everything working in a single `App` component, and then decompose/re-factor the working code into other components

---

## VI. Bundle the code (15%)
- For **option #1** (ES6 Vanilla JS) above, be sure to transpile the code to ES5 and bundle it per these instructions: [Bundling & Transpiling JS](../notes/bundling-transpiling.md)
- For **option #2** (TypeScript) above, be sure to transpile the code to ES5 and bundle it per these instructions: [Intro to TypeScript](../notes/intro-typescript.md)
- For **option #3** (React) above, be sure to transpile the code to ES5 and bundle it per these instructions: [Intro to React#v-bootstrapping-a-react-project](../notes/react-intro.md#v-bootstrapping-a-react-project)
  
---

## VII. The app still works (15%)

- The app should now:
  - look good and be mobile friendly thanks to your HTML/CSS skills, and Bulma
  - be written in Modern JavaScript (ES6, TypeScript or React JSX)
  - meets the [IGME-330 Course Code Style Requirements]((../notes/code-style-required-330.md) )
  - has all of its code bundled into a single file - **bundle.js** - that is linked to the **index.html** file
- And it still functions as it should, both locally and on the banjo web server, right?

---

## VIII. Documentation
- In **documentation.txt**:
  - tell us what language you coded in
  - for section V. options #2 & #3, describe the code changes you implemented
- (-10%) if not submitted
---

## IX. Submission
- Post the completed HW-3 to the banjo web server:
  - Did you forget how to post to `banjo.rit.edu`? Get help here --> https://github.com/tonethar/IGME-235-Shared/blob/master/notes/core-skills/ftp-upload-walkthrough.md
  - don't post unneeded files/folders to banjo (**package.json**, **node_modules**, TypeScript config files, etc)
  - be sure to test the project on banjo and be 100% sure that it works 
- ZIP and Post to myCourses
  - 1) Your completed ***coder-a*-hw3-refactor** folder (be sure to DELETE the **node_modules** folder!)
  - 2) In the same ZIP - ALL the other tooling related files (**package.json**, TypeScript and webpack config files etc)
  - 3) The original version of the web app you re-factored:
    - put this in a folder named **coder-a-original** (using your last name & first initial)
    - IF you refactored *HW-2 - Audio Visualizer - Ultimate Version*, I already have that, so don't worry about re-submitting it
  - **documentation.txt**:
- In the myCourses dropbox comments field, post the link to the project on banjo



