# Name System

## Challenge Description

It's unbelievable! Every day, we get a new piece of information about the Global Domination Corporation. Now it's a new domain, globdominationcorp.com.  I tried to check if they have a website at www.globdominationcorp.com, but there is nothing there. What else can we do?

---

## Solution

As mentioned in the description, there is nothing at `www.globdominationcorp.com`, but what about other subdomains?

Using the `theHarvester` tool, which comes installed in Kali, we can enumerate the subdomains &ndash; that is, list out all of the subdomains:

<img width="568" alt="Screenshot 2021-06-24 at 18 53 20" src="https://user-images.githubusercontent.com/40383042/126383916-14b0a667-022e-4b83-ac2f-0afc3db4d95c.png">

Upon visiting the `internal` subdomain, we can find the flag:

<img width="557" alt="Screenshot 2021-06-24 at 18 42 08" src="https://user-images.githubusercontent.com/40383042/126383920-5baf063d-2864-456a-bbbd-140b5babde2a.png">

### Flag

```text
CDDC21{EnumeRation_!s_the_KEY_F0R_EveryTHING!}
```
