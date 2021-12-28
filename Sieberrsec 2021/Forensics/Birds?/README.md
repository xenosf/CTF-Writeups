# Birds?

**Category**: Forensics

114 points | 30 solves

----

## Challenge Description
I can't help but feel that the birds are trying to tell me something... 
Locate his hidden project and unveal his findings to everyone!

## Attached files
* suspiciousbirds.mp3

----

## Solution
The file sounds like birds chirping (vaguely reminiscent of Orchard Road's horde of mynahs lol), but there's a suspiciously robotic sounding part in the middle. We can open the file in Audacity to investigate further:

[screenshot]

The waveform doesn't look too odd, so let's check out the spectrogram view:

Behold! A hidden message:

```
68 74 74 70 73 3a 2f 2f 70 61 73 74 65 62 69 6e 2e 63 6f 6d 2f 5a 7a 6b 61 46 59 69 4c
```

Converting this hex to text, we get a pastebin link...

```
https://pastebin.com/ZzkaFYiL
```

...which contains the flag:

[screenshot]

### Flag:
```
IRS{s0m3Th1n9_5ouNDs_w3iRd}
```
