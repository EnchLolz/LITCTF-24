# jwt-2

Category: WEB

Points: 118

Solves: 311

>its like jwt-1 but this one is harder

### Solution

This time, they verify the signature, luckily the secret is provided to us in `index.ts`:

```js
...
const jwtSecret = "xook";
const jwtHeader = Buffer.from(
  JSON.stringify({ alg: "HS256", typ: "JWT" }),
  "utf-8"
)
...
```

![Bad JWT](/images/jwt2bad.png)

![Good JWT](/images/jwt2good.png)

Now make sure to replace the `_` with `/` and `-` with `+`, since jwt.io uses url safe base64, but the server uses normal base64. You could also just change your name to `a` or anything else that doesn't contain a `_` or `-`:

![Flag](/images/jwt2flag.png)


### Flag

```LITCTF{v3rifyed_thI3_Tlme_1re4DV9}```