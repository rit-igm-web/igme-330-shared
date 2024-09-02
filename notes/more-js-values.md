# Notes - More about JS values

<details>
  <summary><strong>Table of Contents</strong></summary>

- [A. Falsy Values](#a-falsy-values)
- [B. `NaN`](#b-nan)
- [C. Equality (`==`) vs. Strict Equality (`===`)](#c-equality-vs-strict-equality)
- [D. Template Strings](#d-template-strings)

</details>


## A. Falsy Values
- In JavaScript, a falsy value is a value that is considered `false` when encountered in a Boolean context.
- [MDN Reference - Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
- In a Boolean context, any value that is not a falsy value is considered truthy.

### **Common Falsy Values in JavaScript**
The following are the primary falsy values in JavaScript:
1. `false` - The Boolean value `false`.
2. `null` - Represents the absence of any value.
3. `undefined` - A variable that has not been assigned a value.
4. `NaN` - Not-a-Number, often the result of invalid mathematical operations.
5. `0` and `-0` - Both positive and negative zero are falsy.
6. `0n` - BigInt zero, representing zero in the BigInt type.
7. `""`, `''`, `` `` - Empty strings in any quote type are falsy.

### **Examples of Falsy Values in Action**

```js
const values = [false, 0, -0, undefined, null, NaN, "", '', ``, 0n, true, "Ace Coder", 0.00000000000001, "0"];
for (let value of values) {
  if (value) {
    console.log(`${value} is truthy`);
  } else {
    console.log(`${value} is falsy`);
  }
}
```

<hr>

## B. `NaN`
- The global `NaN` property represents "Not-A-Number," a special value used to indicate that a mathematical operation or conversion has failed to produce a valid number.
- [MDN Reference - `NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)
- Common operations that result in `NaN` include:
  - Arithmetic with non-numeric operands (e.g., `2 * 'two'`)
  - Mathematical functions with invalid inputs (e.g., `Math.sqrt(-1)`)
  - Parsing operations that fail (e.g., `parseInt('abc')`)
  - Operations involving `undefined` (e.g., `2 + undefined`)
  - Self-referencing `NaN` in calculations (e.g., `NaN + 1`)
  
- **Checking for `NaN` in JavaScript:**
  - Use `isNaN()` to check if a value can be coerced to `NaN`. This method performs type coercion before checking, which can lead to misleading results.
  - Use `Number.isNaN()` to strictly check if a value is exactly `NaN` without type coercion, providing a more precise check.
  - [MDN Reference - `isNaN()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN)
  - [MDN Reference - `Number.isNaN()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN)

- **Examples of `NaN`:**

    ```js
    2 + 2;             // Arithmetic, result is a Number (4)
    2 + "two";         // String concatenation, result is a String ("2two")
    2 * "two";         // Multiplication with a non-numeric string, result is NaN
    2 + undefined;     // Adding undefined, result is NaN - common mistake with undefined values
    2 * undefined;     // Multiplying by undefined, result is NaN
    Math.sqrt(-1);     // Square root of a negative number, result is NaN
    ```

- **Testing for `NaN` with `isNaN()` and `Number.isNaN()`:**

    ```js
    console.log(isNaN('hello'));       // true - 'hello' is coerced to NaN
    console.log(Number.isNaN('hello')); // false - 'hello' is not strictly NaN

    console.log(isNaN(undefined));     // true - undefined is coerced to NaN
    console.log(Number.isNaN(undefined)); // false - undefined is not NaN

    console.log(isNaN(NaN));           // true - NaN is NaN
    console.log(Number.isNaN(NaN));    // true - NaN is strictly NaN
    ```

- **`NaN` Demo Code You Can Run:**

    ```js
    const expressions = ["2 + 2", "2 + 'two'", "2 * 'two'", "2 + undefined", "2 * undefined", "Math.sqrt(-1)"];
    for (let exp of expressions) {
      let result = eval(exp); // eval()!
      if (Number.isNaN(result)) {
        console.log(`exp = ${exp}. result = ${result} - this evaluates to NaN`);
      } else {
        console.log(`exp = ${exp}. result = ${result} - this is a Number`);
      }
    }
    ```

- **Note on `eval()`:**
  - [MDN Reference - `eval()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)
  - `eval()` evaluates JavaScript code represented as a string and returns its completion value. Be cautious using `eval()` due to security risks and performance concerns. It’s generally best to avoid `eval()` in production code.


<hr>

## C. Equality (`==`) vs. Strict Equality (`===`)

### **Equality Operator (`==`)**
- The equality operator (`==`) checks if two values are equal, converting their types if necessary (type coercion).
- This can lead to unexpected results due to automatic type conversion.
- **Examples:**
  ```js
  console.log('1' == 1);      // true - string '1' is coerced to number 1
  console.log(0 == false);    // true - 0 is coerced to false
  console.log(null == undefined); // true - considered equal without conversion
  ```

### **Strict Equality Operator (`===`)**
- The strict equality operator (`===`) checks if two values are equal without type conversion.
- It compares both value and type, making comparisons more predictable.
- **Examples:**
  ```js
  console.log('1' === 1);     // false - different types (string vs number)
  console.log(0 === false);   // false - number vs boolean
  console.log(null === undefined); // false - different types
  ```

### **Key Takeaways**
- **`==`** performs type coercion, which can lead to unpredictable results.
- **`===`** does not coerce types and is generally preferred for reliable comparisons.
- **In this course:** We won’t be specifying which operator you must use, but you need to be aware of the differences. Many JavaScript style guides recommend using `===` over `==` for safer and clearer code.
- **Best Practice:** Use `===` to avoid the quirks of type coercion unless there is a specific reason to use `==`.


- **Reference:**
  - [MDN - Equality (`==`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality)
  - [MDN - Strict Equality (`===`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality)

<hr>

## D. Template Strings

- [MDN Reference - Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- **What are Template Strings?**  
  Template strings (or template literals) are enclosed in backticks `` ` ``, which are located next to the escape key on your keyboard. They provide a powerful way to work with strings in JavaScript.
  
- **Key Features of Template Strings:**
  - **Multi-line Strings**: Unlike single (`' '`) or double (`" "`) quoted strings, template strings can span multiple lines, making it easier to write formatted text or HTML.
  - **Embedded Expressions**: Use `${}` within backticks to evaluate any JavaScript expression inside the string, not just variable names.
  - **Enhanced Readability**: They simplify string construction, especially when working with dynamic content or concatenating multiple variables.

- **Example of Template Strings vs. String Concatenation:**
  
    ```js
    let greeting = "Hello";
    let title = "Potentate";
    let firstName = "Ace";
    let middleInitial = "B.";
    let lastName = "Coder";

    // Using string concatenation
    let salutation = greeting + " " + title + " " + firstName + " " + middleInitial + " " + lastName + "!";

    // Using a template string
    let salutation2 = `${greeting} ${title} ${firstName} ${middleInitial} ${lastName}!`;
    ```

- *Which one is easier to code and read?*
