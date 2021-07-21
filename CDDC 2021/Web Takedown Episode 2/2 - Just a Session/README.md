# Just a Session

## Challenge Description
It looks suspicious, nothing on the main page, but I’m sure that there is something you can manipulate to get further access.

Target URL: http://X.X.X.X/NMJX72GV [IP redacted]

---

## Solution
Using inspect element, we can look at the cookies.
<!--screenshot-->

We can see what looks like a session marker of some sort that's set to `0`. Let's set this to 1 and reload:
<!--screenshot-->

### Flag:
```
CDDC21{I_Have_a_C00KIE_foR_Y0u}
```
