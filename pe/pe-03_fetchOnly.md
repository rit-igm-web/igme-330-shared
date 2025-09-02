# Exercise: Review `fetch` & Parsing CSV/JSON/XML (aka PE-03)

- There are quite a few topics covered in this exercise, some are review, and some are new.
- We will be using the modern `fetch` API instead of the older `XMLHttpRequest` (XHR) approach.

## I. Loading and parsing CSV files with `fetch`

### Overview
- In this assignment, we are going to walk through using the browser [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) API to download data and display it to the user
- We will download and parse data in multiple formats - CSV, JSON, and XML
- `fetch` is the modern replacement for `XMLHttpRequest` and uses promises, making it easier to work with
- Topics covered:
  - `fetch` API - promises, `.then()`, `.catch()`, `response.text()`
  - Parsing CSV data with `String.split()`
  - Creating HTML with `array.map()`, `array.join()` and arrow functions
  - Using the Web Inspector (specifically the Elements tab) to see the "live" version of the DOM

### Start files
- For today's exercise, we will load in a CSV data file of pet names, and display them on the screen in ordered lists
- Below is your CSV data file **pet-names.csv** - it consists of popular dog and cat pet names on separate lines
- CSV stands for "**C**omma **S**eparated **V**alues" - a very popular data format
- Go ahead and create a **data** folder and place the **pet-names.csv** file into it:

**data/pet-names.csv**
```text
Bella,Luna,Charlie,Lucy,Cooper,Max,Bailey,Daisy,Sadie,Lola,Buddy,Molly,Stella,Tucker,Bear,Zoey,Duke,Harley,Maggie,Jax
Oliver,Leo,Milo,Charlie,Simba,Max,Jack,Loki,Tiger,Jasper,Ollie,Oscar,George,Buddy,Toby,Smokey,Finn,Felix,Simon,Shadow
```

- Here is our HTML file that has the HTML/CSS/JS to get us started:

**fetch-get-csv.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
	<title>Fetch - Load CSV</title>
	<style>
	body{
	  font-family: sans-serif;
	}
	</style>
</head>
<body>
	<h2>Fetch - Load CSV File</h2>
	<p>The <code>pet-names.csv</code> file contains popular dog and cat pet names on separate lines, separated by commas.</p>
	<p>Note that because fetch is loading a local file, this example will have to be run off a web server rather than from your computer's hard drive (e.g. banjo.rit.edu or VSCode's liveserver etc)</p>
	
	<hr>
	<button id="my-button">Load Data</button>
	<p id="output">Click button to load</p>
	
<script>
const loadCSVFile = () => {
  fetch('./data/pet-names.csv')
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.text()
    })
    .then(text => {
      console.log('Success:', text);
      
      // TODO: You implement the parsing and display code here!
      // See step-by-step instructions below
    })
    .catch(error => {
      console.error('Error:', error);
      document.querySelector('#output').innerHTML = `<p>Error loading file: ${error.message}</p>`;
    });
};

document.querySelector('#my-button').addEventListener('click', loadCSVFile);
</script>
</body>
</html>
```

### Your Implementation Tasks

**Step 1: Test the basic fetch**
- Load the page and click the button
- Check the browser console - you should see the pet names string logged
- The file loads successfully, but nothing displays yet

**Step 2: Parse the CSV data**
CSV data has names on separate lines. Use [String.split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) to:
1. Split by newlines (`\n`) to get each row
2. Split each row by commas (`,`) to get individual names

Be sure to `console.log()` the results at each step, then check the browser console. 

**Step 3: Create HTML from the arrays**
Convert the name arrays into HTML lists:
```javascript
// Create HTML for dog names
const dogHTML = `<h3>Dog Names</h3><ol>${...something goes here...}</ol>`;

const catHTML = ...;

