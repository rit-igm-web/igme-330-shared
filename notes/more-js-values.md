# Notes - More about JS values

## A. Falsy values
- "A falsy value is a value that is considered false when encountered in a `Boolean` context"
- https://developer.mozilla.org/en-US/docs/Glossary/Falsy
- In a boolean context, any value that is NOT a falsy value is true
- Examples

```js
const values = [false, 0, -0, undefined, null, NaN, "", '', ``, 0n, 0.0, true, "Ace Coder", 0.00000000000001, "0"];
for (let value of values){
  if(value){
    console.log(`${value} is true`);
   }else{
    console.log(`${value} is false`);
  }
}
```

<hr>

## B. `NaN`
- The global `NaN` property is a value representing Not-A-Number.
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN
- There are five different types of operations that return `NaN` (see link above)
- Test for `NaN` with `isNan()` - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN
- Examples:

```js
2 + 2;  // arithmetic, result is a Number
2 + "two"; // in JS adding a string and a number is string concatenation, so the result is a String
2 * "two"; // in JS with multiplication, division etc this is NaN (but in Python, it returns "twotwo")
2 + undefined; // NaN - a common mistake if a variable or property is unexpectedly undefined - watch for this in your code
2 * undefined; // NaN - a common mistake if a variable or property is unexpectedly undefined - watch for this in your code
Math.sqrt(-1) // NaN
```

- `NaN` demo code you can run:

```js
const expressions = ["2 + 2", "2 + 'two'", "2 * 'two'", "2 + undefined", "2 * undefined", "Math.sqrt(-1)"];
for (let exp of expressions){
  let result = eval(exp); // eval()!
  if(isNaN(result)){
    console.log(`exp = ${exp}. result = ${result} - this evaluates to NaN`);
  }else{
  console.log(`exp = ${exp}. result = ${result} - this is a Number`);
  }
}
```

- Note the use of `eval()`:
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval
  - `eval()` evaluates JavaScript code represented as a string and returns its completion value

<hr>

## C. Equality `==` v. Strict Equality `===`

- The equality (`==`) operator checks whether its two operands are equal, returning a `Boolean` result. 
  - Unlike the *strict equality* operator, it attempts to convert and compare operands that are of different types.
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality
- The strict equality (`===`) operator checks whether its two operands are equal, returning a `Boolean` result. 
  - Unlike the *equality* operator, the strict equality operator always considers operands of different types to be different.
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality

```js
// Equality
console.log(1 == 1); // true
console.log('hello' == 'hello'); // true
console.log('1' ==  1); // true because JS converts '1' to a Number before doing the comparison
console.log(0 == false); // true because JS converts 0 to a Boolean before doing the comparison

// Strict Equality
console.log(1 === 1); // true
console.log('hello' === 'hello'); // true
console.log('1' ===  1); // false
console.log(0 === false); // false
```

- In this course we won't be specifying which one of these you use, but you definitely need to be aware of the differences
- Many JS style guides require the use of `===` over `==`
- For example: https://github.com/airbnb/javascript#comparison-operators--equality

<hr>

## D. Template Strings

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals
- Encloses characters in backticks - `` ` `` - which is next to your keyboard's escape key
- Get used to them - we'll we using them a lot in this course
- Unlike single or double-quoted strings, can be multi-line in your source code. We'll see this in our Web Components - it makes it easy to have nicely formatted HTML templates
- Within the backticks, any JS [*expression*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators) (not just a variable name) inside of `${}` is *evaluated*

```js
let greeting = "Hello";
let title = "Potentate";
let firstName = "Ace";
let middleInitial = "B.";
let lastName = "Coder";
// string concatenation
let salutation = greeting + " " + title + " " + firstName + " " + middleInitial + " " + lastName + "!";
// template string
let salutation2 = `${greeting} ${title} ${firstName} ${middleInitial} ${lastName} !`;
```

- *Which one is easier to code and read?*
