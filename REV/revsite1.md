# revsite1

Category: REV

Points: 155

Solves: 96

>It's a website?? (no ads ofc)

### Solution

Disgusting, we need to reverse a webassembly flag checker :sob:

![Wasm Code](/images/revsite1wasmflagcheck.png)

Downloading the WASM, we can use `wasm-decompile` from the `WABT` tool to generate some `C` flavored pseudocode:

![Psuedo Code](/images/revsite1psuedocode.png)

Unfortunately we still have 10k lines to analyze ;-; From here I put the code into python, and I assumed the check_flag function would generate our flag, specfically in array `d` as it's the only array. I also manually cleaned up some syntaxx and used find and replace to get rid of the non python syntax like `var`, `:int`, etc:

![Python Code](/images/revsite1pythonish.png)

Then skipping over all the random operations done in the middle (it was only math operations, no function calls or anything else :skull:), we can look at the very end of `check_flag`:

![Function End](/images/revsite1psuedocodeend.png)

Going back to our python code we can modify the end by removing everything after the `strcmp` and just printing out `d`:

![Python patch](/images/revsite1pythonpatch.png)

Luckily all my jumping to conclusions worked out and we get an output that is plausible as the flag:

```
output:
[76, 73, 84, 67, 84, 70, 123, 116, 48, 100, 52, 121, 95, 49, 53, 95, 108, 49, 116, 51, 114, 97, 108, 108, 121, 95, 116, 104, 51, 95, 100, 52, 121, 95, 98, 51, 102, 48, 114, 101, 95, 116, 104, 101, 95, 99, 48, 110, 116, 51, 115, 116, 125, 0, 0, 0, 0, 0, 0, 0]
```

```py
>>> ''.join(chr(x) for x in [76, 73, 84, 67, 84, 70, 123, 116, 48, 100, 52, 121, 95, 49, 53, 95, 108, 49, 116, 51, 114, 97, 108, 108, 121, 95, 116, 104, 51, 95, 100, 52, 121, 95, 98, 51, 102, 48, 114, 101, 95, 116, 104, 101, 95, 99, 48, 110, 116, 51, 115, 116, 125, 0, 0, 0, 0, 0, 0, 0])
'LITCTF{t0d4y_15_l1t3rally_th3_d4y_b3f0re_the_c0nt3st}\x00\x00\x00\x00\x00\x00\x00'
```

Yeah idk what I was doing but it worked \:D

### Flag

```LITCTF{t0d4y_15_l1t3rally_th3_d4y_b3f0re_the_c0nt3st}```


