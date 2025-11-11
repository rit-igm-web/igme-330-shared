# React Todo App - Complete Guide

## What We're Building
A fully functional todo list application with:
- Add new todos
- Delete todos  
- Persistent storage (survives page refresh!)
- Real-time todo count

**Duration**: 50-60 minutes  
**Technologies**: React, Vite, localStorage  
**Prerequisites**: Basic understanding of components and props

---

## Setup: Create Vite Project (5 minutes)

### Step 1: Create Project Folder

Open your terminal and run:

```bash
# Create folder and navigate into it
mkdir todo-app
cd todo-app

# Create Vite project in current directory
npm create vite@latest
```

**When prompted:**
- Project name: type a period `.` (creates project in current folder)
- Framework: choose **React**
- Variant: choose **JavaScript**
- Use rolldown-vite (Experimental)?: choose **No** (or just press Enter)
- Install with npm and start now?: choose **Yes**

**If you chose Yes above**, the dev server will start automatically. Open the localhost URL in your browser to see the default Vite React app.

**If you chose No**, run these commands:

```bash
npm install
npm run dev
```

### Step 2: Clean Up Unnecessary Files (2 minutes)

Vite gives us demo code we don't need. Let's clean it up.

**Important:** If the dev server is running, press `Ctrl+C` to stop it before deleting files. You can restart it with `npm run dev` after cleaning up.

**Delete these files:**
- `src/assets/` (entire folder)
- `public/vite.svg`
- `src/index.css` (delete this file)

**Keep these files:**
- `src/App.jsx`
- `src/App.css`
- `src/main.jsx`
- `index.html`

**Modify `index.html`** - change the title:
```html
<title>Todo App</title>
```

**Modify `src/main.jsx`** - remove the index.css import. Your file should look like this:

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

**Modify `src/App.css`** - delete all the content (empty the file completely). We'll add our own styles.

---

## Part 1: Basic App Structure and First Todo (10 minutes)

### Step 1: Create Basic App Shell (3 minutes)

**In `src/App.jsx`, replace everything with this:**

```jsx
import './App.css'

function App() {
  return (
    <div className="app">
      <header>
        <h1>My Todo List</h1>
      </header>
      <main>
        <p>Todos will go here</p>
      </main>
      <footer>
        <p>&copy; 2024 Ace Coder</p>
      </footer>
    </div>
  );
}

export default App;
```

**Understanding this code:**
- `import './App.css'` - Brings in our CSS file
- `function App()` - This is our main component
- `return (...)` - Returns JSX that describes the UI
- `<div className="app">` - Note: it's `className` not `class` in React/JSX
- `export default App` - Makes this component available to other files

**Checkpoint:** Save and check your browser. You should see the heading, placeholder text, and footer.

### Step 2: Add Basic Styling (2 minutes)

**In `src/App.css`, add this:**

```css
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: sans-serif;
}

header {
  text-align: center;
  margin-bottom: 30px;
}

footer {
  text-align: center;
  margin-top: 30px;
  color: gray;
}
```

**Understanding this CSS:**
- `.app` - Styles our main container
- `max-width: 600px` - Keeps content from getting too wide
- `margin: 0 auto` - Centers the container horizontally
- The header and footer styling adds spacing and visual hierarchy

**Checkpoint:** The app should now be centered with better spacing and typography.

### Step 3: Add a Hard-Coded Todo Item (5 minutes)

**In `src/App.jsx`, replace the `<main>` section with this:**

```jsx
<main>
  <ul className="todo-list">
    <li className="todo-item">
      <span>Learn React</span>
      <button className="delete-btn">Delete</button>
    </li>
  </ul>
</main>
```

**Understanding this structure:**
- `<ul>` - Unordered list to hold our todos
- `<li>` - Each todo will be a list item
- `<span>` - Wraps the todo text
- `<button>` - Delete button (not functional yet)
- We're using CSS class names for styling

**In `src/App.css`, add these styles:**

```css
.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  margin-bottom: 8px;
  background: #f0f0f0;
  border-radius: 4px;
}

.delete-btn {
  background: #ff4444;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.delete-btn:hover {
  background: #cc0000;
}
```

