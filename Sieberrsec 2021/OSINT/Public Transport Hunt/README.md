# Public Transport Hunt 

**Category**: OSINT

380 points | 21 solves

----

## Challenge Description
My friend stole my $10 book voucher and camera away and took these pictures of bus stops and train stations...

I have absolutely no idea where he is now.

Help me trace the sly path of his scheming thievery and get my voucher back!

You might even get the voucher if you are fast enough...

Submit the flag as IRS{*A_B_C*}

Where:

A = Bus stop code of the bus stop he was at in the first image

B = MRT station code (include line code without spacing) in the second image

C = Postal code of the building right beside the bus stop he was at in the third image

## Attached files
* youcantfindme.zip, containing 3 images:

### 1.jpeg

### 2.jpeg

### 3.jpeg

----

## Solution

We know that the locations are generally in the same region, so we can solve the easier ones first and work from there. (We solved this challenge in the order 2 -> 3 -> 1.)

### 2.jpeg

Zooming into the sign visible in the second image, we can see that the MRT station has exits to Boon Lay Way and Jurong Point.

[image zoomed in]

Looking it up on Google Maps, we can find that the MRT station adjacent to Jurong Point is Boon Lay MRT station `EW27`.

[screenshot]

### 3.jpeg

The most obvious clue in the image is the bus numbered `99`. Upon closer inspection, we can also see:

* An overhead bridge,
* A roundish building on the opposite side of the road, and
* An MRT track passing behind the building.

We can look up the [route of bus 99](https://www.transitlink.com.sg/eservice/eguide/service_route.php?service=99) online, and find that it goes from Clementi Interchange to Joo Koon Interchange.

[screenshot]

Using this information, we can display the route on Google Maps (by looking up the directions between the interchanges and using appropriate filters).

[screenshot]

Then, we can use the satellite image view to identify a location along the route that contains the features identified earlier.

[annotated sc]

After a bit of searching and confirmation using Street View, we can find the bus stop where the image was taken, and identify the postal code of the adjacent building, `609961`.

[screenshot]

[screenshot]

### 1.jpeg

This was the most challenging image to work from, in my opinion. From the other two images, we know that the location is in the west of Singapore, near Boon Lay.

Examining the image closely, we can glean a little bit of information:

* A bus in the image; the bus number is a bit difficult to ascertain from the image resolution, but it looks like 243 + a letter, and the display says the route terminates in Boon Lay.
* The bus stop is near blocks that has a number ending with `5B` and `5C`.
* The nearest visible lamp post is numbered `12`.

(My teammate initially led us on a wild goose chase to look in a lamp post database to find the location of the lamp post, but it turns out there are multiple lamp posts numbered `12`, none of which were within the area we were looking for. At least now I know there's a dataset for lamp posts in Singapore :P)

Looking for the bus service online, we find two services: `243G` and `243W`. The approximately five pixels in the image look more like "G", so we can start with looking up the route of bus `243G` on Google Maps.

[screenshot]

Following the route, we can look for nearby blocks with numbers ending with `5B` or `5C`. After some searching, we can find block `685C` labelled on the map.

To confirm which bus stop it is, we can hop into Street View and look for a lamp post numbered `12`. (It will likely look different from the image, since construction is temporary.)

[screenshot]

Indeed, we can now find the bus stop, which is numbered `22599`.

[screenshot]

### Flag:
```
IRS{22599_EW27_609961}
```
