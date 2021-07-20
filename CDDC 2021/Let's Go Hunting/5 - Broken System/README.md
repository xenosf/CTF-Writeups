# Broken System

## Challenge Description
The CryptIT Banking and Consulting company suspects that the GlobalDominationCorporation is attacking its email systems. They need your help to fix the misconfiguration.

---

## Solution

I used a [DNS checker website](https://dnschecker.org/
) to look at the DNS records for the page. Since the problem is with their email systems, we should take a look at the email-related records, such as [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework) and [DMARC](https://en.wikipedia.org/wiki/DMARC) records.

The SPF records were unfortunately a red herring:

![spf cryptit](https://user-images.githubusercontent.com/40383042/126385060-84ea13d7-84c2-45da-a288-9c279513623d.png)

However, the DMARC record had the flag!

![Screenshot 2021-06-23 at 19-33-20 DMARC Record Lookup - DMARC Validation - DNSChecker org](https://user-images.githubusercontent.com/40383042/126385067-4e4912a9-530f-464c-af51-ed703a2c789c.png)

### Flag:
```
CDDC21{_10x_f0r_yOur_Serv!ce_}
```
