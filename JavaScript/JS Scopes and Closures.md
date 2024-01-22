```javascript
// ## Scope and Closure
// ==================================================

/* var */

// if we declare a var variable outside, it is available globally
var color = "red";

// if we declare a var variable in a function, it is scoped to the function
function blah() {
  var animal = "Flamboyant Cuttlefish";
}

// if we declare a var variable in a code block, it is available globally
if (true) {
  var food = "Chicken Parmesan";
}

console.log(color); // "red"
console.log(food); // "Chicken Parmesan"
console.log(animal); // undefined

for (var i = 0; i < 10; i++) {
  console.log(i);
}

console.log("AFTER THE LOOP IS OVER!", i); // 10

// problems with var
var origin = "Ethiopia";

// now we have just overridden the origin property on window
console.log(window.origin); // "Ethiopia"

/* let and const */

/**
 * let origin = "Ethiopia" does not attach to window object
 * window.origin // undefined
 *
 * const city = "Lisbon" // cannot be redeclared
 */

/**
 * let and const are scoped to functions
 * and code blocks, besides object literals
 */
```

```javascript
// ==================================================
// The Scope Chain
// ==================================================
/** how javascript looks for value of variable
 * 1. in the local scope
 * 2. any outer functions
 * 3. in the global scope
 */

let age = 10;

function outer() {
  let age = "ageless";
  function inner() {
    age = "Eternal";
    console.log(age); // Eternal
  }
  inner();
  console.log(age); // Eternal
}
outer();
console.log(age); // 10

function outer() {
  let age = "ageless";
  function inner() {
    function superInner() {
      console.log(age); // "ageless"
    }
    superInner();
  }
  inner();
}
outer();
```

```javascript
// ==================================================
// Static Scope (Lexical Scoping)
// ==================================================
let animal = "Barn Owl";

function printAnimal() {
  console.log(animal);
}

function alsoPrintAnimal() {
  let animal = "Burrowing Owl";
  printAnimal();
}

alsoPrintAnimal(); // "Barn Owl"
```

```javascript
// ==================================================
// Hoisting
// ==================================================
/** var variable declarations are pulled to the top of their function
 * so the variable is legal, but equal to undefined until definition
 */

console.log(food); // undefined
var food = "pizza";

/** var food = undefined;
 *  console.log(food)
 *  food = "pizza"
 */

console.log(color); // reference error
let color = "black";

/**
 *  let color = UNACCESSIBLE
 *  // TEMPORAL DEAD ZONE STARTS
 *  console.log(color);
 *  // ReferenceErrror: Cannot access 'color'
 *  before initialization
 *  let color = "black"
 *  // TEMPORAL DEAD ZONE ENDS
 */

// technically let and const are hoisted as well but javascript
// does not allow us to do anything with it until we reach the original
// line where we declared it

// leads to unexpected behavior
function blah() {
  if (false) {
    var message = "Hello";
  }
  console.log(message); // undefined
}

blah();
```

```javascript
// ==================================================
// IIFE (immediately invoked function expression)
// ==================================================
(function () {
  var origin = "Brazil";
  console.log("HELLO FROM AN IIFE!!!");
  console.log("Origin is: ", origin); // Brazil
})();
console.log(window.origin); // "http://127.0.0.1:5500"
```

```javascript
// ==================================================
// Closures
// ==================================================
function outerFunction() {
  let outerVariable = "I am from outer!";
  function innerFunction() {
    console.log("I AM THE INNER FUNCTION");
    console.log("outerVariable is", outerVariable);
  }
  return innerFunction;
}

const myClosure = outerFunction();
myClosure();
// I AM THE INNER FUNCTION
// outerVariable is I am from outer!

function idGenerator() {
  let count = 1;
  return function generate() {
    return count++;
  };
}

const nextId = idGenerator();
// has access to counter without polluting the global space

// * private variables *
function createCounter() {
  let count = 0;
  return {
    increment: function () {
      return count++;
    },
    decrement: function () {
      return count--;
    },
    getCount: function () {
      return count;
    },
  };
}
const counter = createCounter();
// count cannot be accessed directly
// could also be written as an IIFE
```

```javascript
// ==================================================
// Closures: Factory Functions
// ==================================================
function createExponentFunction(exponent) {
  return function (val) {
    return val ** exponent;
  };
}

const square = createExponentFunction(2);
const cube = createExponentFunction(3);
square(3); // 9
cube(3); // 27

function uniqueIdGenerator(prefix) {
  let id = 0;
  return function () {
    id += 1;
    return `${prefix}${id}`;
  };
}

const getBookId = uniqueIdGenerator("book-");
const getUserId = uniqueIdGenerator("user_");
getBookId(); // book-1
getUserId(); // user_1
```

```javascript
// ==================================================
// Closures: Event Listeners
// ==================================================
document.querySelector("#btn1").addEventListener(
  "click",
  (function () {
    let count = 0;
    return function () {
      console.log(`You clicked me ${++count} time(s)`);
    };
  })()
);

// the beauty of closures
function createCounterButton(id) {
  const btn = document.getElementById(id);
  let count = 0;
  btn.addEventListener("click", function () {
    btn.innerText = `Clicked ${++count} time(s)`;
  });
}

createCounterButton("btn1");
createCounterButton("btn2");
createCounterButton("btn3");
```

```javascript
// ==================================================
// Closures: Loops
// ==================================================
// for (var i = 1; i < 6; i++) {
//   (function (j) {
//     setTimeout(function () {
//       console.log(j);
//     }, 1000 * j);
//   })(i);
// }

for (let i = 1; i < 6; i++) {
  setTimeout(function () {
    console.log(i);
  }, 1000 * i);
}

```