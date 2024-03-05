```ts
// parse mailto
function parseMailto(s: string) {
  const [start, params] = s.split("?");
  const recipient = start.split(":")[1];
  const result: ParseMailto = { recipient };
  params.split("&").forEach((param) => {
    const [key, encoded] = param.split("=");
    result[key] = decodeURIComponent(encoded);
  });
  return result;
}
```