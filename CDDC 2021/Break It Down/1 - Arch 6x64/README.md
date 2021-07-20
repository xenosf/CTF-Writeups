# Arch 6x64

## Challenge Description
The keepers got a full network traffic capture from the CyberBots network and have extracted a strange-looking string. They couldn’t find a way to understand it after running a couple of deciphering scripts. Can you clear it up?  The data is:
```
Vm0xNFYxbFdTWGhXYms1WFlURmFWVll3Wkc5ak1YQllaRVZ3YkZKdFVrWldSelZyVmxaYWRGcEVWbFZOUmtwRVZsUktSMk5yTlVWV1ZEQTk=
```

---

## Solution
Hmm... *strange-looking*, eh? What could it possibly be?? Sure looks like base64 to me.

Upon decoding from base64 once, we get this:
```
Vm14V1lWSXhWbk5XYTFaVVYwZG9jMXBYZEVwbFJtUkZWRzVrVlZadFpEVlVNRkpEVlRKR2NrNUVWVDA9
```

Still looks strange... let's decode it more. After decoding it for a total of 6 times, we get this:
```
PQQP21{0u_zL_o4F3}
```

Still not the flag. What next? We know that it starts with `CDDC21`. Additionally, we can tell that the letters have undergone a simple rotation cipher of some sort.

After using an online tool (I was too lazy to manually find the shift – for the curious, it was 13), we can decode the strange-looking string to get the flag!

### Flag:
```
CDDC21{0h_mY_b4S3}
```
