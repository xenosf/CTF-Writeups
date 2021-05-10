# Secret Briefing
### Category: Forensics
---
## Challenge Description
Since APOCALYPSE recently deployed a mole to infiltrate our investigation team, we've decided to return the favor and deployed one of our members to attend their "super secrect briefing" to gather some intel on their latest schemes. Unfortunately, it seems that our mole got compromised. But on the bright side, he managed to send us a copy of the meeting slides before he got compromised. However, the most critical slide in the powerpoint seems to be missing. It would be a big help to us if you could recover the contents of that slide!
## Attached files
* apocalypse-redacted.pptx

---
## Solution
Opening the file in Powerpoint, we see the presentation.

(Cute.)

The titles of the slides are helpfully in order, showing us which one is missing.
* Step_1
* Step_2
* Step_3
* ???
* Step_5: Profit

Nice meme. That aside, we now have to recover the last slide.

From my extensive experience in trying to speed up my lecturer's droning in presentation recordings (sorry Ms T), I knew that .pptx files could be unzipped. So, I unzipped it, and dug around.

In the `ppt` folder, there was a file called `presentation.xml`.

Opening it up (and prettifying it so we can actually read it) reveals this section:
```xml
    <p:sldIdLst>
        <p:sldId id="256" r:id="rId6"/>
        <p:sldId id="257" r:id="rId7"/>
        <p:sldId id="258" r:id="rId8"/>
        <p:sldId id="259" r:id="rId9"/>
        <p:sldId id="260" r:id="rId10"/>
        <p:sldId id="262" r:id="rId12"/>
    </p:sldIdLst>
```
Following the pattern, we can deduce that there is a missing element:
```xml
    <p:sldId id="261" r:id="rId11"/>
```
Replacing it, and converting the folder back to a .pptx, should give us the presentation with the missing slide.

This is where I got stuck. No matter what I tried, zipping the file back up would always result in a file that Powerpoint refused to open or repair. I thought that I'd missed out some other file, or messed up the editing somehow, and did not end up getting the flag. 😔

Revisiting the problem afterwards with someone else, we found out that Powerpoint just doesn't like people manually re-zipping the files. 

![pain.jpg](https://media.metrolatam.com/2019/10/16/18ps27-638137169a2d73f9f63481c115a7f092.jpg)



If we edit `presentation.xml` without unzipping it, we would have no problem opening it afterwards.

Using vim, we can open the .pptx file and edit `presentation.xml` like before.

After saving it, we can open the file to reveal the hidden slide with the flag.


```Cyberthon{n0_l33k_k4pp4}```