# Fetch API - Complete Guide

## What We're Building
A user directory application that fetches data from an API and displays it dynamically:
- Fetch user data from JSONPlaceholder API
- Display users as cards
- Show user details on click
- Handle loading states and errors properly

**Duration**: 75 minutes
**Technologies**: Fetch API, async/await, Promises
**API**: JSONPlaceholder (https://jsonplaceholder.typicode.com)

---

## A Quick Note About `.then()` vs `async/await` (3 minutes)

Before we dive in, you should know there are **two ways** to work with fetch:

### The `.then()` approach:
```js
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### The `async/await` approach (what we're using today):
```js
async function getData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```

**Why we're using async/await:**
- Easier to read (looks like "normal" code)
- Easier to debug
- Industry standard in 2025
- Less "callback nesting"

**Want to learn `.then()` syntax?**
- See [HW-ajax-5.md](https://github.com/rit-igm-web/igme-330-shared/blob/main/notes/HW-ajax-5.md) for a complete guide
- Watch the video walkthrough in myCourses
- Both approaches do the same thing, just different syntax!

For today's class, we're focusing on `async/await` so we have time to build something cool.

---

## Setup: Create Project Files (5 minutes)

### Step 1: Create Project Folder

Create a folder called `user-directory` with these files:

**File structure:**
```
user-directory/
  ├── index.html
  ├── styles.css
  └── app.js
```

### Step 2: Basic HTML Setup

**Create `index.html`:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>User Directory</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="container">
    <header>
      <h1>User Directory</h1>
      <button id="load-btn">Load Users</button>
    </header>

    <div id="status"></div>
    <div id="user-list"></div>
  </div>

  <script src="app.js"></script>
</body>
</html>
```

**Understanding this structure:**
- `#load-btn` - Button to trigger fetching data
- `#status` - Shows loading/error messages
- `#user-list` - Container where user cards will appear
- Script at bottom - Runs after HTML loads

### Step 3: Basic CSS

**Create `styles.css`:**

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  background: #f0f0f0;
  padding: 20px;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

header {
  background: white;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

#load-btn {
  background: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

#load-btn:hover {
  background: #45a049;
}

#status {
  text-align: center;
  padding: 20px;
  font-size: 18px;
  color: #666;
}

#user-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.user-card {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.user-card h3 {
  margin-bottom: 10px;
  color: #333;
}

.user-card p {
  color: #666;
  margin-bottom: 5px;
}

