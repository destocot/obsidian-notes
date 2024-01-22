## Plain Old JavaScript Object ("POJO"):

```javascript
let o1 = {};
let o2 = new Object();

o1.name = "Whiskey";

// Bracket notation supports expressions
o1["name"] = "Whiskey";

o1.taco; // undefined

// All keys get "stringified"
o1[1] = "hello";
o1["1"] = "goodbye";

// Values can be functions
o1.sayHi = function () {
  return "Hi!";
};
o1.sayHi();
```

# Mixing Data & Functions With Objects
- Using a POJO:
```javascript
// This references "this object"
let myTri_1 = {
  a: 3,
  b: 4,

  getArea: function () {
    return (this.a + this.b) / 2;
  },

  getHypothenuse: function () {
    return Math.sqrt(this.a ** 2 + this.b ** 2);
  },
};
```

# Class Basics
- Classes are a "blueprint" of functionality. Defines the methods each instance of Triangle will have.

```javascript
class Triangle {
  // Constructor will be called on making a new instance
  // Things to do in constructor: Validate data, Assign properties
  constructor(a, b) {
    if (!Number.isFinite(a) || a <= 0) throw new Error(`Invalid a: ${a}`);
    
    if (!Number.isFinite(b) || b <= 0) throw new Error(`Invalid b: ${b}`);
    
    this.a = a;
    this.b = b;
  }
  
  getArea() {
    return (this.a * this.b) / 2;
  }

  getHypotenuse() {
    return Math.sqrt(this.a ** 2 + this.b ** 2);
  }

  describe() {
    return `I am a triangle with an area of ${this.getArea()}`;
  }
}

let myTri = new Triangle(3, 4); // "Instantiation"
myTri.getArea(); // 6
myTri.getHypothenuse(); // 5

typeof myTri; // object
myTri instanceof Triangle; // true
```

# Inheritance and Extends
```javascript
class ShyTriangle extends Triangle {
  // Don't repeat if not different
  // Will "inherit" from "parent"
  describe() {
    return "(runs and hides)";
  }
}
```

# Super
```javascript
class ColorTriangle extends Triangle {
  constructor(a, b, color) {
    // Call parent constructor w/ (a, b)
    super(a, b);
    this.color = color;
  }
}

class ColorMoodTriangle extends ColorTriangle {
  constructor(a, b, color, mood) {
    super(a, b, color);
    this.mood = mood;
  }
}
```

# Static Properties
- individual pieces of data are on the class, not an instance
```javascript
class CatWithStaticProp {
  constructor(name) {
    this.name = name;
  }

  static meow() {
    console.log("THIS IS: ", this);
  }

  // Good example of a static property
  // All instances of cats are the same species;
  // It doesn't vary from one cat to another
  static genusSpecies = "feline catus";

  describe() {
    return `${this.name} is a ${CatWithStaticProp.genusSpecies}`;
  }
}

// Can access static properties without an instance
CatWithStaticProp.genusSpecies; // feline catus
```

# Group Related Functionality With Static Methods
```javascript
class MyMath {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }
}
```

# Use Static Methods As Factory Functions
```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `${this.name} says hi`;
  }

  static registerUser(username, password) {
    const names = ["Bob", "Rob", "Tom"];
    const name = names[Math.floor(Math.random() * names.length)];
    return new User(name, "unknown");
  }
}
```

# Getters and Setters Example
```javascript
class UserProfile {
  constructor(username, email, birthdate) {
    this.setUsername(username);
    this.setEmail(email);
    this.setBirthdate(birthdate);
  }

  get username() {
    return this._username;
  }

  set username(newUsername) {
    this.setUsername(newUsername);
  }

  get email() {
    return this._email;
  }

  set email(newEmail) {
    this.setEmail(newEmail);
  }

  get birthdate() {
    return this._birthdate;
  }

  set birthdate(newBirthdate) {
    this.setBirthdate(newBirthdate);
  }

  setUsername(username) {
    if (typeof username !== "string" || username.length === 0) {
      throw new Error("Invalid username.");
    }
    this._username = username;
  }

  setEmail(email) {
    if (!email.includes("@")) {
      throw new Error("Invalid email.");
    }
    this._email = email;
  }

  setBirthdate(birthdate) {
    if (isNaN(Date.parse(birthdate))) {
      throw new Error("Invalid birthdate.");
    }
    this._birthdate = birthdate;
  }
}
```

