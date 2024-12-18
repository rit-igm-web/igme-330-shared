<details>
  <summary>Table of Contents</summary>

- [JS Functions](#js-functions)
  - [I. Simple function, no parameters](#i-simple-function-no-parameters)
  - [II. A function that takes a parameter and returns a value](#ii-a-function-that-takes-a-parameter-and-returns-a-value)
  - [III. Functions declared with the `function` keyword can be called *before* they are declared](#iii-functions-declared-with-the-function-keyword-can-be-called-before-they-are-declared)
  - [IV. Functions can be declared *inside* of other functions](#iv-functions-can-be-declared-inside-of-other-functions)
  - [V. Recursion - functions can "call themselves"](#v-recursion---functions-can-call-themselves)
  - [VI. Arrow functions](#vi-arrow-functions)
    - [VI-A. Declarations with `function`](#vi-a-declarations-with-function)
    - [VI-B. Arrow Function examples](#vi-b-arrow-function-examples)
  - [VII. Functions are "First class" objects](#vii-functions-are-first-class-objects)
    - [VII-A. Functions can be assigned to variables and properties](#vii-a-functions-can-be-assigned-to-variables-and-properties)
    - [VII-B. Functions can be passed to other functions as parameters](#vii-b-functions-can-be-passed-to-other-functions-as-parameters)
    - [VII-C. Functions can have properties and methods just like any other object](#vii-c-functions-can-have-properties-and-methods-just-like-any-other-object)
    - [VII-D. Functions can `return` other functions](#vii-d-functions-can-return-other-functions)

</details>


# JS Functions

- Functions are one of the fundamental building blocks in JavaScript.
- A function is a "subprogram" that can be called by code external to the function.
- Like the program itself, a function is composed of a sequence of statements called the function body.
- Values can be passed to a function as parameters, and the function will `return` a value.
- In JavaScript, every function is actually a `Function` object.
  - [MDN: Function Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)
  - [MDN: Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)
  - [MDN: Functions Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
  - The "super power" of a function object is that (unlike other objects) it can be *called* or *invoked* with `()`.

---

## I. Simple function, no parameters

```js
let counter = 0;
function incrementCounter(){
  counter ++;
}

console.log(incrementCounter(), counter); // undefined, 1
```

- The above function takes no parameters.
- Because it lacks a `return` statement, the return value will default to `undefined`, which is standard behavior for all JavaScript functions without an explicit `return`.
- A function can access all variables and functions defined inside the scope in which it is *defined* (meaning, the "parent scope").
  - The function above has access to and is modifying the global `counter` variable.
  - In some languages, the above function would be called a *procedure* because it does not return a value and instead relies on the "side effect" of modifying a part of the program that is outside of its own scope.
  - Functions that have *side effects* are sometimes called *impure functions* - [Pure vs. Impure Functions](https://www.freecodecamp.org/news/pure-function-vs-impure-function/).

---

## II. A function that takes a parameter and returns a value

```js
function doubleIt(num){
  return num * 2;
}

const doubleTen = doubleIt(10);
console.log(doubleTen); // 20
```

- Specifying default values for parameters is easy:

```js
function doubleIt(num = 0){
  return num * 2;
}

console.log(doubleIt()); // returns 0 * 2, which is 0
console.log(num); // ERROR - num is not in scope outside of function
```

- Because the above function does NOT have any *side effects*, it can be called a *pure function*.
  - [Pure vs. Impure Functions](https://www.freecodecamp.org/news/pure-function-vs-impure-function/)
- Note that `num` above is *defined* inside the `doubleIt()` function and cannot be accessed from anywhere outside the `doubleIt()` function due to JavaScript's lexical scoping rules.

---

## III. Functions declared with the `function` keyword can be called *before* they are declared
- The `function` declaration creates functions that are [hoisted](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#function_hoisting).
- This means that they can be called *before* they are declared.

```js
console.log(doubleIt(10)); // 20

// ...

function doubleIt(num = 0){
  return num * 2;
}
```

- However, only function declarations are hoisted, not function expressions or arrow functions.

---

## IV. Functions can be declared *inside* of other functions

- In the example below, `capitalize()` is "private" to the `sayHello()` function and not visible outside of it (and `name` and `capFirstLetter` are similarly not visible from outside the `sayHello()` function).

```js
function sayHello(name, capFirstLetter){
  function capitalize(str){
    return str.charAt(0).toUpperCase() + str.slice(1);
  }
  
  if(capFirstLetter){
    name = capitalize(name);
  }
  
  return "Hello " + name;
}

console.log(sayHello("ace")); // Hello ace
console.log(sayHello("ace", true)); // Hello Ace
console.log(capitalize("billy")); // Uncaught ReferenceError: capitalize is not defined
```

- Why would you do something like this?
  - *Encapsulation* and *information hiding* along with a *stable interface* generally make for better software.
  - [Information Hiding](https://en.wikipedia.org/wiki/Information_hiding)

---

## V. Recursion - functions can "call themselves"

- Recursion is the act of a function calling itself, and is used to solve problems that contain smaller sub-problems.
- Recursion is limited by the call stack size limit of the JS run-time (which varies by machine & browser, but is usually over 10,000 on modern browsers).
  - [Recursion](https://developer.mozilla.org/en-US/docs/Glossary/Recursion)
  - [Too Much Recursion Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Too_much_recursion)
  - [Recursion vs. Loops](https://dev.to/thawkin3/recursion-vs-loops-in-javascript-14em)
  - [Recursion (Computer Science)](https://en.wikipedia.org/wiki/Recursion_(computer_science))

```js
function factorial(n){
  if (n === 0) {
    return 1;
  } else {
    return n * factorial(n - 1);
  }
}
console.log(factorial(10)); // 3628800
```

- It's important to have a base case in recursion to avoid infinite recursion, which would lead to a stack overflow error.

---

## VI. Arrow functions

- Arrow functions are a compact alternative to a traditional function expression where a function is declared with the `function` keyword.
- ***The major difference between an arrow function and a regular function (other than the compact syntax) is that arrow functions do not bind their own `this` context***.
  - This behavior is especially useful in scenarios like event listeners, callbacks, and methods in classes, where it helps to maintain the intended `this` context.
  - [Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

---

### VI-A. Declarations with `function`

- "Old school" function declaration.
- Note that we can call it *before* it is declared.

```js
console.log(doubleIt(10)); // 20

function doubleIt(num = 0){
  return num * 2;
}
```

- See below that we can assign a `function` to a variable as an *expression*.
- However, now (because we are using `const` or `let`) we can only call the function AFTER we have declared it.

```js
console.log(doubleIt(10)); // ERROR: Uncaught ReferenceError: doubleIt is not defined

const doubleIt = function(num = 0){
  return num * 2;
};

console.log(doubleIt(10)); // 20
```

---

### VI-B. Arrow Function examples

- We have to assign them to a variable or constant.

**Version #1** of `doubleIt()` as an arrow function:

```js
const doubleIt = (num = 0) => {
  return num * 2;
};

console.log(doubleIt(10)); // 20
```

**Version #2**:
- If we only have one line of code in the function body, we can get rid of the curly braces.
- And we can get rid of `return` as well, and whatever that line of code evaluates to will be returned.

```js
const doubleIt = (num = 0) => num * 2;
```

**Version #3**:
- If there is only one parameter, and we get rid of the default value, we can also get rid of the parentheses around the arguments.
- It's worth noting, though, that many JS style guides require parentheses for arrow functions.
  - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript#arrows--one-arg-parens)

```js
const doubleIt = num => num * 2;
```

**A real example**:
- Which is easier to read?

```js
const scores = [100000, 90, 85, 500, 22, 54321, 1001];
// get rid of scores higher than 999
// #1 - Use an arrow function
const filteredScores = scores.filter(s => s < 1000);
console.log(filteredScores);

// #2 - Use a regular function
const filteredScores2 = scores.filter(function(s){
  return s < 1000;
});
console.log(filteredScores2);
```

- Note above that both of the functions being passed to `array.filter()` above are *anonymous functions*.
- Alternatively, they could have been *named* function expressions.
- One obvious advantage of naming the filtering function above is that we could *reuse* it.

```js
const scores1 = [100000, 90, 85, 500, 22, 54321, 1001];
const scores2 = [-13, 82, 300000, 77, 900, 11, 54321, 1001];

const removeInvalidScores = s => s >= 0 && s <= 1000; // OK, this line is a little ugly!

const filteredScores1 = scores1.filter(removeInvalidScores);
const filteredScores2 = scores2.filter(removeInvalidScores);
console.log(filteredScores1); // [90, 85, 500, 22]
console.log(filteredScores2); // [82, 77, 900, 11]
```

- `array.filter()` above calls the `removeInvalidScores()` function on every element in the `scores1` and `scores2` arrays, and returns a new array with only those elements "that pass the test" implemented by the provided function.
- BTW - functions that operate on other functions, either by taking them as arguments or by returning them, are called *higher-order functions*.
- Meaning that `array.filter()` is a *higher-order function* (or *higher-order method*, because `filter()` is a method of the `Array` object).

---

## VII. Functions are "First class" objects
- In JavaScript, functions are [first-class](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function) objects:
  - because they can be passed to other functions as parameters,
  - returned from functions,
  - and assigned to variables and properties.
- They can also have properties and methods just like any other object.
- What distinguishes them from other objects is that functions can be *called* (or *invoked*) with `()`.

---

### VII-A. Functions can be assigned to variables and properties

```js
// Declare some functions
function doubleItAgain(num = 0){
  return num * 2;
};

const doubleItAgainAgain = num => num * 2;

// Assign them to variables
let dub = doubleItAgain;
let dub2 = doubleItAgainAgain;

// Call them
console.log(dub(10)); // 20
console.log(dub2(10)); // 20

// Assign as a property of an object and call it
const obj = {
  doubleIt: dub
};
obj.doubleIt2 = dub2;

console.log(obj.doubleIt(10)); // 20
console.log(obj.doubleIt2(10)); // 20
```

---

### VII-B. Functions can be passed to other functions as parameters

```js
const tripleIt = num => num * 3;

const doSomeMaths = (funcRef, value) => {
  let result = funcRef(value);
  return result;
};

console.log(doSomeMaths(tripleIt, 100)); // 300
```

- `doSomeMaths()` above is a *higher-order function* because we are passing it a function as an argument, and it is performing operations with that function.

---

### VII-C. Functions can have properties and methods just like any other object

```js
doSomeMaths.useCount = 1; // add a property to the function - 'cause it's an object, remember?
console.log(doSomeMaths.useCount); // 1
```

- Note: We don't usually add properties or methods to our function objects, but it's good to know that we CAN.
- In the debugger, you can see that functions have a `name` property (this is sometimes useful for the developer when debugging code).

---

### VII-D. Functions can `return` other functions

- Here's an example of a function that returns a function.
- This particular example creates a [closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) - achieving true encapsulation/privacy of the `heroHP` and `heroName` variables.
- Closures allow functions to "remember" the scope in which they were created, enabling encapsulation and privacy.
- We'll talk more about closures later in the course.

```js
const heroMaker = (name) => {
  let heroHP = Math.floor(Math.random() * 10) + 1;
  let heroName = name;
  return (newHP) => {
    if(newHP >= 0) heroHP = newHP;
    console.log(`${heroName} now has ${heroHP} hitpoints`);
    return heroHP;
  };
};

let hero1 = heroMaker("Ace");
let hero2 = heroMaker("Coder");
console.log(hero1());
console.log(hero1(20));
console.log(hero2());
console.log(hero2(50));
console.log(hero1()); // still has 20 hitpoints
```
