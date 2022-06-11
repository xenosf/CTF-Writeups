# Huh Where

**Category**: OSINT

100 points | 52 solves

----

## Challenge Description

Bruce Wayne enjoys going for walks at park connectors and taking in the view of the Garden City. He recently stopped by this place which was nearby the trail he was on and took a picture before continuing on his walk.

Can you find out where this picture was taken?

Flag format is the name of the place exactly as shown on Google Maps with no capital letters and no spaces. Example would be `SEE{123sesamestreet}``.

## Attached files

* challenge.png

### challenge.png

![challenge](https://user-images.githubusercontent.com/40383042/173182168-e89395f8-4a0b-4b61-9efb-a84d0fce62ae.png)

----

## Solution

The image does not contain any metadata that could help us out, so we have to use our Eye Power to figure out where. Upon examining the photo, we can see several notable features:

* The path is circular and loops around a tree
* The image is a screenshot from Google Street View (as can be seen by the faint watermark on the left)
* The location is near some railings
* There are a few lamp posts

We also know from the challenge description that the location is near a park connector.

We can find the location using Google Maps and Street View. First, we can search "park connector", which shows us the paths of park connectors throughout Singapore.

To aid us, we can turn on terrain view to allow us to see better. We can also click the Street View person to highlight areas that are accessible through Street View.

From there, we can click around and look for a circular path, and find the correct location ([Street View link](https://goo.gl/maps/3di89zSMCJ1tCLMP8)), Lor Lada Hitam.

### Flag

```text
SEE{lorladahitam}
```
