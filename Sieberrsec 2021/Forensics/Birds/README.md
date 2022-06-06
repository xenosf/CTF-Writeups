# Birds?

**Category**: Forensics

114 points | 30 solves

----

## Challenge Description

I can't help but feel that the birds are trying to tell me something... 

## Attached files

* suspiciousbirds.mp3

----

## Solution

The file sounds like birds chirping (vaguely reminiscent of Orchard Road's horde of mynahs lol), but there's a suspiciously robotic sounding part in the middle. We can open the file in Audacity to investigate further:

<img width="1052" alt="Screenshot of provided mp3 file in Audacity waveform view" src="https://user-images.githubusercontent.com/40383042/147550915-5c1c1b1d-0d54-4e25-a00f-f871f5db288e.png">

The waveform doesn't look too odd, so let's check out the spectrogram view:

<img width="1275" alt="Screenshot of provided mp3 file in Audacity spectrogram view" src="https://user-images.githubusercontent.com/40383042/147550595-c37ae908-d52c-4a2d-9944-1de6b53db5d4.png">

Behold! A hidden message:

![Screenshot of spectrogram hidden message edited for readability](https://user-images.githubusercontent.com/40383042/147550806-bab7ab29-aede-4c0d-8def-b806bccff5be.png)

```text
68 74 74 70 73 3a 2f 2f 70 61 73 74 65 62 69 6e 2e 63 6f 6d 2f 5a 7a 6b 61 46 59 69 4c
```

Converting this hex to text, we get a pastebin link...

```text
https://pastebin.com/ZzkaFYiL
```

...which contains the flag:

<img width="1052" alt="Screenshot of pastebin page containing the flag" src="https://user-images.githubusercontent.com/40383042/147550374-125ce958-2bc5-4f7b-b028-f2b9b7543a20.png">

### Flag

```text
IRS{s0m3Th1n9_5ouNDs_w3iRd}
```
