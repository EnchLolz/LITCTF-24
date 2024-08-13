# pope shuffle

Category: Crypto

Points: 122

Solves: 250

>it's like caesar cipher but better. Encoded: ࠴࠱࠼ࠫ࠼࠮ࡣࡋࡍࠨ࡛ࡍ࡚ࡇ࡛ࠩࡔࡉࡌࡥ

### Solution

Putting it into python we can see that we have some pretty big utf-8 characters:

```py
>>> "\U+0834\U+0831\U+083C\U+082B\U+083C\U+082E\U+0863\U+084B\U+084D\U+0828\U+085B\U+084D\U+085A\U+0847\U+085B\U+0829\U+0854\U+0849\U+084C\U+0865"
'࠴࠱࠼ࠫ࠼\u082eࡣࡋࡍࠨ࡛ࡍ࡚ࡇ࡛ࠩࡔࡉࡌࡥ'
>>> [ ord(x) for x in "\U+0834\U+0831\U+083C\U+082B\U+083C\U+082E\U+0863\U+084B\U+084D\U+0828\U+085B\U+084D\U+085A\U+0847\U+085B\U+0829\U+0854\U+0849\U+084C\U+0865"]
[2100, 2097, 2108, 2091, 2108, 2094, 2147, 2123, 2125, 2088, 2139, 2125, 2138, 2119, 2139, 2089, 2132, 2121, 2124, 2149]
```

Since we know the flag wrapper starts with `L=76`, we can see that `2100-76 = 2024`. From here we can just subtract `2024` from all the values to get the flag:

```py
>>> l = [2100, 2097, 2108, 2091, 2108, 2094, 2147, 2123, 2125, 2088, 2139, 2125, 2138, 2119, 2139, 2089, 2132, 2121, 2124, 2149]
>>> ''.join(chr(x-2024) for x in l)
'LITCTF{ce@ser_sAlad}'
```

### Flag

```LITCTF{ce@ser_sAlad}```


