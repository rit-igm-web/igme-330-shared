# Callback Function Demo

This is intended to give you a little more practice with callback functions before you'll likely be encountering them on the first Practical.  So let's start with a tutorial:

## **Tutorial: Callbacks with a Custom Cake Order**
### **I: Explaining Callbacks (The Bakery Analogy)**
Imagine you walk into a bakery and order a custom cake with your name on it. You don’t stand around waiting – instead, you give them your phone number, and they call you when the cake is ready.

👉 **Ordering the cake = making a request**  
👉 **Giving your phone number = passing a callback function**  
👉 **Getting called back = executing the callback later**  

Now, let's see this in JavaScript:

---

### **II: Example 1 – Named Function as a Callback**
In this example, we define a separate function and **pass its name** as a callback:

```javascript
const notifyCustomer = (cake) => {
    console.log(`Your ${cake} is ready for pickup!`);
};

const orderCake = (flavor, callback) => {
    console.log(`Ordering a ${flavor} cake...`);

    setTimeout(() => { // Simulating the time needed to bake the cake
        const cake = `${flavor} cake with frosting`;
        callback(cake); // Call the callback function with the cake
    }, 2000);
};

// Using the function with a named callback
orderCake('chocolate', notifyCustomer);
```

**Explanation:**
- `orderCake` **takes a callback function** and calls it when the cake is ready.
- We **pass `notifyCustomer` by name**. (a function reference).
- The callback **executes separately** once the cake is "done."

**Output:**
```
Ordering a chocolate cake...
(Waits 2 seconds)
Your chocolate cake with frosting is ready for pickup!
```

Good so far?  Let's remove the need for a separate `notifyCustomer` function...

---

### **III: Example 2 – Using an Inline Arrow Function**
Instead of defining the callback separately, we can use an **anonymous arrow function** inline:

```javascript
orderCake('vanilla', (cake) => {
    console.log(`Enjoying my delicious ${cake}!`);
});
```

**Key Differences:**
- Instead of passing a function **by name**, we **define the callback directly** in the function call.
- This is useful when we only need the callback **once** and don’t need to reuse it.

---

## **Student Exercises – Fetching Colors 1**
### **Instructions:**
1. Complete the function **signature** for `fetchColors(callback)`.
2. Call `callback(colors)` where appropriate.
3. Call `fetchColors`, passing `displayColors` as the callback.

### **Starter Code:**
```javascript
const displayColors = (colorList) => {
    document.querySelector("#color-list").innerHTML = 
        `<ul>${colorList.map(color => `<li>${color}</li>`).join('')}</ul>`;
};

// TODO: Complete the function signature
const fetchColors = () => {
    setTimeout(() => {
        const colors = ["Red", "Blue", "Green"];
        // TODO: Call the callback function with the colors array
    }, 1000);
};

// TODO: Call fetchColors, passing displayColors as the callback
```
How would you re-write it to send an anonymous arrow function to fetchColor instead of a function reference to `displayColors`?

---

## **Student Exercises - Fetching Colors 2**
This version is a little more challenging:

In this one, you'll complete the function **`fetchColors`**, which will:  
  - **Take a passed in number (`count`) as input** – the number of colors to retrieve.
  - **Select that many colors** from a pre-defined **rainbow** array.  
  - **Call the (also passed in) callback function**, passing in the selected colors once they are ready.  

---

### **Instructions:**
1. Complete the function signature for **`fetchColors()`** so that:
   - It **takes two parameters**: `count` (number of colors to fetch) and `callback` (function to execute once colors are ready).
   - It **simulates fetching data** by using `setTimeout()`.  
2. Inside `fetchColors`:
   - **Select up to `count` colors** from the list of **rainbow colors**.
   - If `count` is **not provided** or `0`, **return 1 color**. (the Math.max method can help with this).
   - If `count` is **greater than 7**, return **7 colors**. (the Math.min method can help with this).
   - **Call the callback function**, passing the selected colors. (the Array.slice method can help with this).
3. Call `fetchColors()` to fetch 4 colors and display them on the page.
   - What arguments will you need to send?

---

### **Starter Code:**
```javascript
const displayColors = (colorList) => {
    document.querySelector("#color-list").innerHTML = 
        `<ul>${colorList.map(color => `<li>${color}</li>`).join('')}</ul>`;
};

// TODO: Complete the function signature so it accepts count and callback
const fetchColors = () => {
    setTimeout(() => {
        const rainbowColors = ["Red", "Orange", "Yellow", "Green", "Blue", "Indigo", "Violet"];

        // TODO: Select up to `count` colors from rainbowColors
        // If `count` is not provided or 0, default to 1 color
        // If `count` is greater than 7, limit it to 7

        // TODO: Call the callback function with the selected colors
    }, 2000);
};

// TODO: Call fetchColors with 4 and pass displayColors as the callback
```

---

### **Example Expected Output**
If you call:
```javascript
fetchColors(4, displayColors);
```
After **2 second**, the `#color-list` container will update to:

```html
<ul>
    <li>Red</li>
    <li>Orange</li>
    <li>Yellow</li>
    <li>Green</li>
</ul>
```

---

### Want help with the Math.min, Math.max, and Array.slice parts?
Okay, since they aren't the point of this exercise, we'll give them to you:
```javascript
        // Ensure count is at least 1 and at most 7
        const limitedCount = Math.min(Math.max(count, 1), 7);

        // Select the requested number of colors
        const colors = rainbowColors.slice(0, limitedCount);
```