**Understanding this CSS:**
- `list-style: none` - Removes bullet points
- `display: flex` - Makes the todo text and button sit side-by-side
- `justify-content: space-between` - Pushes text left and button right
- `align-items: center` - Vertically centers content
- `:hover` - Changes color when you hover over the delete button

**Checkpoint:** You should see a styled todo item with a delete button (button doesn't work yet).

---

## Part 2: Adding State with `useState` (10 minutes)

Right now our todo is hard-coded HTML. To make it dynamic and interactive, we need **state**.

### What is State?

**State** is data that can change over time. When state changes, React automatically re-renders the component to show the new data. Think of it like variables that React is "watching" - when they change, React updates what you see on the screen.

### Step 1: Import useState and Create State (3 minutes)

**At the top of `src/App.jsx`, modify the imports:**

```jsx
import { useState } from 'react'
import './App.css'
```

**Inside the `App` function, add this BEFORE the `return` statement:**

```jsx
function App() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React' },
    { id: 2, text: 'Build a Todo App' },
    { id: 3, text: 'Master useState' }
  ]);

  return (
    // ... rest of code
```

**Understanding `useState`:**

```jsx
const [todos, setTodos] = useState([...initial data...]);
```

Let's break this down piece by piece:

- **`useState`** - A React Hook that adds state to our component
- **The initial value** - The array `[{ id: 1, text: 'Learn React' }, ...]` is what `todos` starts as
- **`todos`** - This is our state variable (the current value)
  - It's an array of objects
  - Each object has an `id` (unique identifier) and `text` (the todo content)
- **`setTodos`** - This is a function React gives us to update the state
  - We MUST use this function to change `todos`
  - We can't just do `todos = newValue` (won't work!)
  - When we call `setTodos(newValue)`, React will re-render the component
- **Array destructuring** - The `[todos, setTodos]` syntax unpacks the array useState returns
  - useState returns an array with 2 items: `[currentValue, setterFunction]`
  - We're pulling those out into separate variables

**Why objects with IDs?**
- We need unique IDs to track which specific todo to delete
- Just like we needed `key` when using `.map()` in the Tigers app
- The `id` must be unique for each todo

**Could you define the data elsewhere?**
Yes! You could do this:

```jsx
const initialTodos = [
  { id: 1, text: 'Learn React' },
  { id: 2, text: 'Build a Todo App' },
  { id: 3, text: 'Master useState' }
];

function App() {
  const [todos, setTodos] = useState(initialTodos);
  // ...
}
```

This is totally valid! The advantage is you can reuse `initialTodos` elsewhere if needed, or keep your component code cleaner. It's a matter of preference and organization.

### Step 2: Render Todos from State (5 minutes)

Now let's make React display our todos from state instead of hard-coded HTML.

**In `src/App.jsx`, replace the `<main>` section with:**

```jsx
<main>
  <ul className="todo-list">
    {todos.map(todo => (
      <li key={todo.id} className="todo-item">
        <span>{todo.text}</span>
        <button className="delete-btn">Delete</button>
      </li>
    ))}
  </ul>
</main>
```

**Understanding this code:**

```jsx
{todos.map(todo => (
  <li key={todo.id} className="todo-item">
    <span>{todo.text}</span>
    <button className="delete-btn">Delete</button>
  </li>
))}
```

Let's break down what's happening:

- **Curly braces `{}`** - These tell React "I'm writing JavaScript, not HTML"
- **`todos.map()`** - The `.map()` array method transforms each todo into JSX
  - It loops through each item in the `todos` array
  - For each todo, it creates an `<li>` element
- **`todo =>`** - Arrow function that runs for each todo
  - `todo` is the current item in the array
  - The `=>` means "transform this into..."
- **Parentheses `(...)`** - Allow us to write multi-line JSX
- **`key={todo.id}`** - Required by React for list items
  - React uses this to track which items changed
  - Must be unique for each item
  - Helps React optimize re-rendering
