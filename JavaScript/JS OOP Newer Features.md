d# Getters and Setters

```javascript
// Getters
// Allow you to retrieve the value of an object's property.

// Setters
// Allow you to set the value of an object's property.
class Circle {
  static allowedColors = new Set(["red", "green", "blue"]);

  constructor(radius, color) {
    this._radius = radius;
    this.setColor(color);
  }

  setColor(newColor) {
    if (Circle.allowedColors.has(newColor)) {
      this._color = newColor;
    } else {
      throw new Error("That color is not allowed");
    }
  }

  get radius() {
    return this._radius;
  }

  get color() {
    return this._color;
  }

  set color(newColor) {
    this.setColor(newColor);
  }

  //   Setter for the radius
  // acts as property (no need to invoke)
  set radius(value) {
    if (value < 0) {
      throw new Error("Radius cannot be negative!");
    } else {
      this._radius = value;
    }
  }

  // Getter for the diameter
  // acts as property (no need to invoke)
  get diameter() {
    return this._radius * 2;
  }
}
```

# Class Fields
```javascript
class MyClass {
  // Public field
  publicField = "Public Field";

  // Private field
  // maintains encapsulation and
  // not allow external acces
  #privateField = "Private Field";

  getPrivateField() {
    return this.#privateField;
  }

  // Private Method (same idea as private field)
  #privateMethod() {
    console.log("PRIVATE METHOD CALLED");
  }

  publicMethod() {
    this.#privateMethod();
  }
}

const instance = new MyClass();
console.log(instance.publicField); // "Public Field"
console.log(instance.getPrivateField()); // "Private Field"
// console.log(instance.#privateField); // Error

/** in chrome dev tools: instance.#privateField will work **/
```

# ES2022 Static Initialization Blocks
```javascript
// Block of code that runs once, when class is loaded
// "this" refers to the class itself not the instance
class DatabaseConnection {
  static connection;

  static {
    if (process.env.NODE_ENV === "production") {
      this.connection = this.loadProductionConnection();
    } else {
      this.connection = this.loadDevelopmentConnection();
    }
  }

  static loadProductionConnection() {}

  static loadDevelopmentConnection() {}
}
```