.error {
  color: #ff4444;
  background: #ffe0e0;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

.loading {
  color: #4CAF50;
}
```

**Understanding the CSS:**
- Grid layout for responsive cards
- `.user-card` styles for each user
- `.error` and `.loading` classes for status messages
- Clean, modern design that's easy to read

### Step 4: Empty JavaScript File

**Create `app.js`:**

```js
console.log('App loaded!');
```

**Checkpoint:** Open `index.html` in your browser. You should see:
- A header with title and green button
- Open DevTools console - should see "App loaded!"

---

## Part 1: Your First Fetch Request (12 minutes)

Let's fetch user data from the JSONPlaceholder API and log it to the console.

### Step 1: Understanding the API (2 minutes)

**JSONPlaceholder** provides fake REST API data for testing:
- Base URL: `https://jsonplaceholder.typicode.com`
- Users endpoint: `/users`
- Returns array of 10 user objects

**What we're fetching:**
```
GET https://jsonplaceholder.typicode.com/users
```

**What we get back:** Array of user objects like:
```json
[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "address": { ... },
    "phone": "1-770-736-8031",
    "website": "hildegard.org",
    "company": { ... }
  },
  ...
]
```

### Step 2: Write Your First Async Function (5 minutes)

**In `app.js`, replace everything with:**

```js
async function loadUsers() {
  console.log('Loading users...');

  const response = await fetch('https://jsonplaceholder.typicode.com/users');
  console.log('Response:', response);

  const users = await response.json();
  console.log('Users:', users);
}

// Hook up button
document.querySelector('#load-btn').onclick = loadUsers;
```

**Understanding this code:**

```js
async function loadUsers() {
```
- **`async`** keyword tells JavaScript this function will use `await`
- Makes the function automatically return a Promise
- Required whenever you want to use `await` inside

```js
const response = await fetch('https://jsonplaceholder.typicode.com/users');
```
- **`fetch(url)`** makes an HTTP request to the URL
- Returns a Promise that resolves to a Response object
- **`await`** tells JavaScript: "pause here until the data comes back"
- Without `await`, you'd get a pending Promise (not useful!)
- **`response`** is the Response object (has status, headers, methods)

```js
const users = await response.json();
```
- **`response.json()`** parses the response body as JSON
- Also returns a Promise (that's why we need another `await`!)
- **Why two awaits?** Because fetch works in two steps:
  1. Download the response (get the Response object)
  2. Parse the body (convert to JSON, text, etc.)
- **`users`** now contains the actual JavaScript array of user objects

```js
document.querySelector('#load-btn').onclick = loadUsers;
```
- Hooks up our button to call `loadUsers` when clicked
- Note: We pass `loadUsers`, not `loadUsers()` - we want to pass the function itself, not call it immediately

**Checkpoint:**
1. Click "Load Users" button
2. Open DevTools console
3. You should see:
   - "Loading users..."
   - Response object (status: 200, ok: true)
   - Array of 10 user objects

**Try this:** Expand the users array in the console and look at the data structure.

### Step 3: Display Users on the Page (5 minutes)

Now let's show the users on the page instead of just logging them.

**In `app.js`, update the `loadUsers` function:**

```js
async function loadUsers() {
  const statusDiv = document.querySelector('#status');
  const userList = document.querySelector('#user-list');

  statusDiv.textContent = 'Loading users...';
  statusDiv.className = 'loading';

  const response = await fetch('https://jsonplaceholder.typicode.com/users');
  const users = await response.json();

  statusDiv.textContent = '';
  displayUsers(users);
}

function displayUsers(users) {
  const userList = document.querySelector('#user-list');
  userList.innerHTML = ''; // Clear previous results

  users.forEach(user => {
    const card = document.createElement('div');
    card.className = 'user-card';
    card.innerHTML = `
      <h3>${user.name}</h3>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
    `;
    userList.appendChild(card);
  });
}

// Hook up button
document.querySelector('#load-btn').onclick = loadUsers;
```

**Understanding the new code:**

**Loading state:**
```js
statusDiv.textContent = 'Loading users...';
statusDiv.className = 'loading';
```
- Shows user feedback while data is loading
- Applies `.loading` CSS class (green text)
- Important for UX - users know something is happening!

**Clearing status:**
```js
statusDiv.textContent = '';
```
- Removes loading message after data arrives
- Clean UI once users are displayed

**The displayUsers function:**
```js
function displayUsers(users) {
  const userList = document.querySelector('#user-list');
  userList.innerHTML = ''; // Clear previous results
```
- Separate function for displaying (separation of concerns)
- `innerHTML = ''` clears any existing cards
- Prevents duplicates if button clicked multiple times

**Creating cards:**
```js
users.forEach(user => {
  const card = document.createElement('div');
  card.className = 'user-card';
  card.innerHTML = `
    <h3>${user.name}</h3>
    <p><strong>Username:</strong> ${user.username}</p>
    <p><strong>Email:</strong> ${user.email}</p>
    <p><strong>Phone:</strong> ${user.phone}</p>
  `;
  userList.appendChild(card);
});
```
- **`.forEach()`** loops through each user in the array
- **`createElement('div')`** creates a new div element
- **`card.innerHTML`** uses template literals (`` backticks) to create HTML
- **`${user.name}`** inserts the user's name into the string
- **`appendChild(card)`** adds the card to the page

**Why template literals?**
- Much easier than creating each element separately
- Clean, readable HTML structure
- Can insert variables with `${}`

**Checkpoint:**
1. Refresh the page
2. Click "Load Users"
3. You should see "Loading users..." briefly
4. Then 10 user cards should appear in a grid layout

---

## Part 2: Error Handling (15 minutes)

Right now, if something goes wrong, our app just breaks silently. Let's fix that!

### What Can Go Wrong?

1. **Network is offline** - Can't reach the server
2. **Server returns error** - 404, 500, etc.
3. **Response isn't valid JSON** - Server sends HTML instead
4. **Timeout** - Request takes too long

Let's handle these properly.

### Step 1: Understanding `response.ok` (5 minutes)

The Response object has a property called `ok`:
- `response.ok` is `true` if status code is 200-299 (success)
- `response.ok` is `false` for 400s, 500s, etc. (errors)

**Important:** Fetch only throws errors for **network failures**. A 404 response is considered "successful" by fetch (the request completed). We need to check `response.ok` ourselves!

**In `app.js`, update `loadUsers`:**

```js
async function loadUsers() {
  const statusDiv = document.querySelector('#status');
  const userList = document.querySelector('#user-list');

  statusDiv.textContent = 'Loading users...';
  statusDiv.className = 'loading';

  const response = await fetch('https://jsonplaceholder.typicode.com/users');

  // Check if response is ok
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  const users = await response.json();

  statusDiv.textContent = '';
  displayUsers(users);
}

// displayUsers function stays the same
function displayUsers(users) {
  const userList = document.querySelector('#user-list');
  userList.innerHTML = '';

  users.forEach(user => {
    const card = document.createElement('div');
    card.className = 'user-card';
    card.innerHTML = `
      <h3>${user.name}</h3>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
    `;
    userList.appendChild(card);
  });
}

document.querySelector('#load-btn').onclick = loadUsers;
```

**Understanding the check:**

```js
if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`);
}
```
- **`!response.ok`** means "if response is NOT ok"
- **`throw new Error()`** creates and throws an error
- **`${response.status}`** includes the status code (404, 500, etc.)
- This stops the function and jumps to error handling (which we'll add next)

**IMPORTANT:** Right now this `throw` will create an **uncaught exception** - the error shows in the console but nothing happens on the page. We're just setting up the check now. In the next step, we'll add `try/catch` to actually handle this error and show it to the user!

**Checkpoint:**
1. Save and test - should still work normally
2. Change the URL to `https://jsonplaceholder.typicode.com/userssss` (break it)
3. Click button - **you'll see an uncaught error in the console** (red text)
4. **The page won't show anything** - that's expected! We'll fix this in the next step.
5. Change URL back to `/users` before continuing