- **`{todo.text}`** - Displays the text property of each todo object
  - Remember: curly braces insert JavaScript values into JSX

**Why `.map()` and not a `for` loop?**
- JSX needs an expression that returns a value
- `.map()` returns a new array (the JSX elements)
- `for` loops are statements, not expressions - they don't return anything

**Checkpoint:** You should now see 3 todos rendered from the state array!

### Step 3: Add Delete Functionality (2 minutes)

Now let's make the delete button actually work.

**In `src/App.jsx`, add this function inside `App`, before the `return` statement:**

```jsx
const deleteTodo = (id) => {
  setTodos(todos.filter(todo => todo.id !== id));
};
```

**Update the delete button to call this function:**

```jsx
<button 
  className="delete-btn"
  onClick={() => deleteTodo(todo.id)}
>
  Delete
</button>
```

**Understanding the delete function:**

```jsx
const deleteTodo = (id) => {
  setTodos(todos.filter(todo => todo.id !== id));
};
```

Breaking this down:

- **`const deleteTodo = (id) => {...}`** - Arrow function that takes an `id` parameter
- **`todos.filter(...)`** - Creates a NEW array
  - `.filter()` keeps only items that pass a test
  - The test is: `todo => todo.id !== id`
  - Translation: "Keep all todos whose id does NOT match the id we want to delete"
- **`setTodos(...)`** - Updates state with the filtered array
  - This causes React to re-render
  - The deleted todo disappears from the screen

**Example:** If we have todos with ids `[1, 2, 3]` and delete id `2`:
```
Original: [1, 2, 3]
Filter: keep todos where id !== 2
Result: [1, 3]
```

**Understanding the button's onClick:**

```jsx
onClick={() => deleteTodo(todo.id)}
```

- **`onClick`** - React event handler (runs when button is clicked)
- **`() => deleteTodo(todo.id)`** - Arrow function wrapper
  - We NEED this wrapper!
  - Without it: `onClick={deleteTodo(todo.id)}` would call the function immediately
  - With it: React saves this function and calls it later when clicked
- **`todo.id`** - Passes the specific todo's id to the delete function

**Why the arrow function wrapper?**
```jsx
// ❌ Wrong - Calls deleteTodo immediately during render
onClick={deleteTodo(todo.id)}

// ✅ Correct - Creates a function that React calls later
onClick={() => deleteTodo(todo.id)}
```

**Checkpoint:** Click the delete buttons - todos should disappear! But if you refresh the page, they come back. We'll fix this with localStorage later.

---

## Part 3: Adding New Todos (12 minutes)

Now let's add the ability to create new todos.

### Step 1: Create Input State (3 minutes)

We need another piece of state to track what the user is typing in the input field.

**In `src/App.jsx`, add another `useState` call below the first one:**

```jsx
const [todos, setTodos] = useState([
  { id: 1, text: 'Learn React' },
  { id: 2, text: 'Build a Todo App' },
  { id: 3, text: 'Master useState' }
]);

const [newTodo, setNewTodo] = useState('');
```

**Understanding this state:**

- **`newTodo`** - Stores the current text in the input field
- **`setNewTodo`** - Function to update what's in the input
- **Initial value is `''`** - Empty string (input starts empty)
- This is a **controlled component** pattern (more on this below)

### Step 2: Add Form UI (4 minutes)

**In `src/App.jsx`, add this form ABOVE the `<ul>` inside `<main>`:**

```jsx
<main>
  <form onSubmit={handleSubmit} className="add-form">
    <input 
      type="text"
      value={newTodo}
      onChange={(e) => setNewTodo(e.target.value)}
      placeholder="Add a new todo..."
      className="todo-input"
    />
    <button type="submit" className="add-btn">Add</button>
  </form>

  <ul className="todo-list">
    {/* existing todo list code */}
  </ul>
</main>
```

**Understanding this form:**

```jsx
<form onSubmit={handleSubmit} className="add-form">
```
- **`onSubmit`** - Event handler for form submission
- Fires when user presses Enter or clicks the submit button
- Note: it's on the `<form>`, not the button!

