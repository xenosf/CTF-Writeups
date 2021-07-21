# History Lesson
**Category:** Forensics

---
## Challenge Description
Okay. I have both good news, and bad news. The bad news is that APOCALYPSE has deployed a mole to infiltrate our investigation team and he managed to send over one of our precious flags to them. However, the good news is that we've identified the mole, apprended him, and also obtained his browser history. Can you do some forensics and figure out what flag he leaked?
## Attached files
* Mozilla.zip
---
## Solution
Upon unzipping the file, I started looking through the folder's contents, landing in the profile folder.

I was on the lookout for something along the lines of 'history', but since there was nothing named such, the browser history was stored somewhere else. `places.sqlite` caught my eye - after all, browser history is a record of the *places* he'd visited.

<img width="1230" alt="Screenshot of Finder window" src="https://user-images.githubusercontent.com/40383042/117594071-fc75d900-b16f-11eb-8118-e794dddc9756.png">

Using DB browser for sqlite, I opened the file.

<img width="1149" alt="Screenshot 2021-05-10 at 08 57 32" src="https://user-images.githubusercontent.com/40383042/117594455-a35a7500-b170-11eb-9187-9814eb30c836.png">

Aha! There's a table called `moz_historyvisits`! But alas, this table only contained place IDs.

<img width="459" alt="Screenshot 2021-05-10 at 08 57 48" src="https://user-images.githubusercontent.com/40383042/117594196-27602d00-b170-11eb-8878-5ac1236a90d5.png">

The next most logical place to look would be `moz_places`.

<img width="460" alt="Screenshot 2021-05-10 at 08 58 07" src="https://user-images.githubusercontent.com/40383042/117594228-41017480-b170-11eb-8f97-0929ae40d38a.png">

Jackpot. All we need to do now is find the flag. After scrolling past comedy gold such as

> is teamviewer a virus - Google Search

> Amazon.com : hello kitty luggage (x2!)

 > what do I do if my life is a meme - Google Search

we finally land on

> I hope no one sees this - Pastebin.com

Well, that's not suspicious at all. Nope. Not one bit. Totally innocent.

Opening the link takes us to the pastebin with the flag.

<img width="1033" alt="Screenshot 2021-05-10 at 09 01 03" src="https://user-images.githubusercontent.com/40383042/117594254-4c54a000-b170-11eb-9c3e-09d046150027.png">


```
Cyberthon{why_4r3_y0u_5py1ng_0n_m3_y1k35}
```
