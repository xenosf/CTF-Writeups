# Broken System

## Challenge Description
The CryptIT Banking and Consulting company suspects that the GlobalDominationCorporation is attacking its email systems. They need your help to fix the misconfiguration.

---

## Solution

I used a [DNS checker website](https://dnschecker.org/
) to look at the DNS records for the page. Since the problem is with their email systems, we should take a look at the email-related records, such as [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework) and [DMARC](https://en.wikipedia.org/wiki/DMARC) records.

The SPF records were unfortunately a red herring:
<!--screenshot-->

However, the DMARC record had the flag!
<!--screenshot-->

### Flag:
```
CDDC21{_10x_f0r_yOur_Serv!ce_}
```