```jsx
<input 
  type="text"
  value={newTodo}
  onChange={(e) => setNewTodo(e.target.value)}
  placeholder="Add a new todo..."
  className="todo-input"
/>
```

This is a **controlled component**. Let's understand what that means:

- **`value={newTodo}`** - Binds the input's value to our state
  - The input always shows what's in `newTodo` state
  - React controls what's displayed
- **`onChange={(e) => setNewTodo(e.target.value)}`** - Updates state on every keystroke
  - `e` is the event object
  - `e.target` is the input element itself
  - `e.target.value` is the current text in the input
  - `setNewTodo(...)` updates the state
- **The data flow:**
  1. User types a character
  2. `onChange` fires
  3. `setNewTodo` updates state
  4. React re-renders
  5. Input displays the new value

**Why this pattern (controlled component)?**
- React is the "single source of truth"
- We can easily validate, format, or manipulate the input
- We can clear the input programmatically (we'll do this after adding)
- Makes it easy to access the value when submitting

**Add the form styling to `src/App.css`:**

```css
.add-form {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  border: 2px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.add-btn {
  background: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.add-btn:hover {
  background: #45a049;
}
```

**Understanding the form CSS:**
- `display: flex` - Makes input and button sit side-by-side
- `gap: 8px` - Adds space between input and button
- `flex: 1` on input - Makes input take up all available space
- Button stays its natural width

**Note:** You'll see an error about `handleSubmit` not being defined. That's next!

### Step 3: Implement handleSubmit (5 minutes)

**In `src/App.jsx`, add this function before the `return` statement:**

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  
  if (newTodo.trim() === '') {
    return;
  }
  
  const todo = {
    id: Date.now(),
    text: newTodo
  };
  
  setTodos([...todos, todo]);
  setNewTodo('');
};
```

**Understanding this function piece by piece:**

**Line 1: `e.preventDefault()`**
```jsx
e.preventDefault();
```
- `e` is the event object (automatically passed to event handlers)
- `preventDefault()` stops the form's default behavior
- By default, forms submit and refresh the page
- We don't want that! We're handling everything with JavaScript

**Lines 3-5: Validation**
```jsx
if (newTodo.trim() === '') {
  return;
}
```
- `.trim()` removes whitespace from beginning and end
- Prevents adding empty or whitespace-only todos
- `return` exits the function early if input is empty

**Lines 7-10: Create new todo object**
```jsx
const todo = {
  id: Date.now(),
  text: newTodo
};
```
- Creates a new todo object with the same structure as our state
- `id: Date.now()` - Uses current time in milliseconds as a unique ID
  - `Date.now()` returns something like `1699564800000`
  - This is unique enough for our purposes (two clicks can't happen in the same millisecond)
- `text: newTodo` - The text the user typed

**Line 12: Update state**
```jsx
setTodos([...todos, todo]);
```

This is **crucial** - let's really understand it:

- **`[...todos, todo]`** - Creates a BRAND NEW array
- **`...todos`** - The spread operator "unpacks" the existing todos
  - If `todos` is `[{id:1, text:'A'}, {id:2, text:'B'}]`
  - Then `...todos` becomes `{id:1, text:'A'}, {id:2, text:'B'}`
- **`, todo`** - Adds our new todo to the end
- **Result**: New array = `[{old todo 1}, {old todo 2}, {new todo}]`

**Why not just `todos.push(todo)`?**

```jsx
// ❌ WRONG - Mutating state directly
todos.push(todo);
setTodos(todos);

