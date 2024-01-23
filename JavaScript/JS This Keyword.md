// ## 'this'

```javascript
class Cat {
  constructor(firstName) {
    this.firstName = firstName;
  }

  dance(style = "tango") {
    return `Meow, I am ${this.firstName}` + ` and I like to ${style}`;
  }
}

/** Makes sense **/
let fluffy = new Cat("Fluffy");
fluffy.firstName; // "Fluffy"
fluffy.dance("tango"); // works!

/** Gives an error **/
// Loses the 'this' context
// let fDance = fluffy.dance;
// fDance("salsa"); // error?!

// JavaScript "Functions"
// In a sense, JavaScript doesn't have functions
// Everything is called on something, like a method

// ## Global Object
// When you call a function on nothing
// You call it on the "global object"
// In browser JS, that's typically window (the browser window)
// In NodeJS, that's global (where some Node utilities are)

// In General (not always)
// The value of 'this' is left of the dot
// window.whatIsThis() ==> 'this' is window

/* .call */

// Sometimes, you'll need to say
// "call this function on this object"
let fDance = fluffy.dance;
// Call on fluffy, passing "tango" as an arg
fDance.call(fluffy, "tango");

/* .apply */

// Similar to apply but takes in an
// array or array-like argument
ringo.greet.call(george, "Heyo", "!!!");
// George says Heyo!!!

ringo.greet.apply(george, ["Heyo", "!!!"]);
// George says Heyo!!!

/* .bind */
// bind() is a method on functions that
// returns a bound copy of the function

// You can "perma-bind" a function
// to a context
fDance("tango"); // error -- 'this' isn't a cat
fDance.call(fluffy, "tango"); // ok but tedious to always do
let betterDance = fDance.bind(fluffy);
betterDance("tango"); // ok -- bound so that 'this' is Fluffy

// ## Binding Arguments
// You can also bind arguments to functions
function applySalesTax(taxRate, price) {
  return price + price * taxRate;
}

// "null" here means it doesn't matter what 'this' is
const applyCASalesTax = applySalesTax.bind(null, 0.0725);
// New function made where the tax rate is binded to 0.0725
applyCASalesTax(50); // 53.63

// Variations with arguments "baked-in"
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);
const triple = multiply.bind(null, 3);
const alwaysNine = multiply.bind(null, 3, 3);

// ## Binding event listeners
const conan = {
  name: "Conan",
  city: "Los Angeles",
  sing: function () {
    console.log("THIS is: ", this);
    console.log(`${this.name} sings LA LA LA`);
  },
};

const btn = document.querySelector("#clickMe");
btn.addEventListener("click", conan.sing.bind(conan));

// ## Binding timers
class Counter {
  constructor(startingNum = 0, incrementAmt = 1) {
    this.count = startingNum;
    this.incrementAmt = incrementAmt;
  }

  start() {
    setInterval(this.incrementAndPrint.bind(this), 1000);
  }

  incrementAndPrint() {
    console.log(this.count);
    this.count += this.incrementAmt;
  }
}

// ## Arrow Functions and 'this'
// Arrow functions don't make their own this
class Cat {
  constructor(firstName) {
    this.firstName = firstName;
  }

  superGreet() {
    console.log(`#1: I am ${this.firstName}`); // works

    setTimeout(function () {
      console.log("THIS IS: ", this);
      console.log(`#2 I am ${this.firstName}`); // uh oh
    }, 500);

    setTimeout(() => {
      console.log("THIS IS: ", this);
      console.log(`#3 I am ${this.firstName}`); // yay!
    }, 1000);
  }
}

let kitty = new Cat("Kitty");
kitty.superGreet();
```

# Key Takeaways
- 'this' is a reserved keyword whose value is determined only at the point of function execution.
- If you don't call a function yourself, you need to ensure JS knows what the 'this' context should be.
