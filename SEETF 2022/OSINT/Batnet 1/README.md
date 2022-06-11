# Batnet 1

**Category**: OSINT

100 points | 109 solves

----

## Challenge Description

"With so many people starting to use the Internet, Master Wayne has also joined in the fun. He has been fixated on using all kinds of online accounts not just for leisure, but also for monitoring and surveillance as part of his new artificial intelligence project. For Master Wayne's own safety and as my position as his butler, I need your help to find all of his accounts that use the username `1mn0tb4tm4m`." ~ Alfred

**Note:** There are 4 flag fragments to find. There are also limited attempts to this challenge so do not try to bruteforce the flag.

**Note:** There are red herrings created by other participants. Please ignore everything created after the CTF started.

Validator service: `nc fun.chall.seetf.sg 60001`

----

## Solution

To start off, we can use an OSINT tool such as [Sherlock](https://sherlock-project.github.io/) to find accounts with the username `1mn0tb4tm4m` across many social media platforms.

```sh
$ python3 sherlock --print-found 1mn0tb4tm4m
[*] Checking username 1mn0tb4tm4m on:

[+] GitHub: https://www.github.com/1mn0tb4tm4m
[+] Instagram: https://www.instagram.com/1mn0tb4tm4m
[+] Telegram: https://t.me/1mn0tb4tm4m
```

The GitHub link leads to [`1mn0tb4tm4m`'s GitHub page](https://www.github.com/1mn0tb4tm4m). The page bio tells us we can find part 3/4 of the flag here. Looking around the page, we can find the flag fragment in an extended commit message in the only repository there.

```text
_f1nd_0ut
```

Similarly, the Instagram link leads to [`1mn0tb4tm4m`'s Instagram page](https://www.instagram.com/1mn0tb4tm4m). The account's bio contains the first part of the flag.

```text
SEE{br0th3r
```

The Telegram link, on the other hand, doesn't lead anywhere. This could be a false positive. To find the other 2 flags, we need to move on to other methods.

Using the website [namechk.com](namechk.com), we can also search for social media accounts with the username `1mn0tb4tm4m`. There are some false positives, but this website tells us that there is also a TikTok account with the username `1mn0tb4tm4m`.

In the [TikTok profile](https://www.tiktok.com/@1mn0tb4tm4m)'s bio, we can find the 2nd part of the flag.

```text
_3y3_help_m3
```

The final account cannot be found using social media search tools such as Sherlock and Namechk. It took me far too long to realise while doing the challenge, but I eventually realised that the last account was a Discord account inside the SEETF Discord server.

In the's "About Me" section, there is a cryptic string:

```text
cGFydCA0LzQKX2FiMHV0XzN2ZXJ5MG5lfQ==
```

This string looks like a base64 encoded string, and decoding it gives us the last part of the flag:

```sh
$ echo "cGFydCA0LzQKX2FiMHV0XzN2ZXJ5MG5lfQ==" | base64 -d
part 4/4
_ab0ut_3very0ne}
```

Having found all 4 parts of the flag, we can put them together to form the whole flag.

### Flag

```text
SEE{br0th3r_3y3_help_m3_f1nd_0ut_ab0ut_3very0ne}
```
