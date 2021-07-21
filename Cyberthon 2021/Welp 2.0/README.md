# Welp 2.0
**Category:** Crypto

---
## Challenge Description
APOCALYPSE heard us dissing on their RSA encryption script and decided to update it. But the moment we saw their updated implementation, we all collectively went welp once again. Let's see if you can manage to decrypt a flag that has been encrypted with their updated script.
## Attached files
* generate.py
* pubkey.pem
* flag.txt.encrypted
### generate.py
```python
from Crypto.Util.number import getPrime
from Crypto.PublicKey import RSA
from Crypto.Util.number import bytes_to_long, long_to_bytes

rsa = RSA.construct((getPrime(512) * getPrime(512), 5))

with open('flag.txt', 'rb') as f:
    data = f.read()

plaintext = bytes_to_long(data)
ciphertext = pow(plaintext, rsa.e, rsa.n)

with open('./dist/flag.txt.encrypted', 'wb') as f:
    f.write(long_to_bytes(ciphertext))

with open('./dist/pubkey.pem', 'wb') as f:
    f.write(rsa.export_key('PEM'))
```
---
## Solution
Comparing `generate.py` to the one from [Welp 1.0](../Welp%201.0), it was nearly identical, save for this line:

```python
rsa = RSA.construct((getPrime(512) * getPrime(512), 5))
```

...where the public exponent `e` had been changed from `2` to `5`. *Welp*.

To solve this, I used the exact same script I used in Welp 1.0. 

```python
from Crypto.PublicKey import RSA
from Crypto.Util.number import bytes_to_long, long_to_bytes
import gmpy2

with open('pubkey.pem', 'rb') as f:
    rsa = RSA.import_key(f.read())

with open('flag.txt.encrypted', 'rb') as f:
    ciphertext = bytes_to_long(f.read())

for k in range(0, 1000):
    plaintext, exact = gmpy2.iroot(ciphertext + k*rsa.n, rsa.e)
    if exact:
        with open(f'flag.txt', 'wb') as f:
            f.write(long_to_bytes(plaintext))
        break
```

I went into more detail in [the earlier write-up](../Welp%201.0), and it's pretty much all the same stuff.

Running the script gives us the flag:

```
Cyberthon{f0rg0t_t0_p4d!}
```