### Step 2: Add try/catch for Error Handling (10 minutes)

Now let's catch errors and show them to the user.

**In `app.js`, wrap the loadUsers function logic in try/catch:**

```js
async function loadUsers() {
  const statusDiv = document.querySelector('#status');
  const userList = document.querySelector('#user-list');

  try {
    // Show loading state
    statusDiv.textContent = 'Loading users...';
    statusDiv.className = 'loading';
    userList.innerHTML = ''; // Clear previous results

    // Fetch data
    const response = await fetch('https://jsonplaceholder.typicode.com/users');

    // Check if response is ok
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // Parse JSON
    const users = await response.json();

    // Display results
    statusDiv.textContent = '';
    displayUsers(users);

  } catch (error) {
    // Handle any errors
    console.error('Error loading users:', error);
    statusDiv.innerHTML = `
      <div class="error">
        <strong>Error loading users!</strong><br>
        ${error.message}
      </div>
    `;
    statusDiv.className = '';
  }
}

function displayUsers(users) {
  const userList = document.querySelector('#user-list');

  users.forEach(user => {
    const card = document.createElement('div');
    card.className = 'user-card';
    card.innerHTML = `
      <h3>${user.name}</h3>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
    `;
    userList.appendChild(card);
  });
}

document.querySelector('#load-btn').onclick = loadUsers;
```

**Understanding try/catch:**

```js
try {
  // Code that might fail
} catch (error) {
  // Code that runs if something in try{} fails
}
```

