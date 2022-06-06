# Just a Session

## Challenge Description

It looks suspicious, nothing on the main page, but I'm sure that there is something you can manipulate to get further access.

Target URL: `http://X.X.X.X/NMJX72GV` [IP redacted]

---

## Solution

Using inspect element, we can look at the cookies.

<img width="1400" alt="Screenshot 2021-06-24 at 16 27 27" src="https://user-images.githubusercontent.com/40383042/126439012-720c8c92-29dc-428c-882a-5cd298facbe2.png">

We can see what looks like a session marker of some sort that's set to `0`. Let's set this to `1` and reload:

<img width="1400" alt="Screenshot 2021-06-24 at 16 27 42" src="https://user-images.githubusercontent.com/40383042/126439016-ca42fe14-81f3-41ff-897b-9550c0df247f.png">

### Flag

```text
CDDC21{I_Have_a_C00KIE_foR_Y0u}
```
