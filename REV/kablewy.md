# kablewy

Category: REV

Points: 121

Solves: 259

>**WARNING: your computer might go kablewy if you try to do this one...**

### Solution

Looks like we need to curl the website again:

![Curl](/images/kablewy.png)

Ok, there is a js file that we need:

![Curl 2](/images/kablewyjs.png)

Disgusting, ok let's first format the code:

```js
(function () {
  ...
})();
const y =
    "KGZ1bmN0aW9uKCl7InVzZSBzdHJpY3QiO2V2YWwoYXRvYigiZDJocGJHVWdLSFJ5ZFdVcElHTnZibk52YkdVdWJHOW5LQ2RyWVdKc1pYZDVKeWs3Q25CdmMzUk5aWE56WVdkbEtDZE1KeWs3IikpfSkoKTsK",
  C = (r) => Uint8Array.from(atob(r), (e) => e.charCodeAt(0)),
  o =
    typeof window < "u" &&
    window.Blob &&
    new Blob([C(y)], { type: "text/javascript;charset=utf-8" });
function Q(r) {
  let e;
  try {
    if (((e = o && (window.URL || window.webkitURL).createObjectURL(o)), !e))
      throw "";
    const t = new Worker(e, { name: r == null ? void 0 : r.name });
    return (
      t.addEventListener("error", () => {
        (window.URL || window.webkitURL).revokeObjectURL(e);
      }),
      t
    );
  } catch {
    return new Worker("data:text/javascript;base64," + y, {
      name: r == null ? void 0 : r.name,
    });
  } finally {
    e && (window.URL || window.webkitURL).revokeObjectURL(e);
  }
}
const Z =
    "KGZ1bmN0aW9uKCl7InVzZSBzdHJpY3QiO2V2YWwoYXRvYigiZDJocGJHVWdLSFJ5ZFdVcElHTnZibk52YkdVdWJHOW5LQ2RyWVdKc1pYZDVKeWs3Q25CdmMzUk5aWE56WVdkbEtDZEpKeWs3IikpfSkoKTsK",
...

const n = [];
n.push(Q);
n.push(F);
n.push(I);
n.push(G);
n.push(g);
n.push(M);
n.push(P);
n.push(_);
n.push(re);
n.push(de);
n.push(we);
n.push(ce);
n.push(ie);
n.push(ke);
for (let r = 0; r < 100; r++) new Le();
let Re = "";
(async () => {
  for (const r of n) {
    const e = new r();
    (Re += await new Promise((t) => {
      e.onmessage = (a) => {
        t(a.data);
      };
    })),
      e.terminate();
  }
})();
```

Seems like there are a ton of weird functions with base64 strings which print the flag at the end. So let's first see what the first base64 string does:

```bash
$ echo KGZ1bmN0aW9uKCl7InVzZSBzdHJpY3QiO2V2YWwoYXRvYigiZDJocGJHVWdLSFJ5ZFdVcElHTnZibk52YkdVdWJHOW5LQ2RyWVdKc1pYZDVKeWs3Q25CdmMzUk5aWE56WVdkbEtDZE1KeWs3IikpfSkoKTsK | base64 -d
(function(){"use strict";eval(atob("d2hpbGUgKHRydWUpIGNvbnNvbGUubG9nKCdrYWJsZXd5Jyk7CnBvc3RNZXNzYWdlKCdMJyk7"))})();
```

Huh, another base64 string:

```bash
$ echo d2hpbGUgKHRydWUpIGNvbnNvbGUubG9nKCdrYWJsZXd5Jyk7CnBvc3RNZXNzYWdlKCdMJyk7 | base64 -d
while (true) console.log('kablewy');
postMessage('L');
```

Ah, we infinite print `kablewy` but also get the first letter of the flag `L`. Ok so lets now do this for all base64 strings in our `js`:

```bash
$ grep " \"[A-Za-z0-9]*\"," rev.js | xargs -I {} echo {} | tr -d ',' | base64 -d | grep -oE "\"[A-Za-z0-9]{10,}\"" | tr -d '"' | base64 -d
while (true) console.log('kablewy');
postMessage('L');while (true) console.log('kablewy');
postMessage('I');while (true) console.log('kablewy');
postMessage('T');while (true) console.log('kablewy');
postMessage('C');while (true) console.log('kablewy');
postMessage('T');while (true) console.log('kablewy');
postMessage('F');while (true) console.log('kablewy');
postMessage('{');while (true) console.log('kablewy');
postMessage('k');while (true) console.log('kablewy');
postMessage('3');while (true) console.log('kablewy');
postMessage('F');while (true) console.log('kablewy');
postMessage('7');while (true) console.log('kablewy');
postMessage('z');while (true) console.log('kablewy');
postMessage('H');while (true) console.log('kablewy');
postMessage('}');
```

### Flag

```LITCTF{k3F7zH}```