**How it works:**
1. JavaScript tries to run everything in the `try` block
2. If ANY error occurs (network failure, `throw`, etc.), it immediately jumps to `catch`
3. The `catch` block receives the error object
4. We can log it, display it, or handle it however we want

**The catch block:**
```js
catch (error) {
  console.error('Error loading users:', error);
  statusDiv.innerHTML = `
    <div class="error">
      <strong>Error loading users!</strong><br>
      ${error.message}
    </div>
  `;
  statusDiv.className = '';
}
```
- **`console.error()`** logs the full error (useful for debugging)
- **`statusDiv.innerHTML`** shows user-friendly error message
- **`error.message`** contains the error description
- **`.error` class** applies red styling

**Why this is important:**
- Users see helpful messages instead of broken pages
- Developers can debug with console logs
- App doesn't crash completely
- Professional user experience

**Checkpoint - Test error handling:**

**Test 1: Network error**
1. Open DevTools → Network tab
2. Set throttling to "Offline"
3. Click "Load Users"
4. Should see error message: "Failed to fetch"
5. Set back to "No throttling"

**Test 2: Bad URL**
1. Change URL to `https://jsonplaceholder.typicode.com/nope`
2. Click "Load Users"
3. Should see: "HTTP error! status: 404"
4. Change URL back to `/users`

**Test 3: Normal operation**
1. With correct URL, click "Load Users"
2. Should work normally with no errors

**Success!** Your app now handles errors gracefully.

---

## Part 3: Adding User Details (20 minutes)

Let's make it more interactive! When you click a user, we'll fetch and display their full details.

### Understanding the Details API

JSONPlaceholder provides individual user endpoints:
- `https://jsonplaceholder.typicode.com/users/1` - Gets user #1
- `https://jsonplaceholder.typicode.com/users/2` - Gets user #2
- etc.

This returns MORE data than the list, including full address and company info.

### Step 1: Make Cards Clickable (5 minutes)

**In `app.js`, update the `displayUsers` function:**

```js
function displayUsers(users) {
  const userList = document.querySelector('#user-list');

  users.forEach(user => {
    const card = document.createElement('div');
    card.className = 'user-card';
    card.innerHTML = `
      <h3>${user.name}</h3>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
      <p class="click-hint">Click for details →</p>
    `;

    // Make card clickable
    card.style.cursor = 'pointer';
    card.onclick = () => loadUserDetails(user.id);

    userList.appendChild(card);
  });
}
```

**Add CSS for the click hint to `styles.css`:**

```css
.click-hint {
  color: #4CAF50;
  font-style: italic;
  margin-top: 10px;
  font-size: 14px;
}

.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  transition: all 0.2s;
}
```

**Understanding the changes:**

```js
card.style.cursor = 'pointer';
```
- Changes cursor to pointer (hand) on hover
- Visual cue that card is clickable

```js
card.onclick = () => loadUserDetails(user.id);
```
- Adds click handler to each card
- Arrow function wraps the call so we can pass `user.id`
- When clicked, calls `loadUserDetails` with that user's ID

**The hover effect in CSS:**
```css
.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  transition: all 0.2s;
}
```
- Card lifts up slightly on hover
- Shadow gets bigger
- Smooth transition over 0.2 seconds
- Gives tactile feedback