// Display the HTML on the page
document.querySelector('#output').innerHTML = dogHTML + catHTML;
```


### Key Fetch Concepts
- `fetch()` returns a Promise
- Use `.then()` to handle successful responses
- Use `.catch()` to handle errors
- Check `response.ok` to verify the request succeeded
- Use `response.text()` to get the text content (also returns a Promise)

Make sure that your files are in a ***containing folder*** named  **lastName-firstInitial-csv-1**

---

## II. Loading and parsing JSON files with `fetch`

### Overview
- We will build on what we did last time by modifying our CSV loading code to instead download and parse a JSON ("**J**ava**S**cript **O**bject **N**otation") file
- JSON is the most popular data format for web APIs and modern web applications
- Additional topics covered:
  - Working with JSON - JavaScript Object Notation
  - `response.json()` method for automatic JSON parsing
  - Object destructuring assignment of variables

### Start files
- You will want to start by first making a copy of your **lastName-firstInitial-csv-1** folder from above, and naming the copy **lastName-firstInitial-json-2**
- Go ahead and rename your completed HTML file from last time - from **fetch-get-csv.html** to **fetch-get-json.html** 
- Below is the JSON data file **pet-names.json** that we will use this time
- **pet-names.json** contains popular dog and cat pet names in a structured JSON format
- Go ahead and create the **pet-names.json** file and put it in your **data/** folder

**data/pet-names.json**
```json
{
  "categories": {
    "dogs": ["Bella","Luna","Charlie","Lucy","Cooper","Max","Bailey","Daisy","Sadie","Lola","Buddy","Molly","Stella","Tucker","Bear","Zoey","Duke","Harley","Maggie","Jax"],
    "cats": ["Oliver","Leo","Milo","Charlie","Simba","Max","Jack","Loki","Tiger","Jasper","Ollie","Oscar","George","Buddy","Toby","Smokey","Finn","Felix","Simon","Shadow"]
  }
}
```

### Your Implementation Tasks for JSON

**Step 1: Update the HTML file**
- Copy your **lastName-firstInitial-csv-1** folder and rename to **lastName-firstInitial-json-2**
- Rename **fetch-get-csv.html** to **fetch-get-json.html**
- Update the function name from `loadCSVFile` to `loadJSONFile`
- Update the fetch URL from `'./data/pet-names.csv'` to `'./data/pet-names.json'`

**Step 2: Parse JSON with response.json()**
JSON parsing is much easier than CSV! Update your fetch chain:
```javascript
.then(response => {
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  return response.json(); // Changed from response.text()!
})
.then(data => {
  console.log('Success:', data);
  
  // TODO: Add object destructuring and display code here
})
```

**Step 3: Use object destructuring**
Extract the dog and cat arrays from the JSON object. Use destructuring syntax to create a `dogs` variable and a `cats` variable.
```javascript
// Object destructuring (for example)
const someData = {
  a: 1,
  b: [ 2, 3, 4 ]
};
const { a, b } = someData;
const {b: [two, three, four]} = someData;
console.log(a, b, two, three, four);
```

**Step 4: Create and display HTML**
Generate the HTML lists (same as before).

**Step 5: Do birds**
Go ahead and modify the code so that it displays a list of pet bird names. Find 20 popular names for pet birds, and add them as a new property to the **pet-names.json** file:
```json
{
  "categories": {
    "dogs": [...],
    "cats": [...],
    "birds": ["Tweety", "Polly", "Charlie", ...]
  }
}
```
Update your destructuring code to extract the `birds` array, and display those bird names as an ordered list underneath another `<h3>` subheading.

Make sure that your files are in a ***containing folder*** named  **lastName-firstInitial-json-2**

---

## III. Loading and parsing XML files with `fetch`

### Overview
- We will build on what we did last time by modifying our CSV loading code to instead download and parse an XML ("E**x**tensible **M**arkup **L**anguage") file.
- Additional topics covered:
  - Authoring custom XML files
  - 5 rules for *well-formed* XML
  - `response.text()` for XML content
  - Parsing an XML file with `DOMParser`
  - Writing "guard" code

### About XML
- "***Extensible Markup Language (XML)** is a markup language and file format for storing, transmitting, and reconstructing arbitrary data. It defines a set of rules for encoding documents in a format that is both human-readable and machine-readable.*"
- XML is a set of rules for defining your own markup language
- The top 5 rules for writing *well-formed* XML are:
  - All XML elements must have a closing tag
  - XML tags are case sensitive
  - All XML elements must be properly nested
  - All XML documents must have a root element
  - Attribute values must always be quoted

### Start files
- You might want to start by first making a copy of your **lastName-firstInitial-fetch-2** folder from last time, and naming the copy **lastName-firstInitial-fetch-3**
- Go ahead and rename your completed HTML file from last time - from **fetch-get-csv.html** to **fetch-get-xml.html** 
- Below is the XML data file **pet-names.xml** that we will use this time
- **pet-names.xml** contains popular dog and cat pet names that are "marked up"
- Note that the dog and cat names can now be distinguished by looking at the `cid` attribute (which stands for "category ID")
- Go ahead and create the **pet-names.xml** file and put it in your **data/** folder

- Start by first making a copy of your **lastName-firstInitial-csv-1** folder from above, and name the copy **lastName-firstInitial-xml-3**.
- Rename your completed HTML file from last time - from **fetch-get-csv.html** to **fetch-get-xml.html** 
- Below is the JSON data file **pet-names.xml** that we will use this time; put it in your **data/** folder

**data/pet-names.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<petnames>
  <namelist cid="dognames">Bella,Luna,Charlie,Lucy,Cooper,Max,Bailey,Daisy,Sadie,Lola,Buddy,Molly,Stella,Tucker,Bear,Zoey,Duke,Harley,Maggie,Jax</namelist>
  <namelist cid="catnames">Oliver,Leo,Milo,Charlie,Simba,Max,Jack,Loki,Tiger,Jasper,Ollie,Oscar,George,Buddy,Toby,Smokey,Finn,Felix,Simon,Shadow</namelist>
</petnames>
```

