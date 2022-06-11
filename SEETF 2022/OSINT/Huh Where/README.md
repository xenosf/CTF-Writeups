# Huh Where

**Category**: OSINT

100 points | 52 solves

----

## Challenge Description

Bruce Wayne enjoys going for walks at park connectors and taking in the view of the Garden City. He recently stopped by this place which was nearby the trail he was on and took a picture before continuing on his walk.

Can you find out where this picture was taken?

Flag format is the name of the place exactly as shown on Google Maps with no capital letters and no spaces. Example would be `SEE{123sesamestreet}`.

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

![Challenge image with notable features highlighted](https://user-images.githubusercontent.com/40383042/173193841-aee44f4f-9934-4f6b-9b2d-348f2b09a238.png)

We also know from the challenge description that the location is near a park connector.

We can find the location using Google Maps and Street View. First, we can search "park connector", which shows us the paths of park connectors throughout Singapore.

To aid us, we can turn on terrain view to allow us to see better. We can also click the Street View person to highlight areas that are accessible through Street View.

![Screenshot of Google Maps terrain view with park connectors and street view paths highlighted](https://user-images.githubusercontent.com/40383042/173193683-3624b83f-027d-4c35-9a71-e39087ef0fa8.png)

From there, we can click around and look for a small circular path, and find the correct location, Lor Lada Hitam.
![Screenshot of Google Maps satellite view zoomed in to show the circular path](https://user-images.githubusercontent.com/40383042/173193710-7b02d956-54f0-405e-9309-5896379bc732.png)

We can enter Street View and use the features of the image given to us to verify if we are at the correct location. ([Final location Street View link](https://goo.gl/maps/3di89zSMCJ1tCLMP8))

### Flag

```text
SEE{lorladahitam}
```