**Checkpoint:** Save and test. Cards should show pointer cursor on hover and have a nice lift effect. Clicking will show an error (we haven't written `loadUserDetails` yet).

### Step 2: Create Modal for Details (5 minutes)

Let's add HTML for a modal (popup) to show user details.

**In `index.html`, add this BEFORE the closing `</body>` tag:**

```html
  <!-- Modal for user details -->
  <div id="modal" class="modal hidden">
    <div class="modal-content">
      <span class="close">&times;</span>
      <div id="modal-body"></div>
    </div>
  </div>

  <script src="app.js"></script>
</body>
```

**Add modal CSS to `styles.css`:**

```css
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal.hidden {
  display: none;
}

.modal-content {
  background: white;
  padding: 30px;
  border-radius: 8px;
  max-width: 600px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
  position: relative;
}

.close {
  position: absolute;
  top: 10px;
  right: 20px;
  font-size: 30px;
  cursor: pointer;
  color: #999;
}

.close:hover {
  color: #333;
}

.detail-section {
  margin-bottom: 20px;
}

.detail-section h3 {
  color: #4CAF50;
  margin-bottom: 10px;
}

.detail-section p {
  margin-bottom: 5px;
}
```

**Understanding the modal:**

**The HTML structure:**
```html
<div id="modal" class="modal hidden">
```
- Starts hidden (`.hidden` class)
- Covers entire viewport when shown

**The CSS:**
```css
position: fixed;
top: 0;
left: 0;
width: 100%;
height: 100%;
background: rgba(0, 0, 0, 0.5);
```
- `position: fixed` - Stays in place when scrolling
- Covers entire screen
- Semi-transparent black background (the "overlay")

```css
display: flex;
justify-content: center;
align-items: center;
```
- Centers the modal content on screen
- Works regardless of content size

**Checkpoint:** The modal is hidden, so you won't see anything yet. That's correct!

### Step 3: Fetch and Display User Details (10 minutes)

Now let's fetch individual user details and show them in the modal.

**In `app.js`, add these functions BEFORE the button hookup line:**

```js
async function loadUserDetails(userId) {
  const modal = document.querySelector('#modal');
  const modalBody = document.querySelector('#modal-body');

  try {
    // Show modal with loading state
    modalBody.innerHTML = '<p class="loading">Loading user details...</p>';
    modal.classList.remove('hidden');

    // Fetch individual user data
    const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const user = await response.json();

    // Display user details
    displayUserDetails(user);

  } catch (error) {
    console.error('Error loading user details:', error);
    modalBody.innerHTML = `
      <div class="error">
        <strong>Error loading details!</strong><br>
        ${error.message}
      </div>
    `;
  }
}

function displayUserDetails(user) {
  const modalBody = document.querySelector('#modal-body');

  modalBody.innerHTML = `
    <div class="detail-section">
      <h2>${user.name}</h2>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
      <p><strong>Website:</strong> <a href="http://${user.website}" target="_blank">${user.website}</a></p>
    </div>

    <div class="detail-section">
      <h3>Address</h3>
      <p>${user.address.street}, ${user.address.suite}</p>
      <p>${user.address.city}, ${user.address.zipcode}</p>
    </div>

    <div class="detail-section">
      <h3>Company</h3>
      <p><strong>Name:</strong> ${user.company.name}</p>
      <p><strong>Catchphrase:</strong> ${user.company.catchPhrase}</p>
      <p><em>"${user.company.bs}"</em></p>
    </div>
  `;
}

// Close modal when clicking X or outside
document.addEventListener('DOMContentLoaded', () => {
  const modal = document.querySelector('#modal');
  const closeBtn = document.querySelector('.close');

  closeBtn.onclick = () => modal.classList.add('hidden');

  modal.onclick = (e) => {
    if (e.target === modal) {
      modal.classList.add('hidden');
    }
  };
});
```

**Understanding the new code:**

**Template literals with variables:**
```js
const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
```
- **Backticks** allow inserting variables into strings
- **`${userId}`** inserts the actual user ID
- If userId is 5, URL becomes: `https://jsonplaceholder.typicode.com/users/5`

**Showing the modal:**
```js
modalBody.innerHTML = '<p class="loading">Loading user details...</p>';
modal.classList.remove('hidden');
```
- Set loading message
- Remove `hidden` class - modal becomes visible
- User sees immediate feedback

**Nested object properties:**
```js
<p>${user.address.street}, ${user.address.suite}</p>
<p>${user.address.city}, ${user.address.zipcode}</p>
```
- `user.address` is an object
- `user.address.street` accesses the street property inside address
- JavaScript can "drill down" through nested objects

**Modal closing logic:**
```js
closeBtn.onclick = () => modal.classList.add('hidden');
```
- Clicking X button adds `hidden` class back
- Modal disappears

```js
modal.onclick = (e) => {
  if (e.target === modal) {
    modal.classList.add('hidden');
  }
};
```
- Clicking outside the modal content (on the dark overlay) also closes it
- `e.target === modal` checks if click was on the overlay itself, not the content
- Common UX pattern for modals

**DOMContentLoaded:**
```js
document.addEventListener('DOMContentLoaded', () => {
```
- Waits for HTML to fully load before attaching event listeners
- Ensures `.close` button exists before we try to use it
- Good practice for any code that manipulates the DOM

**Checkpoint - Full test:**
1. Save all files and refresh browser
2. Click "Load Users"
3. Click on any user card
4. Modal should appear with loading message, then full details
5. Try closing modal by:
   - Clicking the X
   - Clicking outside the modal (on dark area)
6. Click different users - each should show their unique details

---

## Part 4: Code Organization and Best Practices (10 minutes)

Let's review our complete code and discuss best practices.

### Your Complete `app.js` Should Look Like This:

```js
// ============================================
// FETCH USER LIST
// ============================================

async function loadUsers() {
  const statusDiv = document.querySelector('#status');
  const userList = document.querySelector('#user-list');

  try {
    // Show loading state
    statusDiv.textContent = 'Loading users...';
    statusDiv.className = 'loading';
    userList.innerHTML = '';

    // Fetch data
    const response = await fetch('https://jsonplaceholder.typicode.com/users');

    // Check if response is ok
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // Parse JSON
    const users = await response.json();

    // Display results
    statusDiv.textContent = '';
    displayUsers(users);

  } catch (error) {
    console.error('Error loading users:', error);
    statusDiv.innerHTML = `
      <div class="error">
        <strong>Error loading users!</strong><br>
        ${error.message}
      </div>
    `;
    statusDiv.className = '';
  }
}

function displayUsers(users) {
  const userList = document.querySelector('#user-list');

  users.forEach(user => {
    const card = document.createElement('div');
    card.className = 'user-card';
    card.innerHTML = `
      <h3>${user.name}</h3>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
      <p class="click-hint">Click for details →</p>
    `;

    card.style.cursor = 'pointer';
    card.onclick = () => loadUserDetails(user.id);

    userList.appendChild(card);
  });
}

// ============================================
// FETCH USER DETAILS
// ============================================

async function loadUserDetails(userId) {
  const modal = document.querySelector('#modal');
  const modalBody = document.querySelector('#modal-body');

  try {
    // Show modal with loading state
    modalBody.innerHTML = '<p class="loading">Loading user details...</p>';
    modal.classList.remove('hidden');

    // Fetch individual user data
    const response = await fetch(`https://jsonplaceholders.typicode.com/users/${userId}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const user = await response.json();

    // Display user details
    displayUserDetails(user);

  } catch (error) {
    console.error('Error loading user details:', error);
    modalBody.innerHTML = `
      <div class="error">
        <strong>Error loading details!</strong><br>
        ${error.message}
      </div>
    `;
  }
}

