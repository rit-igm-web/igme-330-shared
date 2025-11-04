Note: this is a slightly reworked version of "react-intro.md" to utilize codeplayground more before we move on to VITE in which you can go back to those notes.

# React Intro - Student Guide

## Getting Started
1. Open https://react.new in your browser
2. Wait for the code playground to load
3. You'll see three panels: files (left), code editor (center), preview (right)

---

## Part 1: The Smiley Component

### What We're Building
A simple component that displays a smiley face emoji.

### Step 1: Clear the Canvas
Delete everything in `App.js` and start fresh.

### Step 2: Create the Smiley Component

```jsx
function Smiley() {
  return (
    <div style={{fontSize: '48px'}}>
      😊
    </div>
  );
}
```

**Key Points:**
- Component names must be **capitalized** (`Smiley` not `smiley`)
- Use parentheses `()` for multi-line JSX
- `style={{fontSize: '48px'}}` has **double curly braces**:
  - Outer `{}`: "I'm writing JavaScript in JSX"
  - Inner `{}`: "This is a JavaScript object"
- CSS properties use **camelCase**: `fontSize` not `font-size`

### Step 3: Use the Smiley Component

```jsx
function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Smiley />
    </div>
  );
}

export default App;
```

**Key Points:**
- Use components like HTML tags: `<Smiley />`
- Self-closing tag: `<Smiley />` (no closing tag needed)
- Must have a parent wrapper (the outer `<div>`)
- `export default App` makes this the main component

**Checkpoint:** You should see "My React App" and a smiley face! 😊

---

## Part 2: Header Component with Props

### What We're Building
A header component that can display different text (using props).

### Step 1: Create the Header Component

```jsx
function Header(props) {
  return (
    <header>
      <h1>{props.text}</h1>
    </header>
  );
}
```

**Key Points:**
- `props` is a parameter (an object containing data passed to the component)
- `{props.text}` uses curly braces to insert JavaScript into JSX
- The text will be passed in when we use the component

### Step 2: Update App to Use Header

```jsx
function App() {
  return (
    <div style={{padding: '20px', fontFamily: 'sans-serif'}}>
      <Header text="Famous Tigers" />
      <Smiley />
    </div>
  );
}

export default App;
```

**Key Points:**
- `text="Famous Tigers"` passes data to the Header component
- This becomes `props.text` inside Header
- Added some styling to the wrapper div for spacing and font

**Checkpoint:** You should see "Famous Tigers" as the header! 🐯

---

## Part 3: Footer Component with Destructuring

### Step 1: Create the Footer Component

```jsx
function Footer(props) {
  return (
    <footer style={{color: 'gray', marginTop: '20px'}}>
      <p>&copy; {props.year} {props.name}</p>
    </footer>
  );
}
```

**Key Points:**
- Footer takes two props: `year` and `name`
- `&copy;` is the HTML entity for ©
- Multiple style properties: `color` and `marginTop`

### Step 2: Add Footer to App

```jsx
function App() {
  return (
    <div style={{padding: '20px', fontFamily: 'sans-serif'}}>
      <Header text="Famous Tigers" />
      <Smiley />
      <Footer year={2024} name="Ace Coder" />
    </div>
  );
}

export default App;
```

**Checkpoint:** You should see "© 2024 Ace Coder" at the bottom!

### Step 3: Refactor with Destructuring (Cleaner Code!)

Change the Footer component to this:

```jsx
function Footer({ year, name }) {
  return (
    <footer style={{color: 'gray', marginTop: '20px'}}>
      <p>&copy; {year} {name}</p>
    </footer>
  );
}
```

**Key Points:**
- `{ year, name }` **destructures** the props object
- Now we can use `year` and `name` directly (no `props.` needed)
- The page should look exactly the same - just cleaner code!
- The curly braces in `{ year, name }` are JavaScript destructuring (different from JSX curly braces)

---

## Part 4: Image Component

### Step 1: Create the Image Component

```jsx
function Image({ src, altText, height }) {
  return (
    <img 
      src={src}
      alt={altText}
      height={height}
      style={{display: 'block', margin: '10px 0'}}
    />
  );
}
```

