## `JWT` (`JsonWebToken`)
- take a JavaScript object and a secret `string` and deterministically convert it into a random string (which can be converted back using the same secret)
```javascript
import jwt from "jsonwebtoken";

const createJWT = (user) => {
    const token = jwt.sign({id: user.id, username: user.username}, JWT_SECRET)
}
```

```typescript
export const protect = (req: Request, res: Response, next: NextFunction) => {
  const bearer = req.headers.authorization;
  if (!bearer) return res.status(401).end();

  const [, token] = bearer.split(" ");
  if (!token) return res.status(401).end();

  try {
    const payload = jwt.verify(token, JWT_SECRET);
    req.user = payload;
    next();
  } catch (error) {
    if (error instanceof JsonWebTokenError)
      console.log(error.message);
    return res.status(401).end();
  }
};
```

## Express Server
```javascript
import express from "express";
import router from "./router";

const app = express();
app.use("/api", router);

app.listen(3001, () => {
	console.log("Server started at http://localhost:3001");
});
```

## Middlewares
```typescript
export function secret(req: Request, res: Response, next: NextFunction) {
  req.shhh = "SECRET TUNNEL";
  next();
}
```

```typescript
// server.ts
import morgan from "morgan";
import { secret } from "./middlewares";

const app = express();
app.use(morgan("dev"));
app.use(secret);
```
## Extending Types
```typescript
// src/types/express/index.d.ts
import express from "express";

declare global {
  namespace Express {
    interface Request {
      fieldName?: string;
    }
  }
}
```

```typescript
// tsconfig.json
{
  "compilerOptions": {
    "typeRoots": ["./src/types", "./node_modules/@types"]
  }
}
```

## Input Validation
```bash
npm install express-validator
```

```ts
import { body } from "express-validator";
const createProductValidation = () => body("name").isString();
```

```ts
import { Request, Response, NextFunction } from "express";
import { validationResult } from "express-validator";

export function handleInputErrors(
  req: Request,
  res: Response,
  next: NextFunction
) {
  const errors = validationResult(req);

  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }

  next();
}
```

```ts
router.post(
	"/product", // route
	createProductValidation(),  // middleware
	handleInputErrors, // middleware
	createProduct // controller
);
```

## Error Handling
- express handles synchronous errors by default
```ts
// handling synchronous errors
import express, { Request, Response, NextFunction } from "express";
const app = express();

app.get("/", (req, res) => {
	throw new Error("hello");
});

app.use((
		err: Error, 
		req: Request, 
		res: Response, 
		next: NextFunction
	) => {
	return res.json({ message: "oops there was an error " });
});
```

- asynchronous error handling
- using the `next()` function you can pass asynchronous errors to express
```ts
// handling asynchronous errors
import express, { Request, Response, NextFunction } from "express";
const app = express();

app.get("/", (req, res, next) => {
	setTimeout(() => {
		next(new Error("hello"))
	}, 1)
});

app.use((
		err: Error, 
		req: Request, 
		res: Response, 
		next: NextFunction
	) => {
	return res.json({ message: "oops there was an error " });
});
```

- handle errors manually 
```ts
import prisma from "../db";
import { NextFunction, Request, Response } from "express";
import { createJWT, hashPassword } from "../modules/auth";

export const createNewUser = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const user = await prisma.user.create({
      data: {
        username: req.body.username,
        password: await hashPassword(req.body.password),
      },
    });

    const token = createJWT(user);
    res.status(201).json({ token });
  } catch (error: any) {
    error.type = "input";
    next(error);
  }
};
```

```ts
import express, { Request, Response, NextFunction } from "express";
import { createNewUser } from "./handlers/user";

const app = express();

app.post("/user", createNewUser);

app.use((err: any, req: Request, res: Response, next: NextFunction) => {
  console.log(err.message);
  if (err.type === "auth") {
    return res.json({ message: "unauthorized" }).status(401);
  } else if (err.type === "input") {
    return res.json({ message: "invalid input" }).status(400);
  }
  return res.json({ message: "oops, that's on us" }).status(500);
});
```




----
## Vanilla Server
```javascript
import http from "http";

const server = http.createServer((req, res) => {
  if (req.method === "GET" && req.url === "/") {
    res.statusCode = 200;
    res.end();
  }
});

server.listen(8080, () => { console.log("server on port 3001") });
```