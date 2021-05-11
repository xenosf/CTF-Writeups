# Welp 1.0
### Category: Crypto
---
## Challenge Description
APOCALYPSE has recently started to encrypt all their files with RSA. But the moment we saw their implementation, all we can say is welp. Let's see if you can manage to decrypt a flag that has been encrypted with their script.
## Attached files
* generate.py
* pubkey.pem
* flag.txt.encrypted
### generate.py
```python
from Crypto.Util.number import getPrime
from Crypto.PublicKey import RSA
from Crypto.Util.number import bytes_to_long, long_to_bytes

rsa = RSA.construct((getPrime(512) * getPrime(512), 2))

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
Looking at the provided code, something was sus. More specifically, this line:

```python
rsa = RSA.construct((getPrime(512) * getPrime(512), 2))
```

The first argument of the function is the modulus `n`, and the second argument is the public exponent `e`.

But APOCALYPSE did a whoopsie and made `e` laughably small. Welp indeed.

We can reverse this by square-rooting the ciphertext. To deal with the `mod n`, we can bruteforce its coefficient `k` as the flag is short enough for it to be manageable.

With this knowledge, we can write a script that lets us decode the flag.

However, my initial implementation used base Python math functions, which could not handle the large numbers. This resulted in decoding of only the first 7 characters of the file \<\/3 and I had no idea how to fix this.

Revisiting the problem afterwards (and taking a look at another write-up^), I found the library `gmpy2` which could handle large numbers, and has a convenient function `iroot` which allows us to find the `n`th root of a number, and tells us whether or not it is exact.

Using this library, I wrote a new script:

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

that gives us the decoded flag! 🎉

```
Cyberthon{w3lp_th15_15_4b50lut3ly_n0t_h0w_y0u_5h0uld_r5444}
```

### Footnote
*^Many thanks to @arul00 - [this write-up](https://github.com/arul00/CTF-Stuff/tree/main/Cyberthon%20Writeups/Finals/Welp%201.0) helped me a lot with regards to implementation, especially using `gmpy2`. The more you know 💫*