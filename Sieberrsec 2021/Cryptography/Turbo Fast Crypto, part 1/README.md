# Turbo Fast Crypto, part 1

**Category**: Cryptography

117 points | 29 solves

----

## Challenge Description
We found the frontend code for a remote encryption service at `nc [challenge IP]`:

```python
import turbofastcrypto # The source code for this module is only available for part 2 of this challenge :)
while 1:
    plaintext = input('> ')
    ciphertext = turbofastcrypto.encrypt(plaintext)
    print('Encrypted: ' + str(ciphertext))
```

My partner says it operates under the hood with "XOR", whatever that means. I need you to recover the key.

----

## Solution

`nc`ing into the remote encryption service allows us to input something and get encrypted output.

```sh
$ nc [challenge IP]
> abcdefghijklmnopqrstuvwxyz1234567890
Encrypted: b'(00\x1f\x16\x03\x04\x1a\x0c\x1e\x183\x0c\x1c\n/\x03\x17\x05\x11\x14\x1a\x12\x1cX[L234567890'
```

Since we know that it "operates under the hood with 'XOR'", we can get the key back by XORing the output with the input. The Python code I used to do this is below:

```py
plaintext = "abcdefghijklmnopqrstuvwxyz1234567890"
ciphertext = b'(00\x1f\x16\x03\x04\x1a\x0c\x1e\x183\x0c\x1c\n/\x03\x17\x05\x11\x14\x1a\x12\x1cX[L234567890'.decode()
key=""

for (char1, char2) in zip(plaintext, ciphertext):
  key += chr(ord(char1)^ord(char2))

print(key)
```

Running the above script prints the flag.

### Flag:
```
IRS{secrets_are_revealed!!}
```