**Key Points:**
- Using destructuring from the start: `{ src, altText, height }`
- `alt` text is important for accessibility
- Components let us add consistent styling to all images

### Step 2: Add Image to App

First, add the image URL inside the App function (before the return):

```jsx
function App() {
  const imageURL = "https://images.unsplash.com/photo-1561731216-c3a4d99437d5?w=400";
  
  return (
    <div style={{padding: '20px', fontFamily: 'sans-serif'}}>
      <Header text="Famous Tigers" />
      <Image 
        src={imageURL}
        altText="A majestic tiger"
        height={200}
      />
      <Smiley />
      <Footer year={2024} name="Ace Coder" />
    </div>
  );
}

export default App;
```

**Checkpoint:** You should see a tiger image! 🐅

---

## Part 5: Rendering Lists with `.map()`

### Step 1: Create the Data

Add this **above** your App component (at the top of the file, after the other components):

```jsx
const tigerNames = [
  "Aslan",
  "Battle Cat",
  "Cringer",
  "Mufasa",
  "Nala",
  "Snagglepuss"
];
```

### Step 2: Create the TigerList Component

```jsx
function TigerList({ tigers }) {
  return (
    <div>
      <h2>Famous Cartoon Tigers</h2>
      <ul>
        {tigers.map((name, index) => (
          <li key={index}>
            {name}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**Key Points:**
- `{tigers.map(...)}` - curly braces mean "run JavaScript in JSX"
- `.map()` transforms each array item into JSX
- `(name, index) => (...)` is an arrow function that runs for each tiger
- **`key={index}` is required** - React needs it to track list items
- Parentheses `(...)` after `=>` allow multi-line JSX

**How `.map()` works:**
```
["Aslan", "Mufasa"] 
    ↓ .map()