// ✅ CORRECT - Creating new array
setTodos([...todos, todo]);
```

React needs to detect that state changed to trigger a re-render. If you modify the array directly (mutate it), React sees it's the same array object and might not re-render. Creating a new array guarantees React sees the change.

**Line 13: Clear input**
```jsx
setNewTodo('');
```
- Resets the input field to empty
- Clears `newTodo` state back to empty string
- Ready for the next todo!

**Checkpoint:** Try adding new todos! Type something and press Enter or click Add. The todo should appear in the list and the input should clear.

**Try these tests:**
- Add a todo by pressing Enter
- Add a todo by clicking the button
- Try to add an empty todo (nothing happens - validation works!)
- Try to add just spaces (nothing happens - trim works!)

---

## Part 4: Persisting with localStorage and `useEffect` (15 minutes)

### The Problem

Try this: Add a new todo, then refresh your browser page.

**What happened?** The new todo disappeared! Only the original 3 are back.

**Why?** State only lives in memory. When you refresh, React starts from scratch with the initial state values.

**Solution:** We need to save todos to **localStorage** (browser storage that persists) and load them when the app starts.

### What is useEffect?

**`useEffect`** is a React Hook that lets us perform "side effects" in our component.

**What's a side effect?**
- Any operation that reaches outside the component
- Examples: localStorage, API calls, timers, subscriptions
- Things that happen "in addition to" rendering

**Why not just save in handleSubmit?**
- That only catches additions
- We also need to save when deleting
- `useEffect` can watch our state and save whenever it changes

### Step 1: Create storage.js Helper (4 minutes)

Let's create a separate file to handle localStorage operations.

**Create a new file: `src/storage.js`** and add this code:

```js
const STORAGE_KEY = 'react-todo-app';

export const saveTodos = (todos) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  console.log('Saved todos:', todos);
};

