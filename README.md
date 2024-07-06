# Ultimate-JavaScript-Interview-Preparation-Guide

Master JavaScript concepts for your next interview with our ultimate guide. Covering synchronous and asynchronous APIs, event loops, memory storage, identifiers, keywords, error handling, modular JavaScript, and more, this resource is designed to help you ace your JavaScript interviews.

## Table of Contents

- [JavaScript Interview Preparation](#javascript-interview-preparation)
  - [Synchronous APIs](#synchronous-apis)
  - [Asynchronous APIs](#asynchronous-apis)
  - [How Synchronous and Asynchronous APIs Work with the Event Loop](#how-synchronous-and-asynchronous-apis-work-with-the-event-loop)
  - [Conclusion](#conclusion)
  - [Identifiers in JavaScript](#identifiers-in-javascript)
  - [Keywords in JavaScript](#keywords-in-javascript)
  - [Memory Storage](#memory-storage)
  - ["use strict" in JavaScript](#"use-strict"-in-javascript)
  - [With](#with)
  - [Data Types or Types of Identifiers](#data-types-or-types-of-identifiers)
  - [Transpiler](#transpiler)
  - [Error Handling](#error-handling)
  - [Modular JavaScript [ES6]](#modular-javascript-[es6])
  - [Additional Modern JavaScript Features [ES6]](#additional-modern-javascript-features-[es6])
  - [Asynchronous Patterns](#asynchronous-patterns)
  - [Flow Control](#flow-control)
  - [Creating Objects](#creating-objects)
  - [Inheritance](#inheritance)
  - [`this` Keyword](#`this`-keyword)
  - [Closures](#closures)
  - [Recursion](#recursion)
  - [Memoization](#memoization)
  - [Hoisting](#hoisting)
  - [Arrays](#arrays)
  - [Type Casting and Coercion](#type-casting-and-coercion)
  - [Event Loop](#event-loop)

## JavaScript Interview Preparation

### Synchronous APIs

Synchronous APIs are blocking, meaning that they halt the execution of subsequent code until the current operation completes. When you call a synchronous function, the JavaScript engine executes it and does not move to the next line of code until the function has finished executing.

**Examples of Synchronous APIs:**

**Basic Operations:**

```javascript
console.log("Start");
console.log("End");
```

Both `console.log` calls are executed one after the other.

**Synchronous Functions:**

```javascript
function add(a, b) {
  return a + b;
}
let result = add(5, 10);
console.log(result); // 15
```

**Synchronous Loops:**

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

The loop blocks the execution of the following code until it completes.

### Asynchronous APIs

Asynchronous APIs are non-blocking, meaning that they allow the execution of subsequent code while the current operation is still in progress. These APIs rely on callbacks, promises, or async/await to handle operations that take time to complete, such as network requests, file reading, or timers.

**Examples of Asynchronous APIs:**

**setTimeout:**

```javascript
console.log("Start");
setTimeout(() => {
  console.log("Timeout");
}, 1000);
console.log("End");
```

`setTimeout` schedules the callback to run after the specified delay without blocking the code execution.

**Promises:**

```javascript
console.log("Start");
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise resolved");
  }, 1000);
});
promise.then((message) => {
  console.log(message);
});
console.log("End");
```

Promises represent the eventual completion (or failure) of an asynchronous operation and its resulting value.

**Async/Await:**

```javascript
console.log("Start");
async function fetchData() {
  let response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  let data = await response.json();
  console.log(data);
}
fetchData();
console.log("End");
```

`async/await` provides a way to write asynchronous code that looks synchronous, making it easier to read and maintain.

### How Synchronous and Asynchronous APIs Work with the Event Loop

**Synchronous APIs:**
Synchronous APIs run on the main thread, using the call stack. When a synchronous operation is executed, it is pushed onto the call stack. The JavaScript engine processes each function call one at a time, from top to bottom.

**Asynchronous APIs:**
Asynchronous APIs use the event loop to manage operations that can take time to complete. Here's how it works:

1. **Call Stack:** When an asynchronous function is called, it is placed on the call stack.
2. **Event Table:** The asynchronous operation is then moved to the event table (or event registry), where it waits for completion.
3. **Callback Queue:** Once the operation completes, its callback function is placed in the callback queue (or task queue).
4. **Event Loop:** The event loop checks the call stack. If the call stack is empty, it pushes the first callback from the callback queue onto the call stack, executing the callback.

**Examples with Detailed Steps:**

**setTimeout Example:**

```javascript
console.log("Start");
setTimeout(() => {
  console.log("Timeout");
}, 0);
console.log("End");
```

**Execution Flow:**

1. `console.log('Start')` is pushed to the call stack and executed.
2. `setTimeout` is pushed to the call stack. It registers the callback in the event table and is removed from the call stack.
3. `console.log('End')` is pushed to the call stack and executed.
4. The event loop finds the call stack empty, moves the `setTimeout` callback to the callback queue.
5. The event loop pushes the `setTimeout` callback to the call stack and executes it.

**Output:**

```
Start
End
Timeout
```

**Promise Example:**

```javascript
console.log("Start");
Promise.resolve().then(() => {
  console.log("Promise");
});
console.log("End");
```

**Execution Flow:**

1. `console.log('Start')` is pushed to the call stack and executed.
2. `Promise.resolve()` is pushed to the call stack, creating a resolved promise.
3. `.then()` registers the callback in the microtask queue and is removed from the call stack.
4. `console.log('End')` is pushed to the call stack and executed.
5. The event loop finds the call stack empty, moves the microtask (Promise callback) to the call stack.
6. The Promise callback is executed.

**Output:**

```
Start
End
Promise
```

**Async/Await Example:**

```javascript
console.log("Start");
async function fetchData() {
  let response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  let data = await response.json();
  console.log(data);
}
fetchData();
console.log("End");
```

**Execution Flow:**

1. `console.log('Start')` is pushed to the call stack and executed.
2. `fetchData` is called, and the async function is pushed to the call stack.
3. The `await fetch` pauses the execution of `fetchData` and moves the promise to the microtask queue.
4. `console.log('End')` is pushed to the call stack and executed.
5. The event loop finds the call stack empty, moves the resolved promise to the call stack.
6. The `await fetch` resumes, and `response.json()` is called and awaited.
7. The event loop moves the resolved promise to the call stack, and `console.log(data)` is executed.

**Output:**

```
Start
End
{...data...}
```

### Conclusion

**Synchronous APIs:** Block the execution of subsequent code until the current operation completes.
**Asynchronous APIs:** Allow the execution of subsequent code while the current operation is in progress, utilizing the event loop, callback queue, microtask queue, and macrotask queue.
**Event Loop:** Manages the execution order of tasks, ensuring non-blocking operations and efficient handling of asynchronous code.

Understanding the difference between synchronous and asynchronous APIs, and how they interact with the event loop, is crucial for writing efficient, non-blocking JavaScript code. This knowledge helps in designing applications that can handle multiple tasks concurrently, providing a smooth and responsive user experience.

### Identifiers in JavaScript

Identifiers are names assigned by the user to elements such as variables, functions, and objects in a program. These names must follow specific rules and conventions:

- Must start with a letter, underscore (\_), or dollar sign ($).
- The rest of the variable name can include any letter, any number, or the underscore. You can't use any other characters, including spaces, symbols, and punctuation marks.
- Subsequent characters can include letters, digits, underscores, or dollar signs.
- Are case-sensitive (e.g., `myVariable` and `myvariable` are different identifiers).
- As a rule of naming convention, we use camel case for variable names and Pascal case for class or constructor names.

**Examples:**

**Camel Case:**

```javascript
var firstName = "Sparsh";
```

**Pascal Case:**

```javascript
class Student {}
```

**Variable Names:**

```javascript
let myVariable;
const _privateVar = 42;
function calculateSum(a, b) {
  return a + b;
}
var variableName;
var variableName = value;
```

### Keywords in JavaScript

Keywords are reserved words in JavaScript that have special meaning to the JavaScript engine. These words cannot be used as identifiers (i.e., names for variables, functions, or objects).

Below are all the keywords currently supported by the latest JavaScript standards:

- `async`
- `await`
- `break`
- `case`
- `const`
- `continue`
- `debugger`
- `default`
- `delete`
- `do`
- `else`
- `export`
- `extends`
- `finally`
- `for`
- `function`
- `if`
- `import`
- `in`
- `instanceof`
- `let`
- `new`
- `null`
- `return`
- `static`
- `super`
- `switch`
- `this`
- `throw`
- `try`
- `typeof`
- `undefined`
- `var`
- `void`
- `while`
- `yield`
- `Symbol`

**Examples:**

```javascript
if (true) {
  console.log("This is true");
}
let result = 10 + 20;
```

### Memory Storage

Understanding how JavaScript uses memory storage is crucial for writing efficient and bug-free code. JavaScript's memory management involves two primary areas: the stack and the heap. Each serves different purposes and stores different types of data.

**Stack Memory:**

- **Purpose:** Manages function calls and stores primitive data types. Provides fast access for short-lived, simple data.
- **Structure:** Operates in a Last In, First Out (LIFO) manner. Think of it like a stack of plates; you add and remove plates from the top.
- **Characteristics:**
  - Primitive Data Types: Numbers, strings, booleans, null, and undefined are stored directly in the stack.
  - Function Calls: Each function call creates a new stack frame that contains the function’s local variables, parameters, and return address.
  - Execution Context: The stack maintains the execution context, which includes the scope chain,

variable object, and this value.

- **Example:**
  ```javascript
  function multiply(a, b) {
    let result = a * b;
    return result;
  }
  let product = multiply(3, 4);
  console.log(product); // Outputs: 12
  ```

**Heap Memory:**

- **Purpose:** Used for dynamic memory allocation. Stores complex data types like objects, arrays, and functions.
- **Structure:** Unorganized, flexible pool of memory. Think of it like a large, unordered storage space.
- **Characteristics:**
  - Objects and Functions: Complex data structures are stored in the heap.
  - References: Variables in the stack hold references (pointers) to the actual objects in the heap.
  - Garbage Collection: JavaScript's garbage collector periodically scans the heap and removes objects that are no longer referenced.
- **Example:**
  ```javascript
  let person = {
    name: "Sparsh",
    age: 25,
  };
  let anotherPerson = person; // anotherPerson references the same object
  ```

**Interaction Between Stack and Heap:**

- **Example:**
  ```javascript
  function createPerson(name, age) {
    return {
      name: name,
      age: age,
    };
  }
  let person1 = createPerson("Sparsh", 25);
  let person2 = createPerson("John", 30);
  ```

**Garbage Collection:**
JavaScript’s garbage collector automatically frees up memory by removing objects that are no longer referenced. The two primary algorithms are:

- **Mark-and-Sweep:** This algorithm marks all objects that are reachable (directly or indirectly) from the roots (global objects and local variables). Unmarked objects are then swept (deleted).
- **Reference Counting:** Keeps track of the number of references to each object. When an object’s reference count drops to zero, it can be safely deleted. This approach can have issues with circular references.

**Example of Circular Reference:**

```javascript
function createCircularReference() {
  let obj1 = {};
  let obj2 = {};

  obj1.reference = obj2;
  obj2.reference = obj1;
}
createCircularReference();
```

### "use strict" in JavaScript

The `"use strict"` directive is a way to enable strict mode in JavaScript. Introduced in ECMAScript 5 (ES5), strict mode is a way to opt into a restricted variant of JavaScript, which makes it easier to write secure and reliable code.

**How to Enable Strict Mode:**

- **Entire Script:**
  To enable strict mode for the entire script, place `"use strict";` at the top of the script.

  ```javascript
  "use strict";

  function myFunction() {
    // Function code here
  }
  ```

- **Individual Functions:**
  To enable strict mode for a specific function, place `"use strict";` at the beginning of the function.
  ```javascript
  function myFunction() {
    "use strict";
    // Function code here
  }
  ```

**Why Use Strict Mode?**
Strict mode provides several benefits that help developers write better and more secure JavaScript code. Here are the main reasons for using strict mode:

- **Catches Common Coding Errors:** Strict mode catches common mistakes and throws errors to alert developers, making it easier to identify and fix bugs.
- **Prevents Accidental Globals:** In non-strict mode, assigning a value to an undeclared variable automatically creates a global variable. Strict mode prevents this by throwing an error if a variable is used without declaring it.
  ```javascript
  "use strict";
  x = 3.14; // Throws an error because x is not declared
  ```
- **Eliminates Silent Errors:** Certain actions that would fail silently in non-strict mode will throw errors in strict mode, making it clear when something goes wrong.
  ```javascript
  "use strict";
  let obj = Object.freeze({});
  obj.newProp = "test"; // Throws an error
  ```
- **Disallows Duplicate Property Names and Parameter Values:** Strict mode disallows duplicate property names in objects and duplicate parameter names in functions, which can lead to unintended behavior.
  ```javascript
  "use strict";
  let obj = { prop: 1, prop: 2 }; // Throws an error
  function sum(a, a, c) {
    // Throws an error
    return a + a + c;
  }
  ```
- **Improves Performance:** Strict mode enables better optimization by JavaScript engines, as it eliminates certain erratic behaviors, allowing for more predictable and optimized code execution.
- **Prohibits Certain Syntax:** Some syntax that might be used in future versions of ECMAScript is prohibited in strict mode to ensure compatibility with future updates.
  ```javascript
  "use strict";
  let public = 1500; // Throws an error (public is a reserved keyword)
  ```
- **Enhanced Security:** By disabling features that are prone to security issues (like `eval` and `with`), strict mode enhances the security of the code.
  ```javascript
  "use strict";
  eval("var x = 2"); // Still allows, but with limitations in strict mode
  with (Math) {
    x = cos(2);
  } // Throws an error
  ```

**Key Differences in Strict Mode:**

- **Variable Declarations:** Assigning a value to an undeclared variable throws an error.
  ```javascript
  "use strict";
  x = 3.14; // Throws an error
  ```
- **Octal Literals:** Octal literals are not allowed.
  ```javascript
  "use strict";
  let x = 010; // Throws an error
  ```
- **Deleting Undeletable Properties:** Deleting properties that cannot be deleted throws an error.
  ```javascript
  "use strict";
  delete Object.prototype; // Throws an error
  ```
- **this Keyword:** In functions, `this` is `undefined` if the function is not called as a method.
  ```javascript
  "use strict";
  function myFunction() {
    console.log(this); // Outputs undefined
  }
  myFunction();
  ```
- **Function Parameter Names:** Duplicate parameter names are not allowed.
  ```javascript
  "use strict";
  function myFunction(a, a) {
    // Throws an error
    // Code here
  }
  ```
- **with Statement:** The `with` statement is not allowed.
  ```javascript
  "use strict";
  with (Math) {
    // Throws an error
    x = cos(2);
  }
  ```

### With

The `with` statement in JavaScript is used to extend the scope chain for a statement. It is considered bad practice and is not recommended for use in modern JavaScript due to its impact on performance and potential to cause difficult-to-debug code. However, it’s useful to understand how it works for historical purposes.

**Example:**

```javascript
const obj = {
  a: 1,
  b: 2,
  c: 3,
};
with (obj) {
  console.log(a); // 1
  console.log(b); // 2
  console.log(c); // 3
}
```

In this example, the `with` statement is used to create a scope in which the properties of `obj` can be accessed directly without needing to prefix them with `obj`.

**Why Avoid with Statement:**

- **Performance Issues:** The use of the `with` statement can lead to performance degradation as it changes the scope chain and makes it harder for JavaScript engines to optimize the code.
- **Readability and Maintainability:** Code using `with` can be confusing and difficult to understand, as it is not always clear where a variable or property is coming from.
- **Strict Mode:** The `with` statement is disallowed in strict mode (`"use strict"`).

**Alternative Approach:**
Instead of using `with`, you can achieve similar results by directly referencing the object’s properties or using destructuring assignment.

**Direct Reference:**

```javascript
const obj = {
  a: 1,
  b: 2,
  c: 3,
};
console.log(obj.a); // 1
console.log(obj.b); // 2
console.log(obj.c); // 3
```

**Destructuring Assignment:**

```javascript
const obj = {
  a: 1,
  b: 2,
  c: 3,
};
const { a, b, c } = obj;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

### Data Types or Types of Identifiers

The `var` keyword is used to define or declare a variable identifier. Computer memory is in the form of bits (0 or 1), and generally, RAM memory is utilized for programming. When we create a variable with the variableName, we assign a memory address, and the size of memory is dependent on the value being assigned to the variable.

**Rules for Variable Names:**

- The first character must be a letter or an underscore (\_). You can't use a number as the first character.
- The rest of the variable name can include any letter, any number, or the underscore. You can't use any other characters, including spaces, symbols, and punctuation marks.
- Variable names are case-sensitive.
- You can't use one of JavaScript's reserved words as a variable name.

**JavaScript is a loosely typed or weakly typed language:**

- While declaring the variable, we don't define the type of value the variable holds.
- It's also a dynamic language, as the creation of variables at runtime is possible, and the type of variable is determined at runtime or type checking

is done at runtime.

**Types of Identifiers in JavaScript:**

**Reference Types:**

- Stored in heap memory.
- Dynamically stored and considered to be faster. Examples include Object, Array, Function (which is called an object of function).

**Primitive Types:**

- Stored in stack memory and are of fixed size.
- Examples include number, null, string, boolean, undefined, symbol, BigInt.

**Types of Values an Identifier Can Hold in JavaScript:**

**number:**

```javascript
var num = 13; // typeof num -> number
var num = 12.4; // typeof num -> number
var num = -1; // typeof num -> number
var num = NaN; // typeof num -> number
```

**string:**

```javascript
var str = "sparsh"; // typeof str -> string
var str = "s"; // typeof str -> string
```

**boolean:**

```javascript
var flag = true; // typeof flag -> boolean
var flag = false; // typeof flag -> boolean
```

**undefined:**

```javascript
var value; // typeof value -> undefined
```

**object:**

```javascript
var arr = []; // typeof arr -> object
var value = null; // typeof value -> object
var obj = { name: "sparsh" }; // typeof obj -> object
function num() {} // typeof num -> function
```

**Symbol:**

```javascript
var unique = Symbol("id"); // typeof unique -> symbol
```

**Implied globals (obsolete):**
Any variable which is not defined or declared is considered a global variable by default.

```javascript
variableName = value;
```

**Compiler:**
Convert high-level human-readable code into lower-level machine code.

### Transpiler

**“use strict”:**
‘use strict’ enforces stricter parsing and error handling in your code. It helps in failing fast and failing loudly i.e., throwing an error which will lead to failure and problems at a later stage of programming. Also, the JavaScript language is developed in 10 days with lots of errors and features which are difficult to be included to make it more grammatically correct.

- **Prevents the use of global variable:**

  ```javascript
  name = "sparsh";
  console.log(city); // Above code will not work using ‘use strict’

  ("use strict");
  name = "sparsh";
  console.log(city);
  ```

- **Duplicate Parameter Names:**

  ```javascript
  function myFunc(a, a, b) {
    console.log(a, a, b);
  }
  myFunc(1, 2, 3); // Above code will not work using ‘use strict’

  ‘use strict’
  function myFunc(a, a, b) {
    console.log(a, a, b);
  }
  myFunc(1, 2, 3);
  console.log(city);
  ```

### Error Handling

Error in code can occur while executions or runtime, even if our code is free from syntax and compile-time error. JavaScript, like other languages, supports handling such errors.

**Error handling in JavaScript includes three blocks:**

1. **Try block:** Where the code is prone to error/exceptions is written and once the error occurs the catch block will receive the error object.
2. **Catch block:** Receives the error object.
3. **Finally block:** Executes after the `try` and `catch` blocks.

**Example:**

```javascript
try {
  // Code that may throw an error
} catch (error) {
  // Handle the error
} finally {
  // Code that will always execute
}
```

**JavaScript Error capturing flow:** Whenever an error occurs an error block will go to a nearby capturing block and if there is no capturing block then it will go to the browser's onerror block.

### Modular JavaScript [ES6]

**Modular programming:** is a pattern of JavaScript with three formats for writing modular JavaScript code:

1. **AMD**
2. **CommonJS**
3. **Harmony**

**import & export [ES6]:**

To understand the import and export features of modern JavaScript let’s do a small exercise and understand why we need these features. We have implemented the functionality of a basic calculator in filename `calculator.js` as below:

**calculator.js:**

```javascript
export function add(a, b) {
  return a + b;
}
export function subtract(a, b) {
  return a - b;
}
```

**script.js:**

```javascript
import { add } from "./calculator";

console.log(add(4, 5)); // 9
```

**Advantages of Modules:**

- More maintainable code
- Each module is independent of other modules, rare namespace clash won’t
- Easy to track flow as subdivided the code into multiple chunks
- Reusable code encapsulation
- Code readability/organized code is convenient

**Issues of Modules:**

- Global variables (larger global variables)
- Order of script still matters
- Difficult to track Namespace collisions

**ESM:**

- Reusable code encapsulation.
- Code readability/organized code is convenient.
- Issue: Very slow on browsers.

**Export Syntax:**

```javascript
export { functionName, variableName };
```

**Import Syntax:**

```javascript
import { functionName, variableName } from "./fileName";
```

**Default Export:**

```javascript
export default functionName;
```

**Default Import:**

```javascript
import functionName from "./fileName";
```

### Additional Modern JavaScript Features [ES6]

**Spread:**
Allows an iterable such as an array to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected.

```javascript
let numbers = [1, 2, 3];
let moreNumbers = [...numbers, 4, 5];
console.log(moreNumbers); // [1, 2, 3, 4, 5]
```

**Rest:**
Allows us to represent an indefinite number of arguments as an array.

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, curr) => acc + curr, 0);
}
console.log(sum(1, 2, 3)); // 6
```

**Destructuring:**
A JavaScript expression that allows us to extract data from arrays or objects into distinct variables.

```javascript
let person = { name: "Sparsh", age: 25 };
let { name, age } = person;
console.log(name); // Sparsh
console.log(age); // 25
```

**Template Literals:**
Template literals allow for embedded expressions. You can use multi-line strings and string interpolation features with them.

```javascript
let name = "Sparsh";
let greeting = `Hello, my name is ${name}.`;
console.log(greeting); // Hello, my name is Sparsh.
```

### Asynchronous Patterns

**Callback:**
A function passed as an argument to another function.

```javascript
function greet(name, callback) {
  console.log("Hello " + name);
  callback();
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet("Sparsh", sayGoodbye);
```

**Promises:**
Promises are used to handle asynchronous operations. They are an improvement over callback functions by making the code easier to read and maintain.

```javascript
const promise = new Promise((resolve, reject) => {
  let success = true;
  if (success) {
    resolve("Success!");
  } else {
    reject("Failure!");
  }
});

promise
  .then((message) => {
    console.log(message);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Async/Await:**
Syntax to work with promises in a more elegant way.

```javascript
async function fetchData() {
  try {
    let response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.log(error);
  }
}

fetchData();
```

**Generators:**
Generators are functions that can be paused and resumed. They are useful for handling asynchronous operations.

```javascript
function* generatorFunction() {
  yield "Hello";
  yield "World";
}

const generator = generatorFunction();
console.log(generator.next().value); // Hello
console.log(generator.next().value); // World
```

### Flow Control

**if...else:**

```javascript
if (condition) {
  // block of code to be executed if the condition is true
} else {
  // block of code to be executed if the condition is false
}
```

**switch:**

```javascript
switch (expression) {
  case value1:
    // code block
    break;
  case value2:
    // code block
    break;
  default:
  // code block
}
```

**Short-Circuiting:**

- **Logical AND (&&):** If any statement is false, it skips the checking of the following statement and results in false.
- **Logical OR (||):** If any statement is true, it skips the checking of the following statement and results in true.

**Equals vs. Strict Equals (== vs. ===):**

- **Equals (==):** Tests for abstract equality (type conversion occurs).
- **Strict Equals (===):** Tests for strict equality (no type conversion).

**Examples:**

```javascript
console.log(null == undefined); // true
console.log(null === undefined); // false
console.log(-0 == +0); // true
console.log(-0 === +0); // true
console.log(NaN === NaN); // false
console.log(Object.is(NaN, NaN)); // true
```

### Creating Objects

**Object Constructor and `new` Operator:**

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
let person = new Person("Sparsh", 25);
```

**Object Literals:**

```javascript
let person = {
  name: "Sparsh",
  age: 25,
};
```

**Classes (ES6):**

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
let person = new Person("Sparsh", 25);
```

**Prototypes:**

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.greet = function () {
  console.log("Hello, my name is " + this.name);
};
let person = new Person("Sparsh", 25);
person.greet(); // Hello, my name is Sparsh
```

### Inheritance

**Class Inheritance (ES6):**

```javascript
class Employee extends Person {
  constructor(name, age, empId) {
    super(name, age);
    this.empId = empId;
  }
}
let employee = new Employee("Sparsh", 25, "E123");
```

**Prototypal Inheritance:**

```javascript
let person = {
  greet() {
    console.log("Hello, my name is " + this.name);
  },
};
let employee = Object.create(person);
employee.name = "Sparsh";
employee.greet(); // Hello, my name is Sparsh
```

**Private Methods and Properties:**

```javascript
class Person {
  #name;
  constructor(name, age) {
    this.#name = name;
    this.age = age;
  }
  getName() {
    return this.#name;
  }
}
let person = new Person("Sparsh", 25);
console.log(person.getName()); // Sparsh
console.log(person.#name); // SyntaxError: Private field '#name' must be declared in an enclosing class
```

### `this` Keyword

The `this` keyword refers to the object that the function is a property of.

**Example:**

```javascript
let person = {
  name: "Sparsh",
  greet: function () {
    console.log("Hello, my name is " + this.name);
  },
};
person.greet(); // Hello, my name is Sparsh
```

**Using `bind`, `call`, and `apply`:**

```javascript
let person = {
  name: "Sparsh",
};
let greet = function (greeting) {
  console.log(greeting + ", my name is " + this.name);
};

let boundGreet = greet.bind(person);
boundGreet("Hello"); // Hello, my name is Sparsh

greet.call(person, "Hi"); // Hi, my name is Sparsh
greet.apply(person, ["Hey"]); // Hey, my name is Sparsh
```

### Closures

A closure is the combination of a function and the lexical environment within which that function was declared.

**Example:**

```javascript
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log("Outer Variable: " + outerVariable);
    console.log("Inner Variable: " + innerVariable);
  };
}
const newFunction = outerFunction("outside");
newFunction("inside");
// Outer Variable: outside
// Inner Variable: inside
```

**IIFE (Immediately Invoked Function Expression):**

```javascript
(function () {
  console.log("This is an IIFE");
})();
```

### Recursion

A function that calls itself is known as a recursive function.

**Example:**

```javascript
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}
console.log(factorial(5)); // 120
```

### Memoization

Memoization is a technique to speed up the computation by storing the results of expensive function calls.

**Example:**

```javascript
function memoize(fn) {
  const cache = {};
  return function (...args) {
    const key = args.toString();
    if (cache[key]) {
      return cache[key];
    }
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const factorial = memoize(function (n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
});
console.log(factorial(5)); // 120
console.log(factorial(5)); // 120 (cached)
```

### Hoisting

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

**Example:**

```javascript
console.log(myVar); // undefined
var myVar = 5;

hoistedFunction(); // "This function is hoisted"
function hoistedFunction() {
  console.log("This function is hoisted");
}
```

### Arrays

Arrays are a special type of object in JavaScript that can hold collections of values.

**Creating an Array:**

```javascript
let array = [1, 2, 3];
```

**Array Methods:**

- `push`: Adds one or more elements to the end of an array.
- `pop`: Removes the last element from an array.
- `shift`: Removes the first element from an array.
- `unshift`: Adds one or more elements to the beginning of an array.
- `forEach`: Executes a provided function once for each array element.
- `map`: Creates a new array with the results of calling a provided function on every element in the calling array.
- `filter`: Creates a new array with all elements that pass the test implemented by the provided function.
- `reduce`: Executes a reducer function (that you provide) on each element of the array, resulting in a single output value.

**Example:**

```javascript
let numbers = [1, 2, 3, 4, 5];
let doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
```

### Type Casting and Coercion

**Type Coercion:**
Implicit type conversion happens when a value is automatically converted to another type.

**Example:**

```javascript
console.log("5" + 5); // "55"
console.log("5" - 5); // 0
```

**Type Casting:**
Explicit type conversion happens when you manually convert a value to another type.

**Example:**

```javascript
let num = "123";
let number = Number(num);
console.log(number); // 123
```

### Event Loop

The event loop is what allows JavaScript to perform non-blocking operations by offloading operations to the system kernel whenever possible.

**Stack:**
A stack is an ordered collection of items where the addition and removal of items happens at one end (LIFO).

**Queue:**
A queue is an ordered collection of items where the addition of new items happens at one end (rear) and the removal of existing items occurs at the other end (front) (FIFO).

**Heap:**
A heap is a region of memory where memory is allocated and deallocated in an arbitrary order.

**Event Loop:**
The event loop has one simple job: to monitor the call stack and the callback queue. If the call stack is empty, it will take the first event from the queue and push it to the call stack, which effectively runs it. Each event is just a function callback.

**Example:**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

console.log("End");
```

**Output:**

```
Start
End
Timeout
```
