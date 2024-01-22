
```javascript
/* Functional Programming (FP) */

/** FP is the process of building software by composing pure functions, avoiding shared state, mutable data, and side-effects */

/** FP is often declarative rather than imperative, and application state flows through pure functions. */

/** What FP avoids
 * - looping
 * - mutation and shared state
 * - variable declaration
 */
 ```
 
```javascript
// Imperative Programming

let imperativeSum = 0;
for (let i = 1; i <= 5; i++) {
  imperativeSum += i;
}

// Functional Programming
let functionalSum = [1, 2, 3, 4, 5].reduce((acc, val) => acc + val, 0);

// First Class Functions
// javascript treats functions like any other value;

const myFunction = function (person) {
  console.log(`Hello there, ${person}`);
};
```

```javascript
// Writing Pure Functions

/** Referential transparency
 * - The function always gives the same return value for the same arguments
 * - The function cannot depend on any mutable state
 *
 * Side-effect free
 * - The function cannot cause any side effects
 * - Side effects may include I/O (eg, writing to the console or a log file),
 * modifying a mutable object, reassigning a varaible, etc.
 */

// Impure Function
let value = 2;
function squareAndUpdateValue(num) {
  value = num * num;
  return value;
}

// Pure Function
function square(num) {
  return num * num;
}

// Impure Function
const colors = ["red", "orange"];
function addToArray(arr, value) {
  return arr.push(value);
}

// Pure Function
function pureAddToArray(arr, value) {
  return [...arr, value];
}
```

```javascript
// Higher Order Functions
/**
 * A function that recieves another function as an argument, returns a function, or does both
 */

function doTwice(func) {
  func();
  func();
}

doTwice(function () {
  console.log("HELLO WORLD!");
});

// Returning Functions
function multiplyBy(factor) {
  return function (number) {
    return number * factor;
  };
}
const triple = multiplyBy(3);
const double = multiplyBy(2);

// Immutability
const person = { name: "Teddy", age: 2 };
// makes an object immutable
Object.freeze(person);

const nums = [1, 2, 3];

// Immutable Appraoches
function push(arr, val) {
  return [...arr, val];
}

function removeLastItem(arr) {
  return arr.slice(0, -1);
}

function addFieldToObject(obj, key, value) {
  return { ...obj, key: value };
}
```

```javascript
// Recursion
function factorial(n) {
  let result = 1;
  for (let i = n; i > 1; i--) {
    result *= i;
  }
  return result;
}

function recursiveFactorial(n) {
  if (n < 0) throw new Error("Invalid Argument");
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}
```

```javascript
/** Partial Application */
/** With Bind */

/**
 * The process of executing a function with some or all of its arguments.
 * The partially applied function then gets returned for later user.
 *
 */

function greet(greeting, name) {
  console.log(`${greeting}, ${name}!!!`);
}

// We can bake in arguments with bind
const spitefulGreet = greet.bind(null, "I hate you");

/** Partial Application */
/** Writing a Partial Function */
function multiply(a, b) {
  return a * b;
}

function partial(func, ...fixedArgs) {
  return function (...remainingArgs) {
    return func(...fixedArgs, ...remainingArgs);
  };
}

function fetchData(url, apiKey, params) {
  const queryString = new URLSearchParams(params).toString();
  const fullUrl = `${url}?${queryString}`;

  console.log(`Sending request to ${fullUrl}`);
  console.log(`With API key of ${apiKey}`);
}
const myApiKey = "my-secret-api-key";
const myApiUrl = "https://api.mywebsite.com/data";

const fetchMyAPI = partial(fetchData, myApiUrl, myApiKey);
fetchMyAPI({ id: 1, sort: "desc" });
//Sending request to https://api.mywebsite.com/data?id=1&sort=desc
//With API key of my-secret-api-key
```

```javascript
/** Composition Basics */
/**
 * Function composition is a mechanism of combining multiple functions to build a more complicated one
 * The result of each function is passed to the next one.
 *
 * f(g(x))
 */

const add = (a, b) => a + b;
const multiply = (a, b) => a * b;
add(10, multiply(10, 20)); // 210

const addAndMultiply = (a, b, c) => add(multiply(a, b), c);
addAndMultiply(10, 20, 10); // 210

// compose function
function compose(fn1, fun2) {
  return function (value) {
    return fun2(fn1(value));
  };
}

// fancier compose
function generalCompose(...functions) {
  return function (data) {
    return functions.reduceRight((val, func) => func(val), data);
  };
}

function lowerCaseString(str) {
  console.log(str);
  return str.toLowerCase();
}

function splitWords(str) {
  return str.split(" ");
}

function joinWithDash(array) {
  return array.join("-");
}

const sluggify = generalCompose(joinWithDash, splitWords, lowerCaseString);
// mangoes-can-fight-the-world
```

```javascript
/**
 * Currying
 */
// a curried function can be called with any number of arguments
// if you call it with fewer arguments than it takes, it returns
// a "smaller" partial, which you can then call with remaining
// arguments

function add(a, b, c) {
  return a + b + c;
}

function addCurry(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}

/** Advanced Currying */
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function (...args2) {
        return curried.apply(this, args.concat(args2));
      };
    }
  };
}

const curriedAdd = curry(add);
```