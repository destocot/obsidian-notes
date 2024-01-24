# Module Pattern
- the **module pattern** ensures private and public encapsulation in JavaScript, protecting the global namespace and diminishing naming conflicts.
```javascript
const ChickenModule = (() => {
	const eggColor = "white"; // private variable
	const click = () => console.log("Cluck! Cluck!"); // private function
	return {
		layEgg: () => {
			console.log("Laid a", eggColor, "egg.");
			cluck();
		}
	}
})();

ChickenModule.layEgg();
```

- with the use of `IIFE` (Immediately Invoked Function Expression). We can encapsulate functionality.

- ==example== `jquery` uses the module pattern to protect internal details and reveal only the public API.

# Singleton Pattern
- the **singleton pattern** assures only one instance of a class.
```javascript
const ChickenFarm = (() => {
	let instance;
	const createInstance = () => ( { totalChickens: 100 });
	return {
		getInstance: () => {
			if (!instance) {
				instance = createInstance();
			}
			return instance;
		}
	})
})();
```

- Each call to `ChickenFarm.getInstance()` will return the same reference.

```javascript
const instance1 = ChickenFarm.getInstance();
const instance2 = ChickenFarm.getInstance();
console.log(instance1 === instance2); // true
```

#### ==example== Database Connection
```javascript
class DatabaseConnection {
	static instance;

  constructor() {
    if (!DatabaseConnection.instance) {
      this.connection = this.createConnection(); 
      // assume createConnection establishes a database connection
      DatabaseConnection.instance = this;
    }
    return DatabaseConnection.instance;
  }

  createConnection() {
    // logic to connect to the database
    console.log("CREATING CONNECTION TO DATABASE!!!!");
    return { connection: "I am the database connection object" };
  }

  // Other database-related methods...
}

```

- ==example== the `Redux` store in react applications act as a singleton, ensuring a singular store instance.

# Observer Pattern
- the **observer pattern** enables a subscription model where objects (observers) "listen" to events and get notified when events occur.
- ==example== JavaScript's event handling in browsers (`DOM` event listeners) is built upon the observer pattern.

> the Blog class maintains a list of subscribers

```javascript
class Blog {
  constructor() {
    this.subscribers = [];
  }

  subscribe(subscriber) {
    this.subscribers.push(subscriber);
  }

  unsubscribe(subscriber) {
    this.subscribers = this.subscribers.filter((s) => s !== subscriber);
  }

  publish(post) {
    this.subscribers.forEach((subscriber) => subscriber.notify(post));
  }
}
```

> each subscriber gets notified of a change when there is a published post

```javascript
class Subscriber {
  constructor(name) {
    this.name = name;
  }

  notify(post) {
    console.log(
      `${this.name} received notification: New post published - ${post.title}`
    );
  }
}
```

```javascript
const colt = new Subscriber("Colt");
const daniel = new Subscriber("Daniel");

const puppyBlog = new Blog();
puppyBlog.subscribe(colt);
puppyBlog.subscribe(daniel);

```

- ==example== `Vue.js` uses a reactive data model where component re-renders are triggered by data changes, which is an application of the Observer Pattern.
- ==example== `RxJS` is a library that provides utilities for working with observables, making extensive use of the Observer Pattern.
# Registry Pattern
- the **registry pattern** is a design pattern used to store and retrieve instances of objects.
- it acts like a central place to manage the objects, and it is particularly useful when you want to access the instances from different parts of your application without needing to pass them around as parameters.

> Imagine a farm scenario where we need to keep track of all our chickens.
> 
> Instead of passing around a list of chickens or trying to remember where we last accessed a particular chicken, we can use the registry pattern to provide centralized access.
> 
> In this example, each chicken will have a unique ID (e.g., a tag number) and some properties like *name, age,* and *breed*.

```javascript
class Chicken {
  constructor(id, name, age, breed) {
    this.id = id;
    this.name = name;
    this.age = age;
    this.breed = breed;
  }
}
```

```javascript
class ChickenRegistry {
  #chickens = new Map();

  addChicken(chicken) {
    if (!chicken.id) {
      throw new Error("Chicken must have an id");
    }
    this.#chickens.set(chicken.id, chicken);
  }

  getChicken(id) {
    return this.#chickens.get(id);
  }

  removeChicken(id) {
    this.#chickens.delete(id);
  }

  getAllChickens() {
    return [...this.#chickens.values()];
  }
}
```

```javascript
const chicken1 = new Chicken(1, "Henrietta", 3, "Silkie");
const chicken2 = new Chicken(2, "Banjo", 3, "Leghorn");

const farm = new ChickenRegistry();
farm.addChicken(chicken1);
farm.addChicken(chicken2);
```

- ==example== `websocket` chat application
	- the chat Room is not coupled to any particular technology
	- users of this class only need to use `.get` to find a room, they do not need to worry about the registry

# `Mixin` Pattern

```javascript
class Animal {
  constructor(name, species) {
    this.name = name;
    this.species = species;
  }
  greet() {
    return `${this.name} says hi`;
  }
}
```

```javascript
const fly = {
  fly() {
    return `${this.name} flies!!!`;
  },
  land() {
    return `${this.name}, the ${this.species}, returns to earth`;
  },
};
```

```javascript
const swim = {
  swim() {
    return `${this.name} swims underwater! Wow!`;
  },
};
```

```javascript
const bernie = new Animal("Bernie", "Pelican");
Object.assign(bernie, fly);
Object.assign(bernie, swim);
```

# Proxy Pattern (Objects)

- the `Proxy` object enables you to create a proxy for another object
	- which can intercept and redefine fundamental operations for that object

```javascript
const cat = {
  name: "Blue Steele",
  age: 7,
  breed: "Scottish Fold",
};
```

```javascript
const handler = {
  get: function (obj, property) {
    console.log(`Accessing ${property} from object`);
    return obj[property];
  },
  set: function (obj, property, value) {
    if (property === "age") {
      if (value < 0) {
        obj[property] = 0;
      } else {
        obj[property] = value;
      }
    }
  },
};
```

```javascript
const catProxy = new Proxy(cat, handler);
catProxy.name; // Accessing name from object 'Blue Steele'
catProxy.age = 4; // 4
```

# Proxy Pattern (Function Calls)

```javascript
function multiply(a, b) {
	return a * b;
}

function sum(a, b) {
  console.log(a + b);
}
```

```javascript
const handler = {
	apply: function(targetFunction, thisArg, argsList) {
		console.log("You ran the function.");
		console.log("args are: " + argsList);
		targetFunction(...argsList);
	}
}
```

```javascript
const multiplyProxy = new Proxy(multiply, handler);
const sumProxy = new Proxy(sum, handler);
```

# Data Binding With Proxy Objects

```javascript
const input = document.querySelector("#usernameInput");
const h1 = document.querySelector("#usernameDisplay");

const userInfo = {
  username: "",
};
```

```javascript
const handler = {
  set: function (obj, property, newValue) {
    obj[property] = newValue;
    h1.textContent = "Hello there, " + newValue;
  },
};
```

```javascript
const userProxy = new Proxy(userInfo, handler);

input.addEventListener("keyup", (evt) => {
  userProxy.username = evt.target.value;
});
```

