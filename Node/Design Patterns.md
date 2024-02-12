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
- CommonJS modules
- Module definition patterns
- ESM: ECMAScript Modules
- ESM and CommonJS differences