[<li key={0}>Aslan</li>, <li key={1}>Mufasa</li>]
```

### Step 3: Add TigerList to App

```jsx
function App() {
  const imageURL = "https://images.unsplash.com/photo-1561731216-c3a4d99437d5?w=400";
  
  return (
    <div style={{padding: '20px', fontFamily: 'sans-serif'}}>
      <Header text="Famous Tigers" />
      <Image 
        src={imageURL}
        altText="A majestic tiger"
        height={200}
      />
      <TigerList tigers={tigerNames} />
      <Smiley />
      <Footer year={2024} name="Ace Coder" />
    </div>
  );
}
```

**Checkpoint:** You should see a list of tiger names!

### Step 4: Better Practice - Using Object IDs

Instead of just an array of strings, let's create objects with unique IDs.

Add this **below** the `tigerNames` array:

```jsx
const tigerObjects = tigerNames.map((name, index) => ({
  id: index + 1,
  title: name
}));
```

Update the TigerList component:

```jsx
function TigerList({ tigers }) {
  return (
    <div>
      <h2>Famous Cartoon Tigers</h2>
      <ul>
        {tigers.map((tiger) => (
          <li key={tiger.id}>
            {tiger.title}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

Update App to use `tigerObjects` instead of `tigerNames`:

```jsx
<TigerList tigers={tigerObjects} />
```

**Key Points:**
- Each tiger is now an object with `id` and `title`
- We use `tiger.id` as the key (better than using index)
- This is the pattern you'll use with real data from APIs

**Final Checkpoint:** Everything should still work, but now with better data structure!

---

## Complete Final Code

Here's everything together:

```jsx
// Data
const tigerNames = [
  "Aslan",
  "Battle Cat",
  "Cringer",
  "Mufasa",
  "Nala",
  "Snagglepuss"
];

const tigerObjects = tigerNames.map((name, index) => ({
  id: index + 1,
  title: name
}));

// Components
function Header({ text }) {
  return (
    <header>
      <h1>{text}</h1>
    </header>
  );
}

function Image({ src, altText, height }) {
  return (
    <img 
      src={src}
      alt={altText}
      height={height}
      style={{display: 'block', margin: '10px 0'}}
    />
  );
}

function TigerList({ tigers }) {
  return (
    <div>
      <h2>Famous Cartoon Tigers</h2>
      <ul>
        {tigers.map((tiger) => (
          <li key={tiger.id}>
            {tiger.title}
          </li>
        ))}
      </ul>
    </div>
  );
}

function Smiley() {
  return (
    <div style={{fontSize: '48px'}}>
      😊
    </div>
  );
}

function Footer({ year, name }) {
  return (
    <footer style={{color: 'gray', marginTop: '20px'}}>
      <p>&copy; {year} {name}</p>
    </footer>
  );
}

// Main App
function App() {
  const imageURL = "https://images.unsplash.com/photo-1561731216-c3a4d99437d5?w=400";
  
  return (
    <div style={{padding: '20px', fontFamily: 'sans-serif'}}>
      <Header text="Famous Tigers" />
      <Image 
        src={imageURL}
        altText="A majestic tiger"
        height={200}
      />
      <TigerList tigers={tigerObjects} />
      <Smiley />
      <Footer year={2024} name="Ace Coder" />
    </div>
  );
}

export default App;
```

---

## Common Mistakes to Avoid

### ❌ Lowercase component names
```jsx
function header() { ... }  // Wrong!
function Header() { ... }  // Correct!
```

### ❌ Missing `key` in lists
```jsx
{tigers.map(tiger => <li>{tiger.title}</li>)}  // Wrong!
{tigers.map(tiger => <li key={tiger.id}>{tiger.title}</li>)}  // Correct!
```

### ❌ Forgetting curly braces for JavaScript
```jsx
<p>props.text</p>  // Wrong - displays literally "props.text"
<p>{props.text}</p>  // Correct - displays the value
```

### ❌ Missing return in `.map()`
```jsx
// Wrong - missing parens or return
{tigers.map(tiger => {
  <li key={tiger.id}>{tiger.title}</li>
})}

// Correct - implicit return with parens
{tigers.map(tiger => (
  <li key={tiger.id}>{tiger.title}</li>
))}

// Also correct - explicit return
{tigers.map(tiger => {
  return <li key={tiger.id}>{tiger.title}</li>;
})}
```

### ❌ Destructuring but still using props
```jsx
// Wrong!
function Header({ text }) {
  return <h1>{props.text}</h1>;  // props doesn't exist!
}

// Correct!
function Header({ text }) {
  return <h1>{text}</h1>;
}
```

---

## Key Concepts Summary

1. **Components are functions** that return JSX
2. **Component names must be capitalized** (React convention)
3. **Props pass data** to components like function parameters
4. **Destructuring props** makes code cleaner: `{ year, name }`
5. **`.map()` renders lists** - transforms arrays into JSX
6. **`key` attribute is required** for list items (use unique IDs)
7. **Double curly braces** `{{}}` for inline styles (JavaScript object in JSX)
8. **camelCase** for CSS property names in React

---

## Try It Yourself!

**Easy Challenges:**
1. Add another tiger to the `tigerNames` array
2. Change the smiley emoji to a different one (🎉, 🐯, 🚀)
3. Change the footer year to use `{new Date().getFullYear()}` for automatic year
4. Add a different image URL and see it render

**Medium Challenges:**
1. Create a `MovieList` component that displays your favorite movies
2. Add a `width` prop to the Image component
3. Change the tiger list to display items in different colors
4. Create a `Greeting` component that takes a `name` prop

**Advanced Challenges:**
1. Add descriptions to each tiger object and display them
2. Create a separate `Tiger` component for each list item
3. Make the list items have hover effects with inline styles
4. Create an array of image URLs and display multiple images

---

## Next Class

We'll learn about **state** with `useState()` and build an interactive Todo app where you can add and remove items. This is where React gets really powerful!

---

## Resources

- React Playground: https://react.new
- React Official Docs: https://react.dev/learn
- Thinking in React: https://react.dev/learn/thinking-in-react

---

## Need Help?

- **Console errors?** Check for:
  - Capitalized component names
  - Missing `key` in lists
  - Closing tags and parentheses
  - `export default App` at the bottom
  
- **Nothing showing?** Check:
  - Do you have `export default App`?
  - Are all components defined before App?
  - Check the browser console for error messages

- **Still stuck?** Ask your instructor or a classmate!
