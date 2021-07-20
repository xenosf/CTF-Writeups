# Name System

## Challenge Description
It’s unbelievable! Every day, we get a new piece of information about the Global Domination Corporation. Now it’s a new domain, globdominationcorp.com.  I tried to check if they have a website at www.globdominationcorp.com, but there is nothing there. What else can we do?

---

## Solution
As mentioned in the description, there is nothing at `www.globdominationcorp.com`, but what about other subdomains?

Using the `theHarvester` tool, we can enumerate the subdomains – that is, list out all of the subdomains:
<!--screenshot-->

Upon visiting the `internal` subdomain, we can find the flag:
<!--screenshots-->

### Flag:
```
CDDC21{EnumeRation_!s_the_KEY_F0R_EveryTHING!}
```
