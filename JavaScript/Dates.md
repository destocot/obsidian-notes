```js
// Saturday, October 12
new Date(event.date).toLocaleDateString("en-US", {
	weekday: "long",
	month: "long",
	day: "numeric",
})
```

```js
// 11
new Date(event.date).toLocaleDateString("en-US", {
	day: "2-digit",
})
```

```js
// Nov
new Date(event.date).toLocaleDateString("en-US", {
  month: "short",
})
```