
# (S)ingle Responsibility Principle (SRP)
- a class should have only one reason to change. It means that a class should only have one job or responsibility.

```javascript
class User {
	constructor(username, password) {
		this.username = username;
		this.password = password;
	}

	authenticate(inputPassword) {
		return this.password === inputPassword;
	}
}

class UserDataManager {
	static save(user) {
		const db = getDatabaseConnection();
		db.saveUser(user);
	}
}
```

```javascript
const user = new User('Alice', 'password123');
if (user.authenticate('password123')) {
	UserDataManager.save(user);
}
 ```

- the `User` class is solely concerned with the user's attributes and their management
- the `UserDataManager` class is solely concerned with user data persistence.

# (O)pen/Closed Principle (OCP)
- software entities should be open for extension but closed for modification. This means that the behavior of a module can be extended without modifying its source code.

```javascript
class Shape {
  area() {
    // OVERRIDE ME!!!!
    console.log("SHAPE DID NOT IMPLEMENT AREA!");
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  area() {
    return Math.PI * this.radius ** 2;
  }
}

class Square extends Shape {
  constructor(side) {
    super();
    this.side = side;
  }
  area() {
    return this.side * this.side;
  }
}

class AreaCalculator {
  static calculate(shape) {
    return shape.area();
  }
  static calculateAll(shapes) {
    return shapes.reduce((sum, shape) => sum + shape.area(), 0);
  }
}
```

```javascript
const c = new Circle(5);
const s = new Square(5);
```

# (L)iskov Substitution Principle (LSP)
- object of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program.
```javascript
class Bird {}
class FlyingBird extends Bird {
  fly() {
    console.log("THIS BIRD IS FLYING!");
  }
}

class Duck extends FlyingBird {
  fly() {
    console.log("Duck flying!");
  }
}

class Eagle extends FlyingBird {
  fly() {
    console.log("Eagle flying!");
  }
}

class Penguin extends Bird {}

function makeBirdFly(bird) {
  if (bird instanceof FlyingBird) {
    bird.fly();
  } else {
    console.log("this bird CANNOT fly :(");
  }
}
```

```javascript
const duck = new Duck();
const eagle = new Eagle();
const penguin = new Penguin();

makeBirdFly(duck); // Works fine
makeBirdFly(eagle);
makeBirdFly(penguin);
```

# (I)nterface Segregation Principle (ISP)
- a class should not be forced to implement interfaces it does not use.

```javascript
class Workable {
  work() {
    console.log("working on work!");
  }
}

class Eatable {
  eat() {
    console.log("eating a snack");
  }
}

class Sleepable {
  sleep() {
    console.log("taking a nap");
  }
}

function manageWork(workable) {
  workable.work();
}
```

- Here `manageWork` only depends on the work method, so we separate each method to be part of its own class.

# (D)ependency Inversion Principle (DIP)
- **High-level modules** (main application logic) should not depend directly on **low-level modules** (like specific tools or libraries).
- Both should depend on **abstractions** (interfaces or general ideas).

#### ==Example== JavaScript Sign-in
- imagine you're building an application where users can sign in. Initially, users sign in using a username and password.
- but in the future, you might want to allow sign-in using email, phone number, or even third-party providers like Google.

- you create an abstract `AuthMethod` and ensure that your main User class depends on this abstraction, not a specific implementation.
```javascript
// Abstract authentication method
class AbstractAuthMethod {
	authenticate(credentials) {
		throw new Error("This method should be overrideen.");
	}
}

class UsernamePasswordAuth extends AbstractAuthMethod {
	authenticate({ username, password }) {
		// logic to authenticate using username and password
	}
}

class EmailAuth extends AbstractAuthMethod {
	authenticate({ email, token }) {
		// logic to authenticate using email and a token sent to the email
	}
}
```

```javascript
class User {
	constructor(authMethod) {
		if (!(authMethod instanceof AbstractAuthMethod)) {
			throw new Error("Invalid authentication method!");
		}
		this.authMethod = authMethod;
	}

	login(credentials) {
		return this.authMethod.authenticate(credentials);
	}
}
```

# Law Of Demeter
- a design guideline to ensure that our objects don't reveal too much about their structure or their collaborator's structures

```javascript
class Wallet {
  constructor(money) {
    this.money = money;
  }

  debit(amount) {
    this.money -= amount;
  }

  getMoney() {
    return this.money;
  }
}
```

```javascript
class Person {
  constructor(wallet) {
    this.wallet = wallet;
  }

  getWallet() {
    return this.wallet;
  }

  payAmount(amount) {
    this.wallet.debit(amount);
  }
}
```

```javascript
class ShoppingMall {
  chargeCustomer(person, amount) {
    person.payAmount(amount);
  }
}
```

```javascript
let wallet = new Wallet(100);
let person = new Person(wallet);
let mall = new ShoppingMall();

mall.chargeCustomer(person, 50);
```