export const loadTodos = () => {
  const stored = localStorage.getItem(STORAGE_KEY);
  if (stored) {
    console.log('Loaded todos from storage');
    return JSON.parse(stored);
  }
  console.log('No saved todos found');
  return [];
};
```

**Understanding this code:**

**The storage key:**
```js
const STORAGE_KEY = 'react-todo-app';
```
- A unique identifier for our app's data in localStorage
- Prevents conflicts with other apps
- Like a file name for our saved data

**The saveTodos function:**
```js
export const saveTodos = (todos) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  console.log('Saved todos:', todos);
};
```
- **`export const`** - Makes this function available to import elsewhere
- **`localStorage.setItem(key, value)`** - Browser API to save data
- **`JSON.stringify(todos)`** - Converts array to JSON string
  - localStorage can only store strings
  - `[{id:1, text:'A'}]` becomes `'[{"id":1,"text":"A"}]'`
- **`console.log`** - Helps us see when saving happens (useful for debugging)

**The loadTodos function:**
```js
export const loadTodos = () => {
  const stored = localStorage.getItem(STORAGE_KEY);
  if (stored) {
    console.log('Loaded todos from storage');
    return JSON.parse(stored);
  }
  console.log('No saved todos found');
  return [];
};
```
- **`localStorage.getItem(key)`** - Retrieves saved data
- **`if (stored)`** - Checks if anything was saved
- **`JSON.parse(stored)`** - Converts JSON string back to array
  - `'[{"id":1,"text":"A"}]'` becomes `[{id:1, text:'A'}]`
- **`return []`** - If nothing saved, return empty array

### Step 2: Import useEffect and Storage Functions (1 minute)

**At the top of `src/App.jsx`, modify imports:**

```jsx
import { useState, useEffect } from 'react'
import { saveTodos, loadTodos } from './storage'
import './App.css'
```

**Understanding the imports:**
- **`useEffect`** - Another React Hook (in addition to useState)
- **`{ saveTodos, loadTodos }`** - Named imports from our storage file
- **`from './storage'`** - Relative path (`./ `means "in the current folder")
  - Note: We don't need the `.js` extension when importing

### Step 3: Load Todos on App Start (4 minutes)

We need to load saved todos when the app first starts, instead of always using hard-coded defaults.

**In `src/App.jsx`, modify the initial todos state:**

```jsx
const [todos, setTodos] = useState(() => {
  const saved = loadTodos();
  if (saved.length > 0) {
    return saved;
  }
  return [
    { id: 1, text: 'Learn React' },
    { id: 2, text: 'Build a Todo App' },
    { id: 3, text: 'Master useState' }
  ];
});
```

**Understanding this code:**

This is called **lazy initialization**. Let's break it down:

**Before (simple):**
```jsx
const [todos, setTodos] = useState([{ id: 1, ... }]);
```
- The initial value is provided directly
- Simple and straightforward

**After (lazy/function):**
```jsx
const [todos, setTodos] = useState(() => {
  // code that returns initial value
});
```
- Instead of a value, we pass a **function**
- The function runs once when the component first mounts
- The function's return value becomes the initial state

**Why use a function?**
- **Performance**: `loadTodos()` only runs once, not on every re-render
- **Conditional logic**: We can decide what initial value to use based on conditions
- Without the function, `loadTodos()` would run on every render (wasteful!)

**The logic flow:**
```jsx
const saved = loadTodos();  // Try to load from localStorage
if (saved.length > 0) {     // If we found saved todos
  return saved;              // Use them as initial state
}
return [                     // Otherwise, use defaults
  { id: 1, text: 'Learn React' },
  // ...
];
```

**Checkpoint:** If you had saved todos before, they should appear now when you refresh!

### Step 4: Save Automatically with useEffect (4 minutes)

Now we need to save todos to localStorage whenever they change.

**In `src/App.jsx`, add this right after your `useState` declarations and BEFORE the `deleteTodo` and `handleSubmit` functions:**

Your code should now look like this:

```jsx
function App() {
  const [todos, setTodos] = useState(() => {
    const saved = loadTodos();
    if (saved.length > 0) {
      return saved;
    }
    return [
      { id: 1, text: 'Learn React' },
      { id: 2, text: 'Build a Todo App' },
      { id: 3, text: 'Master useState' }
    ];
  });

  const [newTodo, setNewTodo] = useState('');

  // Add this useEffect here
  useEffect(() => {
    saveTodos(todos);
  }, [todos]);

  const handleSubmit = (e) => {
    // ... existing code
  };

  const deleteTodo = (id) => {
    // ... existing code
  };

  return (
    // ... existing JSX
  );
}
```

**Understanding useEffect in detail:**

```jsx
useEffect(() => {
  // Effect code here
}, [dependencies]);
```

**useEffect has two parts:**

**1. The effect function (first parameter):**
```jsx
() => {
  saveTodos(todos);
}
```
- This is the code that runs
- In our case: save todos to localStorage
- Think of it as "what side effect do you want to happen?"

**2. The dependency array (second parameter):**
```jsx
[todos]
```
- Controls when the effect runs
- Our effect only runs when `todos` changes
- Think of it as "when should this side effect happen?"

**How does it work?**

React watches the values in the dependency array:

1. **First render**: Effect runs (saves initial todos)
2. **User adds todo**: `todos` changes → effect runs → saves
3. **User deletes todo**: `todos` changes → effect runs → saves
4. **User types in input**: `newTodo` changes, but `todos` doesn't → effect doesn't run
5. **Component re-renders for other reasons**: If `todos` hasn't changed → effect doesn't run

**Different dependency array scenarios:**

```jsx
// Runs on EVERY render (usually not what you want)
useEffect(() => {
  saveTodos(todos);
});

// Runs ONLY ONCE when component first mounts
useEffect(() => {
  console.log('App started!');
}, []);

// Runs when EITHER todos OR newTodo changes
useEffect(() => {
  // do something
}, [todos, newTodo]);

// Runs whenever todos changes (what we want!)
useEffect(() => {
  saveTodos(todos);
}, [todos]);
```

**Why do we need the dependency array?**

Without it, the effect would run on every single render, which could cause performance issues and infinite loops in some cases. The dependency array gives us precise control.

**Checkpoint - Full test:**

1. **Add a todo** - Check the console, you should see "Saved todos: [...]"
2. **Delete a todo** - Check the console, you should see another save
3. **Refresh the page** - Your todos should persist!
4. **Close the browser tab and reopen** - Todos still there!

**Open DevTools to see localStorage:**
- Chrome/Edge: F12 → Application tab → Storage → Local Storage
- Firefox: F12 → Storage tab → Local Storage
- Look for the key `react-todo-app`
- You'll see your todos stored as a JSON string

**Success!** Your todos now persist across browser sessions!

---

## Part 5: Polish and Improvements (5 minutes)

Let's add some nice finishing touches to make the app more professional.

### Add Todo Count

Let's show users how many todos they have.

**In `src/App.jsx`, update the `<header>`:**

```jsx
<header>
  <h1>My Todo List</h1>
  <p>{todos.length} {todos.length === 1 ? 'todo' : 'todos'}</p>