function displayUserDetails(user) {
  const modalBody = document.querySelector('#modal-body');

  modalBody.innerHTML = `
    <div class="detail-section">
      <h2>${user.name}</h2>
      <p><strong>Username:</strong> ${user.username}</p>
      <p><strong>Email:</strong> ${user.email}</p>
      <p><strong>Phone:</strong> ${user.phone}</p>
      <p><strong>Website:</strong> <a href="http://${user.website}" target="_blank">${user.website}</a></p>
    </div>

    <div class="detail-section">
      <h3>Address</h3>
      <p>${user.address.street}, ${user.address.suite}</p>
      <p>${user.address.city}, ${user.address.zipcode}</p>
    </div>

    <div class="detail-section">
      <h3>Company</h3>
      <p><strong>Name:</strong> ${user.company.name}</p>
      <p><strong>Catchphrase:</strong> ${user.company.catchPhrase}</p>
      <p><em>"${user.company.bs}"</em></p>
    </div>
  `;
}

// ============================================
// MODAL CONTROLS
// ============================================

document.addEventListener('DOMContentLoaded', () => {
  const modal = document.querySelector('#modal');
  const closeBtn = document.querySelector('.close');

  closeBtn.onclick = () => modal.classList.add('hidden');

  modal.onclick = (e) => {
    if (e.target === modal) {
      modal.classList.add('hidden');
    }
  };
});

