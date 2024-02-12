# 1. The Node.js Platform

- The Node.js philosophy or the "Node way"
- The reactor pattern
	- the mechanism at the heart of the Node.js asynchronous event-driven architecture
- JavaScript on the server compared to the browser

**Unix philosophy**
> Small is beautiful.
> Make each program do one thing well.

**The reactor pattern**
- the main idea behind the reactor pattern is to have a handler associated with each I/O operation.
	- a handler in Node.js is represented by a callback (or cb for short) function.

>Handles I/O by blocking until new events are available from a set of observed resources, and then reacts by dispatching each event to an associated handler.

# 2. The Module System

- The module system and its patterns
	- (2015) ECMAScript 6 (ECMAScript 2015 or ES2015) - official proposal for a standard module system
	- **revealing module pattern**
		- using an IIFE (Immediately Invoked Function Express), we create a private scope, only exporting the parts that are meant to be public.
```javascript
const myModule = (() => {
  const privateFoo = () => {};
  const privateBar = [];

  const exported = {
    publicFoo: () => {},
    publicBar: () => {}
  };

  return exported;
})(); 

// Accessing the public methods
console.log(myModule);
// { publicFoo: [Function: publicFoo], publicBar: [Function: publicBar] }
console.log(myModule.publicFoo, myModule.publicBar); 
// undefined undefined
```

- CommonJS modules
	- (how it works)
		- module name is accepted, and the path is resolved
		- if the module has been loaded in the past, it should be returned from the cache
		- if not, we create a `module` object that contains an `exports` property initialize with an empty object, this object will be populated by the code of the module to export its public API, the `module` object is then cached
		- module source code is read and its code is evaluated, and is returned to the called
	- everything is private unless it's assigned to the `module.exports` variable
	- any assignment to `module.exports` must by synchronous




- Module definition patterns
- ESM: ECMAScript Modules
- ESM and CommonJS differences