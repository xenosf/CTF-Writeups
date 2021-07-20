# No Rotations

## Challenge Description
We have taken control of their IRC channel for a short amount of time! We got an important message that was being broadcast to all the accounts in there! It has to be important! Please use the pieces wisely and make the message clear and readable.

## Attached files

### ciphered-message.txt
```
Mxx jia ThfacFpjr mca tmxxao jp pcoac! 
Sj'r snbprrsfxa jp xaj jia carsrjmlta jia pbbpcjwlsjh jp xamk
pwc slypcnmjspl mlhnpca! Fa tmcaywx!

Jia O-OMH rtiaowxa esxx fa slypcnao xmjac.

yxmu sr TOOT21{rWfrJsjwJspl_TsBiAcslu}
```

### key.txt
```
key = 'MFTOAYUISVKXNLPBDCRJWQEGHZ'
```

---

## Solution
Given the key, we can tell that this is a substitution cipher.

Using the below script, I decoded the ciphered text.

```python
encrypted = ""
decrypted = ""
key = "MFTOAYUISVKXNLPBDCRJWQEGHZ"
alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

with open("ciphered-message.txt") as f:
	encrypted = f.read().upper()

for char in encrypted:
	if (key.find(char)!=-1):
		print(key.find(char))
		decrypted += alphabet[key.find(char)]
	else:
		decrypted += char

print(decrypted)
```

Output:
```
ALL THE CYBERBOTS ARE CALLED TO ORDER! 
IT'S IMPOSSIBLE TO LET THE RESISTANCE THE OPPORTUNITY TO LEAK
OUR INFORMATION ANYMORE! BE CAREFUL!

THE D-DAY SCHEDULE WILL BE INFORMED LATER.

FLAG IS CDDC21{SUBSTITUTION_CIPHERING}
```

### Flag:
```
CDDC21{SUBSTITUTION_CIPHERING}
```