// ============================================
// INITIALIZE
// ============================================

document.querySelector('#load-btn').onclick = loadUsers;
```

### Best Practices We Used

#### 1. Separation of Concerns
- **Fetching functions** (`loadUsers`, `loadUserDetails`) - Get data
- **Display functions** (`displayUsers`, `displayUserDetails`) - Show data
- Each function has ONE job
- Makes code easier to read, test, and modify

#### 2. Error Handling Everywhere
- Every `async` function has `try/catch`
- Check `response.ok` for HTTP errors
- Show user-friendly error messages
- Log detailed errors to console for debugging

#### 3. User Feedback
- Loading states ("Loading users...")
- Clear error messages
- Visual feedback (hover effects, cursor changes)
- Professional user experience

#### 4. Consistent Code Style
- Clear variable names (`userList`, `statusDiv`)
- Comments for major sections
- Consistent indentation
- Template literals for readable HTML

#### 5. DRY Principle (Don't Repeat Yourself)
- Reusable `displayUsers` function
- Consistent error handling pattern
- Shared CSS classes

---

## Part 5: Extensions and Challenges (10 minutes discussion)

Let's discuss ways to extend this project.

### Easy Extensions (Can do in class)

1. **Add a search feature**
```js
// Add input to HTML
<input type="text" id="search" placeholder="Search users...">

// Filter users in JavaScript
const searchTerm = document.querySelector('#search').value.toLowerCase();
const filtered = users.filter(user =>
  user.name.toLowerCase().includes(searchTerm)
);
displayUsers(filtered);
```

2. **Add user count**
```js
statusDiv.textContent = `Loaded ${users.length} users`;
```

3. **Sort users alphabetically**
```js
users.sort((a, b) => a.name.localeCompare(b.name));
```

### Medium Extensions (Homework)

1. **Fetch user's posts**
   - API: `https://jsonplaceholder.typicode.com/posts?userId={id}`
   - Show in the modal

2. **Add loading spinner**
   - CSS animation instead of text
   - More polished look

3. **Keyboard shortcuts**
   - ESC key to close modal
   - Enter to search

### Advanced Extensions (Extra Credit)

1. **Pagination**
   - Show 5 users at a time
   - "Next" and "Previous" buttons

2. **Favorites system**
   - Heart icon on each card
   - Save favorites to localStorage
   - "Show Favorites" filter

3. **Multiple API calls**
   - Fetch users, posts, and comments
   - Use `Promise.all()` to fetch in parallel

---

## Key Concepts Summary

### async/await
```js
async function myFunction() {
  const response = await fetch(url);
  const data = await response.json();
}
```
- `async` makes function return a Promise
- `await` pauses until Promise resolves
- Makes asynchronous code look synchronous
- Must use `await` inside `async` functions

### Fetch API
```js
const response = await fetch(url);
```
- Makes HTTP requests
- Returns a Promise that resolves to Response object
- Response contains status, headers, body
- Need to call `.json()` to parse body

### Error Handling
```js
try {
  // Code that might fail
} catch (error) {
  // Handle the error
}
```
- Wrap risky code in `try`
- Errors jump to `catch`
- Always check `response.ok`
- Show user-friendly messages

### Working with JSON
```js
const data = await response.json();
console.log(data.name);
console.log(data.address.city);
```
- `.json()` parses response as JSON
- Returns JavaScript object/array
- Access properties with dot notation
- Can nest multiple levels

---