### Your Implementation Tasks for XML

**Step 1: Set up the XML version**
- Copy your **lastName-firstInitial-json-2** folder and rename to **lastName-firstInitial-xml-3**
- Rename **fetch-get-json.html** to **fetch-get-xml.html**
- Update the function name from `loadJSONFile` to `loadXMLFile`
- Update the fetch URL from `'./data/pet-names.json'` to `'./data/pet-names.xml'`

**Step 2: Parse the XML string**
XML requires a special parser and you need to change back to `response.text()`:
```javascript
.then(response => {
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  return response.text(); // Changed back from response.json()!
})
.then(xmlString => {
  console.log('Success:', xmlString);
  
  // Create a DOM parser and parse the XML
  const parser = new DOMParser();
  const xmlDoc = parser.parseFromString(xmlString, 'text/xml');
  
  console.log('Parsed XML:', xmlDoc);
  
  // TODO: Add guard code and element selection here
})
```

**Step 3: Add guard code**
XML parsing can fail - always check for errors:
```javascript
// Guard code - check for parser errors
if (xmlDoc.querySelector('parsererror')) {
  throw new Error('XML parsing error - check your XML file format');
}

console.log('XML parsed successfully');
```

**Step 4: Select the XML elements**
Now find all the `<namelist>` elements:
```javascript
// Get all namelist elements
const namelists = xmlDoc.querySelectorAll('namelist');
console.log('Found namelists:', namelists);

// We'll process these in the next step
let dogHTML = '';
let catHTML = '';
```

**Step 5: Process each namelist element**
Loop through the elements and extract data based on their `cid` attribute. Then fill in the code below to create the names variable (an array of the animal names in that list) and the corresponding HTML, as done above. 
```javascript
namelists.forEach(namelist => {
  // Get the category ID attribute
  const cid = namelist.getAttribute('cid');
  console.log('Processing category:', cid);
  
  // Get the text content and split into names
  const namesText = nameList.textContent;
  const names = ...
  
  // Create the list HTML
  const listHTML = ...
  
  // Assign to appropriate category
  if (cid === 'dognames') {
    dogHTML = `<h3>Dog Names</h3>${listHTML}`;
  } else if (cid === 'catnames') {
    catHTML = `<h3>Cat Names</h3>${listHTML}`;
  }
});

// Display the results
document.querySelector('#output').innerHTML = dogHTML + catHTML;
```

### Key XML Parsing Concepts
- Use `DOMParser` to parse XML strings
- Always check for parsing errors with "guard code"
- Use `querySelector` and `querySelectorAll` on the parsed XML document
- Access attributes with `getAttribute()`
- Access text content with `textContent`

Make sure that your files are in a ***containing folder*** named  **lastName-firstInitial-xml-3**

---

## IV. Submission
- Put the 3 folders above into a parent folder named **lastName-firstInitial-pe03**
- ZIP and post this to myCourses
- Your folder structure should look like:
  ```
  lastName-firstInitial-pe03/
  ├── lastName-firstInitial-csv-1/
  │   ├── fetch-get-csv.html
  │   └── data/
  │       └── pet-names.csv
  ├── lastName-firstInitial-json-2/
  │   ├── fetch-get-json.html
  │   └── data/
  │       └── pet-names.json
  └── lastName-firstInitial-xml-3/
      ├── fetch-get-xml.html
      └── data/
          └── pet-names.xml
  ```

---

## V. Reference

### APIs Used
- [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) - modern API for making HTTP requests, returns Promises
- [`response.text()`](https://developer.mozilla.org/en-US/docs/Web/API/Response/text) - reads the response body as text
- [`response.ok`](https://developer.mozilla.org/en-US/docs/Web/API/Response/ok) - boolean indicating if response was successful
- [`DOMParser`](https://developer.mozilla.org/en-US/docs/Web/API/DOMParser) - parses XML and HTML strings into DOM documents
- [`Array.map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) - creates new array with transformed elements
- [`Array.join()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) - joins array elements into a string
- [`String.split()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) - splits string into array
- [`String.trim()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trim) - removes whitespace

### Key Concepts
- **Promises** - asynchronous operations that can be chained with `.then()`
- **Array Destructuring** - `const [first, second] = array;`
- **Template Literals** - backtick strings with `${variable}` interpolation  
- **Arrow Functions** - concise function syntax `() => {}`
- **Guard Code** - checking for errors before proceeding
- **DOM Manipulation** - creating and updating HTML elements