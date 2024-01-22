# `new` keyword
```javascript
// ## 'new' keyword

/**
 * 1. Creates an empty object
 * 2. Sets the keyword 'this' to be that object
 * 3. Returns the object - return this
 * 4. Creates a link to the object's prototype
 */

// ## Shared Functions
class DogClass {
  constructor(name) {
    this.name = name;
  }

  bark() {
    return `${this.name} says woof!`;
  }
}

const elton1 = new DogClass("Elton");
const wyatt1 = new DogClass("Wyatt");
/** Same reference in memory */
console.log(elton1.bark === wyatt1.bark); // TRUE

function DogFunction(name) {
  this.name = name;
  this.bark = function () {
    return `${this.name} says woof!`;
  };
}

const elton2 = new DogFunction("Elton");
const wyatt2 = new DogFunction("Wyatt");
console.log(elton2.bark === wyatt2.bark); // FALSE
```

# Prototypes in JavaScript
```javascript
// Every object in JavaScript has a special
// built-in property called its prototype

/** Example: Arrays */
const nums = [1, 2, 3];
// nums will just log [1, 2, 3]
// but we know arrays have lots of methods
// these live on the prototype
// nums.__proto__ ==> ..., .map, .push, etc.

// How to approach prototypes
/** Instead of */
function DogFunctionA(name) {
  this.name = name;
  this.bark = function () {
    return `${this.name} says woof!`;
  };
}

/** We should do this */
function DogFunctionB(name) {
  this.name = name;
}

DogFunctionB.prototype.bark = function () {
  return `${this.name} says woof!`;
};

// ## Review: Objects/Functions in JS

// Every function has a property on it called prototype
// The prototype object has a property called constructor
// which points back to the function
const elton_org = new DogFunctionB("Elton");
const elton_from_org = new elton_org.constructor("Elton Clone");

// When the 'new' keyword is used to invoke a function
// a link between the object created from new and the
// prototype object is established

/**
 * Classes are syntactic sugar on top of
 * constructor functions and prototypes
 */

// ## Inheritance
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
    return `${this.name} says woof!`;
  }
}

class GuideDog extends Dog {
  constructor(name, breed, owner) {
    super(name, breed);
    this.owner = owner;
  }

  alert() {
    return `${this.name} alerts you to danger. Good dog!`;
  }
}

// Via the prototype, GuideDog can access
// .alert() and .bark()

// ## Useful Prototype Methods

const person = {
  sing() {
    return "LALALA";
  },
  isHuman: true,
};

// Object.create(<OBJ_TO_BE_PROTO>)
// Manually sets the prototype
const tom = Object.create(person);
// { [[Prototype]]: person }

// Object.getPrototypeOf(<OBJ_TO_GET_PROTO_OF>)
// Gets the prototype
Object.getPrototypeOf(tom);
// { isHuman: true, sing: f }

// Object.setPrototypeOf(<OBJ_TO_SET_PROTO_OF>, <NEW_PROTO>)
// Sets a new prototype
Object.setPrototypeOf(tom, { isHuman: false });

// Check if it is a prototype of another object
const ling = Object.create(person);
person.isPrototypeOf(ling); // true
```