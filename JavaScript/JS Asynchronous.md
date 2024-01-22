# Callbacks
```javascript
// A function passed to another function, for it to call
// Separates processing patterns from business logic
// Register a function to be called when an event happens

// - Functional programming patterns
// - Event-driven programming

// ## Asynchronous Code

// Call those callbacks when async operation completes

// JS is single-threaded
// Only one bit of code can run at once
```

# Promises
```javascript
// A promise is a one-time guarantee of future value
// Promises in JavaScript are objects
// They are native to the language as of ES2015
// A promise can be in one of three states:
// (1) Pending - It doesn't yet have a value
// (2) Resolved - It has successfully obtained a value
// (3) Rejected - It failed to obtain a value for some reason

// The only way to access the resolved or rejected value is
// to chain a method on the end of the promise (or await it)

// .then and .catch (both accept callbacks)

// ## Promise Chaining (Flatten Code)

// When calling .then on a promise
// We can return a new promise in a callback
// Can chain async operations together with .then calls
// Only need one .catch at the end
const BASE_URL = "https://pokeapi.co/api/v2/pokemon";
// ... (code omitted for brevity)

// ## async / await

// ### async

// Async function always returns promises
// In async functions, you can write code that
// Looks synchronous (but it doesn't block JavaScript)

// ### await

// Inside an async function, we can use await
// Await waits for promise to resolve & evaluates
// It to its resolved value
// Think of the 'await' keyword like a pause button

// Modern approach but important to know that async/await
// Is built off of promises (syntactic sugar)

// ## Promise.all()

// Promise.all is a helper function that takes in an
// Array of promises
// ... (code omitted for brevity)

// ## Promise.allSettled()

// Promise.allSettled() also takes in an array of promises
// ... (code omitted for brevity)

// ## Promise.race()

// Promise.race() also takes in an array of promises
// ... (code omitted for brevity)

// ## Building Own Promises

// You can use Promise with the new keyword
// To make your own promises
// ... (code omitted for brevity)

// ## Promisifying Node's fs.readFile()

// ... (code omitted for brevity)

// ## Callback Hell vs Promisified fs vs Async/Await

// ... (code omitted for brevity)

// ## Async/Await

// ... (code omitted for brevity)

// Async function to get haikus
async function getHaikus() {
  try {
    const haiku1 = await asyncReadFile("haiku1.txt");
    console.log("HAIKU 1");
    console.log(haiku1);
    const haiku2 = await asyncReadFile("haiku2.txt");
    console.log("HAIKU 2");
    console.log(haiku2);
    const haiku3 = await asyncReadFile("haiku3.txt");
    console.log("HAIKU 3");
    console.log(haiku3);
  } catch (e) {
    console.log("ERR", e);
  }
}

getHaikus();
```