## Common Mistakes and Debugging

### Mistake 1: Forgetting `await`

```js
// ❌ WRONG - Gets a Promise, not data
const response = fetch(url);
const data = response.json();

// ✅ CORRECT - Waits for data
const response = await fetch(url);
const data = await response.json();
```

**How to recognize:** Console shows `Promise {<pending>}` instead of data.

### Mistake 2: Forgetting `async`

```js
// ❌ WRONG - Can't use await without async
function getData() {
  const response = await fetch(url); // ERROR!
}

// ✅ CORRECT - Declare function as async
async function getData() {
  const response = await fetch(url);
}
```

**Error message:** "await is only valid in async functions"

### Mistake 3: Not Checking `response.ok`

```js
// ❌ WRONG - Doesn't catch 404, 500, etc.
const response = await fetch(url);
const data = await response.json();

// ✅ CORRECT - Check for HTTP errors
const response = await fetch(url);
if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`);
}
const data = await response.json();
```

**What happens:** App breaks when server returns errors.

### Mistake 4: Not Using try/catch

```js
// ❌ WRONG - Errors crash the app
async function getData() {
  const response = await fetch(url);
  const data = await response.json();
}

// ✅ CORRECT - Errors are caught
async function getData() {
  try {
    const response = await fetch(url);
    const data = await response.json();
  } catch (error) {
    console.error(error);
  }
}
```

**What happens:** Network errors show as uncaught exceptions.

### Debugging Tips

**If data isn't showing:**
1. Open DevTools console - check for errors
2. Check Network tab - did the request succeed?
3. Log the response: `console.log(response)`
4. Log the data: `console.log(data)`
5. Check spelling of property names

**If you get CORS errors:**
- JSONPlaceholder allows CORS, so this shouldn't happen
- If using a different API, it might not allow requests from browsers
- CORS is a security feature of browsers

**If modal won't open:**
1. Check console for errors
2. Verify modal doesn't have `hidden` class
3. Check z-index in CSS
4. Make sure JavaScript ran (check for syntax errors)

---

## What's Next?

### Related Topics to Explore

1. **Other HTTP Methods**
   - POST, PUT, DELETE (not just GET)
   - Sending data to servers
   - See MDN: Using Fetch

2. **Promises in depth (optional, theoretical)**
   - How promises work "under the hood"
   - Creating custom Promises with `new Promise()`
   - Understanding `resolve()` and `reject()`
   - See [HW-ajax-7.md](https://github.com/rit-igm-web/igme-330-shared/blob/main/notes/HW-ajax-7.md)
   - Note: You rarely need to create promises manually in real projects, but understanding how they work internally can be valuable!

3. **Modern alternatives**
   - Axios library (popular fetch wrapper)
   - React Query (for React apps)
   - SWR (stale-while-revalidate)

4. **Advanced patterns**
   - Retry logic
   - Request cancellation
   - Caching strategies
   - Request deduplication

### Homework Assignments

**For more depth on these topics, see:**
- [HW-ajax-5.md](https://github.com/rit-igm-web/igme-330-shared/blob/main/notes/HW-ajax-5.md) - Learn `.then()` syntax
- [HW-ajax-6.md](https://github.com/rit-igm-web/igme-330-shared/blob/main/notes/HW-ajax-6.md) - More async/await examples
- [HW-ajax-7.md](https://github.com/rit-igm-web/igme-330-shared/blob/main/notes/HW-ajax-7.md) - Deep dive into how Promises work internally (optional theoretical knowledge)

---

## Resources

- **MDN - Using Fetch**: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
- **MDN - async/await**: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await
- **JSONPlaceholder API**: https://jsonplaceholder.typicode.com
- **JavaScript.info - Async/await**: https://javascript.info/async-await

---

**Great job!** You've built a fully functional application that:
- Fetches data from an API
- Handles errors gracefully
- Shows loading states
- Displays data dynamically
- Responds to user interactions
- Uses modern async/await syntax

This foundation will serve you well for any web development involving APIs!
