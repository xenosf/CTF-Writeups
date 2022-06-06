# We go way back

**Category**: OSINT

85 points | 28 solves

----

## Challenge Description

My old friends created an app and made a public presentation about it, but they changed the name to something stupid and wont tell me the old one. Could you help me find the old name?

I managed to get the google slides they used when creating the slides

Flag format: IRS{APPNAME}

## Attached files

* link to Google Slides presentation

----

## Solution

Opening the link takes us to a Google Slides presentation that reminds me of every PW project presentation I've ever seen:

![Screenshot of provided Google Slides presentation](https://user-images.githubusercontent.com/40383042/147556351-12c334a4-016b-4641-ba64-ffccabcaea7b.png)

Not gonna lie, this challenge was a bit of a scam. I spent a while trying to poke around and access the Google Slides edit history (which was inaccessible due to the link being view only), but all we had to do was use the Wayback machine, which reveals one snapshot containing the (pretty funny) original app name.

![Screenshot of Wayback Machine snapshot of Google Slides presentation](https://user-images.githubusercontent.com/40383042/147556362-1d949249-06cb-42b2-b6aa-933ed0d17d14.png)

### Flag

```text
IRS{IHungry}
```
