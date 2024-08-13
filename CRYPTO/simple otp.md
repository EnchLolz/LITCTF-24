# simple otp

Category: Crypto

Points: 110

Solves: 535

>We all know OTP is unbreakable...

### Solution

```py
import random

encoded_with_xor = b'\x81Nx\x9b\xea)\xe4\x11\xc5 e\xbb\xcdR\xb7\x8f:\xf8\x8bJ\x15\x0e.n\\-/4\x91\xdcN\x8a'

random.seed(0)
key = random.randbytes(32)
```

We are given the seed so we can just reverse the xor.

```py
>>> enc = b'\x81Nx\x9b\xea)\xe4\x11\xc5 e\xbb\xcdR\xb7\x8f:\xf8\x8bJ\x15\x0e.n\\-/4\x91\xdcN\x8a'
>>> random.seed(0)
>>> key = random.randbytes(32)
>>> ''.join(chr(enc[i]^key[i]) for i in range(32))
'LITCTF{sillyOTPlol!!!!sdfsgvkhf}'
```

### Flag

```LITCTF{sillyOTPlol!!!!sdfsgvkhf}```


