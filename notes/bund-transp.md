# Bundling & Transpiling JS - ABRIDGED

## I. Install Node.js and the *Node Package Manager* (`npm`) - *if you need to*
 
 **1) Change directory to your project folder**
 
- In your command prompt application (Terminal, GitBash, etc), `cd` *into* the **project** folder

 **2) Create a node project with `npm`**
 
 - Type: 
 
 ```js
 npm init -y
 ```
 
 - This will create your **package.json** file with default *metadata* about your project, in the JSON format, which should look something like this:
 
 ```js
 {
  "name": "project-name",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
 ```
- Note that the default `name` of the project is the name of the folder that **package.json** is contained in. You can change this if you wish

<hr>
 
**3) Next we need to install the `webpack` and `webpack-cli` modules to this folder**

```js
npm install webpack --save-dev
```

**and**

```js
npm install webpack-cli --save-dev
```

- Which will download all of the modules you will need - check out the `node_modules` folder in your project directory
- the added `--save-dev` flags will also add `"devDependencies"` keys to *package.json* - which makes it easy for you to re-download these libraries later:
  - note: your version #'s will likely be different, so DO NOT modify what `npm` generated for you

```js
"devDependencies": {
  "webpack": "^5.89.0",
  "webpack-cli": "^5.1.4"
}
```
  

**PS - You will now also see a file named *package-lock.json* - you won't be editing it**

<hr>
 
**4) Create a new file named *webpack.config.js*** (in the **greeter-modules** folder)

- It needs to look like this:

**webpack.config.js**

```js
module.exports = {
  mode: 'development',
  entry: ['./src/main.js'],  // <-- make sure that this line matches the path to your starting js file
  output: {
    filename: './bundle.js'
  }
};
```
- Note, there is more to add below in order to transpile as well as bundle.
<hr>
 
**5) Modify *package.json***

- Open up **package.json** and make the "scripts" key look like this:

```js
"scripts": {        
  "start": "npm run webpack",
  "webpack": "webpack --watch"
},
```

- This custom `start` command will run webpack
- the `-watch` flag tells webpack to "watch" for any changes in the JS files and when we save a change to disk, webpack will automatically re-build the **bundle.js** file for us

<hr>
 
**6) Run npm!**

- Type: 

```js
npm start
```
- The above executes `webpack --watch` for you
- You should now see that **dist/bundle.js** has been created!
- If you open **bundle.js**, you will see that your 2 JavaScript files have been "bundled" into it
  - and it's not very pretty in "development mode", either!

<hr>
 
**7) Edit your HTML file**

In **greeter.html** - make the `<script>` tag look like this:

```html
<!-- <script type="module" src="src/main.js"></script> -->
<script src="dist/bundle.js"></script>  // <-- move this line to the bottom of the page (just inside the closing body tag).
```

- we are now pointing the `<script>` tag at the compiled JS file at **dist/bundle.js** rather than at **loader.js**
- note that this is a regular non-module JavaScript file now, so we don't need `type="module"` any longer

<hr>

**9) Distribution**

When you post this to the web:

- you need the HTML file, **dist/bundle.js** (and your **images** folder, **styles** folder etc if applicable)
- you don't need your `src` folder - because all that ES6 has been compiled down to ES5 and put into *bundle.js*
- you don't need any of the other of the other configuration files or **package.json** or the **node_modules** folder
- PS - this transpiled code will also run off of the desktop - it no longer needs a web server to function

<hr>
 
## II. <a id="section8"> NOW TO ALSO TRANSPILE:

- In **webpack.config.js**, change the `"mode"` from `"development"` to `"production"`
- Quit node in the console (if it's running) with control-c
- Verify that the *current working directory* is the **project** folder
- To install **`babel-loader`**:
  - type: `npm install --save-dev babel-loader @babel/core @babel/preset-env`
  - then check **package.json** to see the `dev-dependencies:` that were added
  - and see that **node_modules** just got bigger with more babel-related files
- Make the **webpack.config.js** look like this.

```js
module.exports = {
  mode: 'development',
  entry: ['./src/main.js'],  // <-- make sure that this line matches the path to your starting js file
  output: {
    filename: './bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
          },
        },
      },
    ],
  }
};

```

<hr>

- Type `npm start` to load the **webpack.config.js** changes 
- Make sure that Greeter still works

- Don't forget to actually use the transpiled JS by adding the `<script src="dist/bundle.js"></script>` tag to the bottom of your HTML page
- Verify that everything still works by temporarily deleting your **src** folder (bring it back after you've done the test)
- ***IMPORTANT -->*** Delete your **node_modules** folder

- PS - If you end up putting this up on banjo, you can (and should) also delete the following files, because they are not needed: **package.json**, **package-lock.json** and **webpack.config.js** (you won't need the **src** folder on banjo either)