</header>
```

**Understanding this code:**

```jsx
{todos.length}
```
- `todos.length` gives us the number of items in the array
- Wrapped in `{}` to insert the JavaScript value into JSX

```jsx
{todos.length === 1 ? 'todo' : 'todos'}
```
- This is a **ternary operator** (conditional expression)
- Format: `condition ? valueIfTrue : valueIfFalse`
- If there's exactly 1 todo: show "todo" (singular)
- Otherwise: show "todos" (plural)
- Examples:
  - 0 todos → "0 todos"
  - 1 todo → "1 todo"
  - 5 todos → "5 todos"

### Add Empty State Message

When there are no todos, let's show a helpful message instead of an empty list.

**In `src/App.jsx`, add this in the `<main>` section, BEFORE the `<ul>`:**

```jsx
{todos.length === 0 && (
  <p className="empty-message">No todos yet. Add one above!</p>
)}
```

**Understanding this code:**

This uses the **logical AND operator (`&&`)** for conditional rendering:

```jsx
{condition && <JSX to render>}
```

**How it works:**
- If `condition` is true → React renders the JSX
- If `condition` is false → React renders nothing

**In our case:**
- If `todos.length === 0` (no todos) → Show the message
- If we have todos → Show nothing (the list will show instead)

**Why use `&&` instead of ternary?**

```jsx
// With && (when you only want to show something conditionally)
{todos.length === 0 && <p>No todos</p>}

// With ternary (when you want to show different things)
{todos.length === 0 ? <p>No todos</p> : <TodoList />}

// With && you don't need the "else" case
```

**Add styling for the message to `src/App.css`:**

```css
.empty-message {
  text-align: center;
  color: gray;
  padding: 40px;
  font-style: italic;
}
```

**Checkpoint:** Delete all your todos. You should see "No todos yet. Add one above!" in gray, centered, and italicized.

---

## Key Concepts Summary

### useState
```jsx
const [value, setValue] = useState(initialValue);
```
- Adds state to function components
- `value` is the current state
- `setValue` is the function to update state
- Calling `setValue` causes React to re-render
- Always create NEW arrays/objects (don't mutate state)

### useEffect
```jsx
useEffect(() => {
  // Side effect code
}, [dependencies]);
```
- Runs side effects after render
- First parameter: function that performs the effect
- Second parameter: array of dependencies
- Effect only runs when dependencies change
- Use for: localStorage, API calls, subscriptions, timers

### Controlled Components
```jsx
<input 
  value={state}
  onChange={(e) => setState(e.target.value)}
/>
```
- React controls the input value
- Input value is bound to state
- State updates on every keystroke
- Single source of truth

### Event Handling
- `onClick`, `onChange`, `onSubmit`
- Pass functions, not function calls
- Use arrow functions to pass parameters: `onClick={() => func(param)}`
- `e.preventDefault()` to stop default behaviors

### Immutable State Updates
```jsx
// Adding to array
setArray([...array, newItem]);

// Removing from array
setArray(array.filter(item => item.id !== idToRemove));

// Updating an item
setArray(array.map(item => 
  item.id === idToUpdate ? { ...item, text: newText } : item
));
```

---

## Common Mistakes and How to Fix Them

### Mistake 1: Mutating State Directly

```jsx
// ❌ WRONG - Mutates state
todos.push(newTodo);
setTodos(todos);

// ✅ CORRECT - Creates new array
setTodos([...todos, newTodo]);
```

**Why it's wrong:** React won't detect the change because the array reference is the same.

### Mistake 2: Missing Dependency Array

```jsx
// ❌ WRONG - Runs on every render
useEffect(() => {
  saveTodos(todos);
});

