```js
setTimeout(() => {
  throw new Error("oops");
}, 300);

process.on("uncaughtException", (error) => {
  console.log(error);
  console.log("uncaughtException");
});

process.on("unhandledRejection", (error) => {
  console.log(error);
  console.log("unhandledRejection");
});

```