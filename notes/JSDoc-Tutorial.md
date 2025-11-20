> Note this was developed Spring 2025, it is pretty brief and hasn't been refined but seems to be working just fine as of Fall 2025 so adding it here!

# 🧪 JSDoc + `@ts-check` Demo (10–15 min)

This tutorial demonstrates how to use **JSDoc** to add smart hints to your functions and how to generate **HTML documentation**. It also shows how to use `@ts-check` for lightweight static type checking in JavaScript.

---

### 📁 1. Create Folder Structure

Set up a new folder:

```
jsdoc-demo/
└── src/
    └── test.js
```

---

### ✏️ 2. Create a Basic Function in `test.js`

Start with a simple function without any documentation:

```js
function add(a, b) {
  return a + b;
}

add(); // Try hovering — no hints!
```

---

### 💬 3. Add JSDoc Comments

Update the function with JSDoc comments:

```js
/**
 * Adds two numbers together.
 * @param {number} a - The first number
 * @param {number} b - The second number
 * @returns {number} The sum of a and b
 */
function add(a, b) {
  return a + b;
}

add(); // Now VS Code shows parameter and return type hints!
```

---

### 🧠 4. Use `@ts-check` for Lightweight Type Checking

Add this at the top of `test.js`:

```js
// @ts-check

/**
 * @type {number | string}
 */
let price = 12;

// This line will trigger a red squiggly underline in VS Code:
price = []; // ❌ Type '[]' is not assignable to type 'number | string'
```

---

### 📦 5. Initialize a Node Project and Install JSDoc

From the project root folder, run:

```bash
npm init -y
npm install --save-dev jsdoc
```

---

### ⚙️ 6. Create `jsdoc.json`

In the root folder, add a file called `jsdoc.json` with this content:

```json
{
  "source": {
    "include": ["src"],
    "exclude": ["node_modules"],
    "includePattern": ".+\\.js(doc|x)?$",
    "excludePattern": "(^|\\/|\\\\)_"
  },
  "opts": {
    "destination": "./docs"
  }
}
```

---

### 📄 7. Generate HTML Documentation

Run this command to generate your documentation:

```bash
npx jsdoc -c jsdoc.json
```

This will create a `docs/` folder. Open `docs/index.html` in a browser to view your documentation.

---

### ⚡ 8. (Optional) Add a Script to `package.json`

To make it easier to generate docs in the future, open `package.json` and add this under `"scripts"`:

```json
"scripts": {
  "docs": "jsdoc -c jsdoc.json"
}
```

Now you can run:

```bash
npm run docs
```

---

### ✅ Summary

- Use `/** ... */` JSDoc blocks for editor hints and autocomplete.
- Add `@ts-check` for built-in type validation in plain JavaScript.
- Use `npx jsdoc -c jsdoc.json` to generate beautiful HTML documentation.

Done! 🎉
