
# Optional Chaining
```javascript
/* Optional chaining allows reading the value
of a property located deep within a chain of connected objects without having to check each reference in the chain. */
const user = {
  name: "Alice",
  address: {
    city: "Wonderland",
  };

// Traditional way: Check each property in the chain
const city = user && user.address && user.address.city;

// With Optional Chaining
const optCity = user?.address?.city; // Outputs: 'Wonderland'

// Also Applies To Methods
user.greet?.(); // Outputs: undefined, no error thrown
```

# Nullish Coalescing
```javascript
// ==================================================
/* The nullish coalescing operator (??) is a logical operator that returns its right-hand operand when its left-hand operand is null or undefined, and otherwise returns its left-hand operand. This is the way to handle default values more predictably than using the OR (||) operator. */

const age = user?.age ?? "IDK THE AGE";
// Outputs: "IDK THE AGE"

// the problem with (||)
// const age = user?.age || "IDK THE AGE";
// the right-hand side of || will be evaluated as long as the left-hand side
// is falsey. however, sometimes the value we are looking for is a falsey value
// so using ?? is more precise because it looks for null or undefined not
// just any falsey value.

// see 0 || "IDK THE AGE" vs 0 ?? "IDK THE AGE"

// ## Numeric Separators
// ==================================================
/* Numeric separators enhance readability by allowing underscores (_) to
be inserted between digits in numeric literals. */

// Long numeric values can be hard to parse at a glance
const withoutSeparator = 1000000000;

// Using the numeric separator for better readability
const withSeparator = 1_000_000_000;

// ## Array.prototype.at()
// ==================================================
const colors = ["red", "orange", "yellow", "green"];
colors[0]; // "red"
colors.at(0); // "red"

colors[-1]; // undefined
// .at() supports negative indexing
colors.at(-1); // "green"

// ## String.prototype.replaceAll()
// ==================================================
const str =
  "I really love cats.  My cat is such an amazing pet.  She loves to cuddle with me and play. What a great cat. cat. cat. cat.";

// with regular expressions
str.replace(new RegExp("cat", "g"), "dog");

// with replace all
str.replaceAll("cat", "dog");

const strWithCaps =
  "I really love cats.  My cat is such an amazing pet.  She loves to cuddle with me and play. What a great cAt. cat. CAT. cat.";

// with replace all and regular expressions
str.replaceAll(new RegExp("cat", "gi"), "dog");
```

# Logical Assignment
```javascript
/* Logical assignments are a combination of logical operators 
(&&, ||, or ??) and assignment expressions. */

// Logical OR Assignment ||=

const todo = { priority: "LOW", task: "Finish Editing Course" };

// Is priority falsy? If so, set it to "MEDIUM"
todo.priority ||= "MEDIUM";

// Logically equivalent to:
// todo.priority || (todo.priority = "MEDIUM");

// Logical AND Assignment &&=

let a = 0;
let b = 2;

// AND assignment (a &&= b) is equivalent to a && (a = b)
a &&= b;
console.log(a); // Outputs: 0

a = 1;
a &&= b;
console.log(a); // Outputs: 2

// Example 2:
const loggedInUser = { name: "taco" };

// with Logical AND Assignment
loggedInUser &&= { ...loggedInUser, colorPreference: "purple " };

// Nullish Coalescing Assignment ??=
// ==================================================

function doSomeSetup(options = {}) {
  options.timeout ??= 3000; // default timeout of 3 seconds
  options.retries ??= 5; // default of 5 retries
  console.log(options);
}

// ## Promise.any
// ==================================================

/* Promise.any takes an iterable of Promise objects and returns a promise
that is fulfilled by the first given promise to be fulfilled, or rejected
if all of the given promises are rejected. */
const BASE_URL = "https://pokeapi.co/api/v2/pokemon";

Promise.any([
  fetch(`http://nope.nope`),
  fetch(`http://nope.nope`),
  fetch(`${BASE_URL}/2`),
])
  .then((firstToFinish) =>
    console.log("THIS IS THE FIRST RESOLVED VALUE!", firstToFinish)
  )
  .catch((e) => {
    console.log("OH NO, this means ALL promises were rejected", e);
  });
// will be fulfilled since at least one promise is fulfilled

// vs Promise.race
// will be fulfilled or reject based on which Promise
// resolves or rejects first (SOME vs ALL)


```