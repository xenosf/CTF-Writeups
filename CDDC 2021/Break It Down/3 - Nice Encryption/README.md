# Nice Encryption

## Challenge Description

We managed to steal an encrypted message from the GDC. The problem is, it is encrypted with some unknown encryption. Help us to find the secret message.

## Attached files

* decrypt_me_plz

---

## Solution

When opened in a text editor, the file seemed like gibberish. Looks like XOR to me!

To decode it, I used CyberChef to XOR brute force it:
![Screenshot 2021-06-24 at 11-05-35 XOR Brute Force - CyberChef](https://user-images.githubusercontent.com/40383042/126366143-b79ec5f2-13a4-4ed4-a620-dd9dbf4d7db5.png)

Lo and behold... the flag.

P.S. After submitting, the hint was revealed:
> The first byte on the encrypted message is probably important…

I'm honestly not sure what to do with this information, since the key wasn't the first character. Strange.

### Flag

```text
CDDC21{Wh0_wrot3_th!s_Thin6?}
```
