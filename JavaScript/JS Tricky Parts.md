# Working with float imprecision
```javascript
// 0.1 + 0.2 = 0.30000000000000004
console.log(0.1 + 0.2 == 0.3); // FALSE

// almost equivalent function
function areFloatsEqual(a, b, epsilon = 1e-10) {
  return Math.abs(a - b) < epsilon;
}

console.log(areFloatsEqual(0.1 + 0.2, 0.3)); // TRUE

// Other Recommendations:
// use a different language
// or look at libraries
```

# Numbers (BigInt() and Really Large Numbers)
```javascript
BigInt("4234234234235235115");
const bigBoy = 235463646262622626126n;
console.log(typeof bigBoy); // 'bigint'

// bigints can do regular arithmetic but only
// with other bigints (no support for floats)
```

# Not a Number (isNaN() vs Number.isNaN())
```javascript
// All NaN values are considered unique
console.log(NaN === NaN); // FALSE

// two ways to check for NaN

/* First Way */
// isNaN(n)
// returns true if n is NaN or is a value
// that cannot be coerced into a number

// isNaN(0/0) => true
// isNaN("number") => true

/* Second Way */
// Number.isNaN(n)

// returns true only if n is NaN
// everything else returns false

// Number.isNaN(0/0) => true
// Number.isNaN("number") => false
```
# Post and Pre Increment: ++x vs. x++
```javascript
/* also applies to decrement operators --x vs. x-- */

let x1 = 5;
let y1 = ++x1; // add 1 to x then evaluate: y and x are both 6

let x2 = 5;
let y2 = x2++; // evaluate x and set y to that, then add 1 to x

// Changing after returning
// using x++ is often useful as a return statement ----
// return this value to the caller, but bump up for next time:
class Counter {
  current = 1;

  next() {
    return this.current++;
  }
}
```

# Automatic Semicolon Insertion
```javascript
/* You can write JavaScript with or without semicolons;
if you omit them, they will be inserted by JS in a process
known as ASI, automatic semicolon insertion. */

/*
function blah() {
    return[;]
    {
        name: "chickenface"[;]
    }
}
*/
// blah() returns undefined because of where
// JavaScript automatically inserts semicolons [;]

// WARNING: watch out for line break errors
```

# JavaScript Generator Functions
```javascript
/* JavaScript can have generator functions -- functions
that return a Generator that can be lazily looped over: */
function* evens(n) {
  while (true) {
    yield n;
    n += 2;
  }
}

// make a "Generator"
// will return even numbers 2+
let allEvens = evens(2);

// lazily get the first 10 even numbers
for (let i = 0; i < 10; i++) {
  console.log(allEvens.next().value);
}

// fibonacci
function* fibonacci() {
  let a = 0,
    b = 1;

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

const fibGen = fibonacci();

for (let i = 0; i < 10; i++) {
  console.log(fibGen.next().value);
}

// POTENTIALS: build an array

// ## When Are Generators Useful?
// ==================================================
const allImages = Array.from(
  { length: 1000 },
  (_, i) => `https://placeimg.com/640/480/any?image=${i}`
);

// gives 10 images at a time
function* getBatchOfImages(images, batchSize = 10) {
  let currIndex = 0;
  while (currIndex < images.length) {
    yield images.slice(currIndex, currIndex + batchSize);
    currIndex += batchSize;
  }
}

const imageGen = getBatchOfImages(allImages);
```

# `Array.from()` method
```javascript
// Array.from() generates new arrays from an initial non-array

Array.from("hello"); // ['h', 'e', 'l', 'l', 'o']
Array.from([1, 2, 3]); // [1, 2, 3]
Array.from(new Set([1, 2, 3, 4])); // [1, 2, 3, 4]

// with DOM Node Lists

const buttons = document.querySelectorAll("button");
buttons; // NodeList(5) [button, button, button, button, button]
Array.from(buttons); // [button, button, button, button, button]
// now have access to array methods

// Optional Second Argument, Mapping Function
Array.from("abcd", (x) => x.toUpperCase()); // ["A", "B", "C", "D"]

Array.from(new Set([1, 2, 3, 4]), (n) => n + 11); // [2, 3, 4, 5]

// Generate Arrays of a specific Length
Array.from({ length: 3 }); // [undefined, undefined, undefined]
Array.from({ length: 3 }, (el) => true); // [true, true, true]

// Generate Large Arrays
Array.from({ length: 100 }, (el) => "lol"); // ["lol", ..., "lol"]

// Generate a Sequence
Array.from({ length: 100 }, (el, idx) => idx + 1); // [1, 2,..., 100]
```