// ✅ CORRECT - Only runs when todos changes
useEffect(() => {
  saveTodos(todos);
}, [todos]);
```

**Why it's wrong:** Without dependencies, effect runs constantly, causing performance issues.

### Mistake 3: Calling Function Immediately

```jsx
// ❌ WRONG - Calls deleteTodo immediately during render
<button onClick={deleteTodo(todo.id)}>Delete</button>

// ✅ CORRECT - Creates function to call later
<button onClick={() => deleteTodo(todo.id)}>Delete</button>
```

**Why it's wrong:** Without the arrow function, `deleteTodo(todo.id)` executes immediately, not when clicked.

### Mistake 4: Forgetting preventDefault

```jsx
// ❌ WRONG - Page refreshes on submit
const handleSubmit = (e) => {
  const todo = { ... };
  setTodos([...todos, todo]);
};

// ✅ CORRECT - Prevents page refresh
const handleSubmit = (e) => {
  e.preventDefault();
  const todo = { ... };
  setTodos([...todos, todo]);
};
```

**Why it's wrong:** Forms refresh the page by default, losing all state.

### Mistake 5: Using Index as Key

```jsx
// ❌ AVOID - Using array index as key
{todos.map((todo, index) => (
  <li key={index}>...</li>
))}

// ✅ BETTER - Using unique ID
{todos.map(todo => (
  <li key={todo.id}>...</li>
))}
```

**Why it's wrong:** If items reorder, React gets confused and may update the wrong items.

---

## Extension Challenges

### Easy
1. **Add a "Clear All" button** that deletes all todos at once
2. **Show creation timestamp** for each todo
3. **Make footer year dynamic** using `new Date().getFullYear()`

### Medium
1. **Add edit functionality** - Double-click a todo to edit it
2. **Add checkboxes** to mark todos as complete
3. **Add filtering** - Show all/active/completed todos
4. **Add todo categories** - Allow users to tag todos

### Advanced
1. **Drag and drop reordering** - Let users reorder todos
2. **Due dates** - Add date picker and sort by due date
3. **Undo/redo** - Keep history of changes
4. **Search/filter** - Search through todos by text

---

## Debugging Tips

### Todos Don't Persist
- **Check console** for "Saved todos" messages
- **Open DevTools** → Application → Local Storage
- Look for the `react-todo-app` key
- Try **clearing localStorage** and starting fresh
- Verify `useEffect` dependency array includes `[todos]`

### Delete Not Working
- Check you're using arrow function: `() => deleteTodo(todo.id)`
- Verify IDs are unique (console.log them)
- Make sure you're not mutating state

### Infinite Loop / App Crashes
- Check your `useEffect` dependency array
- Make sure you're not updating state inside useEffect without proper dependencies
- Verify you're creating new arrays/objects, not mutating

### Input Won't Clear
- Verify `setNewTodo('')` is called after adding
- Check that input's `value` prop is set to `{newTodo}`
- Make sure `onChange` is updating state correctly

### Console Errors
- **"Cannot read property of undefined"** - Check that todos exist before mapping
- **"Each child should have unique key"** - Add `key={todo.id}` to list items
- **"Too many re-renders"** - Check event handlers aren't being called immediately

---

## What's Next?

In future lessons, you'll learn:

- **Component composition** - Breaking this into smaller, reusable components
- **useContext** - Sharing state across many components without prop drilling
- **Custom hooks** - Creating your own reusable hooks
- **React Router** - Building multi-page applications
- **API integration** - Fetching data from servers (like the Amiibo extra credit!)
- **Form validation** - More sophisticated input handling
- **TypeScript** - Adding type safety to React apps

---

## Resources

- **React Documentation**: https://react.dev
- **useState Hook**: https://react.dev/reference/react/useState
- **useEffect Hook**: https://react.dev/reference/react/useEffect
- **Thinking in React**: https://react.dev/learn/thinking-in-react
- **Vite Documentation**: https://vitejs.dev

---

**Great job!** You've built a complete, functional React application with state management and data persistence. This foundation will serve you well as you continue learning React.
