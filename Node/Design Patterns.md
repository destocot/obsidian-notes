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

## Module definition patterns
- the main factor to consider is the balance between private and public functionality

#### Named exports
- the most basic method for exposing a public API is using **named exports**, which involves assigning the values we want to make public to properties of the object references by `exports` (or `module.exports`)

#### Exporting a function
- one of the most popular module definition patterns consists of reassigning the whole `module.exports` variable to a function
	- the main strength of this pattern is the fact that it allows you to expose a single functionality
- this way of defining modules is also know in the community as the **substack pattern**

- using the exported function as a namespace for other public APIs
```js
// logger.js
module.exports = (message) => {
	console.log(`info: ${message}`);
}

module.exports.verbose = (message) => {
	console.log(`verbose: ${message}`);
}
```

```js
// main.js
const logger = require("./logger");

logger("This is an informational message");
logger.verbose("This is a verbose message");
```

> The modularity of Node.js heavily encourages the adoption of the **single-responsibility principle (SRP)**

#### Exporting a class
- a module that exports a class is a specialization of a module that exports a function
- the difference is that with this new pattern, we allow the user to create new instances using the constructor, but we also give them the ability to extend its prototype and forge new classes

#### Exporting an instance
- we can leverage the caching mechanism of `require()` to easily define stateful instances created from a constructor or a factory
- similar to creating a **singleton**

## Modifying other modules or the global scope
- this is referred to as **monkey patching**
- the practice of modifying the existing objects at runtime to change or extend their behavior or to apply temporary fixes
- is generally considered bad practice, but this pattern can be useful and safe under some circumstances (for example, testing)

# ESM: ECMAScript modules
- also known as ES modules or ESM

- the most important differentiator between ESM and CommonJS is that ES modules are *static*
	- meaning that imports are described at the top level of every module and outside any control flow statement

#### Using ESM in Node.js
- several ways to tell the Node.js interpreter to consider a given module as an ES module rather than a CommonJS module
	- give the module file the extension `.mjs`
	- add to the nearest `package.json` a field called "type" with a value of "module"

#### Named exports and imports
- the `import` keyword is flexible, and allows us to import one or more entities and even to rename imports
```js
import * as loggerModule from "./logger.js"
```
- this example the `*` syntax (also called the **namespace import**)  imports all members of the module and assigns them to the local `loggerModule` variable

#### Default exports and imports
- with ESM we can export a single unnamed entity with the `export default` keywords
- internally, a default export is equivalent to a named export with `default` as the name
#### Mixed Exports
```js
import mylog, { info } from "./logger.js";
```

- it is generally considered good practice to stick with named exports

#### Module identifiers (module specifiers)
- the different types of values that we can use in our `import` statements to specify the location of the module we want to load

#### Async imports
- ES modules provides async imports (also called *dynamic imports*)
- async imports can be performed at runtime using the special `import()` operator
	- is syntactically equivalent to a **function** that takes a **module identifier** as an argument and it returns a **promise** that resolve to a **module object**

==example== building a command line application that can print "Hello World" in different languages
```js
// strings-el.js 
export const HELLO = 'Γεια σου κόσμε';

// strings-en.js 
export const HELLO = 'Hello World';

//strings-es.js 
export const HELLO = 'Hola mundo';

//strings-it.js
export const HELLO = 'Ciao mondo';

//strings-pl.js 
export const HELLO = 'Witaj świecie';
```


```js
// main.js 

// (1) Define a list of supported languages
const SUPPORTED_LANGUAGES = ['el', 'en', 'es', 'it', 'pl'];

// (2) Read the selected language from the first argument passed in the command line
const selectedLanguage = process.argv[2];

// (3) Handle the case where the selected language is not supported.
if (!SUPPORTED_LANGUAGES.includes(selectedLanguage)) { 
	console.error('The specified language is not supported');
	process.exit(1); 
}

// (4) Dynamically build the name of the module based on the selected language.
// NOTE: the module name needs to be a relative path to the module file
const translationModule = `./strings-${selectedLanguage}.js`;

// (5) Use the import() operator to trigger the dynamic import of the module;
import(translationModule)
	.then((strings) => {
	// (6) the dynamic import happens asynchronously 
	// strings will be the module namespace
	console.log(strings.HELLO) ;
})
```

```bash
node main.js it
```
- should print `Ciao mondo`








continue from [ module loading in depth ] 