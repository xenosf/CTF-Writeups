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

The GitHub link leads to [`1mn0tb4tm4m`'s GitHub page](https://www.github.com/1mn0tb4tm4m). The page bio tells us we can find part 3/4 of the flag here:

<img width="279" alt="Screenshot of GitHub profile bio saying 'Part 3/4 is found here'" src="https://user-images.githubusercontent.com/40383042/173200682-e5cf8284-26b5-4db4-a319-58c9e4a4aa58.png">

Looking around the page, we can find the flag fragment in an extended commit message in the account's only repository.

<img width="761" alt="Screenshot of extended commit message containing part 3 of the flag" src="https://user-images.githubusercontent.com/40383042/173200687-25fbf395-1107-4b34-8d70-b7f5045e1abc.png">

```text
_f1nd_0ut
```

Similarly, the Instagram link leads to [`1mn0tb4tm4m`'s Instagram page](https://www.instagram.com/1mn0tb4tm4m). The account's bio contains the first part of the flag.

<img width="372" alt="Screenshot of Instagram profile with bio containing part 1 of the flag" src="https://user-images.githubusercontent.com/40383042/173200695-e98be794-9ad5-449b-80cf-d4d98532f84f.png">

```text
SEE{br0th3r
```

The Telegram link, on the other hand, is a false positive and doesn't lead anywhere. To find the other 2 flags, we need to move on to other methods.

Using the website [namechk.com](https://namechk.com), we can also search for social media accounts with the username `1mn0tb4tm4m`. There are some false positives, but this website tells us that there is also a TikTok account with the username `1mn0tb4tm4m`.

<img width="726" alt="Screenshot of Namechk.com search results showing various social media platforms with username availability highlighted" src="https://user-images.githubusercontent.com/40383042/173200720-be1dd527-c356-4407-83f9-b600ba4f4133.png">

In the [TikTok profile](https://www.tiktok.com/@1mn0tb4tm4m)'s bio, we can find the 2nd part of the flag.

<img width="451" alt="Screenshot of TikTok profile with bio containing part 2 of the flag" src="https://user-images.githubusercontent.com/40383042/173200718-3733e7eb-4614-4e93-8477-0b099be21f27.png">

```text
_3y3_help_m3
```

The final account cannot be found using social media search tools such as Sherlock and Namechk.com. It took me far too long to realise while doing the challenge, but I eventually realised that the last account was a Discord account inside the SEETF Discord server.

<img width="293" alt="Screenshot of Discord server search for messages from the account" src="https://user-images.githubusercontent.com/40383042/173200810-b9ecfaf5-5d17-4fb5-a8e1-956f2f8dbc87.png">

In the "About Me" section, there is a cryptic string:

<img width="240" alt="Screenshot of the Discord profile containing base64 encoded flag" src="https://user-images.githubusercontent.com/40383042/173200812-b5c5cd96-ff39-46ba-9663-e5be1e8a65c8.png">

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
