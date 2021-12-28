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
![1](https://user-images.githubusercontent.com/40383042/147566333-16e40fe7-6bbb-4be4-a716-2b88b894f731.jpeg)

### 2.jpeg
![2](https://user-images.githubusercontent.com/40383042/147566344-042c0287-6e38-44a2-981a-6ea2444889df.jpeg)

### 3.jpeg
![3](https://user-images.githubusercontent.com/40383042/147566362-2f19bc15-53e9-47bc-b441-18b2d9da8868.jpeg)

----

## Solution

We know that the locations are generally in the same region, so we can solve the easier ones first and work from there. (We solved this challenge in the order 2 -> 3 -> 1.)

### 2.jpeg

Zooming into the sign visible in the second image, we can see that the MRT station has exits to Boon Lay Way and Jurong Point.

<img width="238" alt="2.jpeg zoomed in to a sign saying Boon Lay Way and Jurong Point" src="https://user-images.githubusercontent.com/40383042/147566473-38e86292-2471-42c2-8ec6-68ad781b22ff.png">

Looking it up on Google Maps, we can find that the MRT station adjacent to Jurong Point is Boon Lay MRT station `EW27`.

<img width="357" alt="Screenshot of Google Maps showing Boon Lay MRT (EW27) next to Jurong Point mall" src="https://user-images.githubusercontent.com/40383042/147570515-a3490561-1f3c-4b60-836b-daf055c48339.png">

### 3.jpeg

The most obvious clue in the image is the bus numbered `99`. Upon closer inspection, we can also see:

* An overhead bridge,
* A roundish building on the opposite side of the road, and
* An MRT track passing behind the building.

We can look up the [route of bus 99](https://moovitapp.com/singapore_新加坡-1678/lines/99/589111/2317091/en-gb) online, and find that it goes from Clementi Interchange to Joo Koon Interchange.

<img width="347" alt="Screenshot of bus 99 route synopsis on Moovit" src="https://user-images.githubusercontent.com/40383042/147566734-fd7b752d-48aa-414f-8772-c860f0a1911f.png">

Using this information, we can display the route on Google Maps (by looking up the directions between the interchanges and using appropriate filters).

<img width="1222" alt="Screenshot of bus 99 route as displayed on Google Maps" src="https://user-images.githubusercontent.com/40383042/147570749-89192c07-6ee6-4494-9126-5428229a6912.png">

Then, we can use the satellite image view to identify a location along the route that contains the features identified earlier.

<img width="910" alt="Screenshot of Google Maps display of bus 99 route alongside MRT line" src="https://user-images.githubusercontent.com/40383042/147571020-f1e3232b-150d-4a76-8265-a300f3a0f3ef.png">

<img width="772" alt="Screenshot of satellite view of bus stop with annotated features" src="https://user-images.githubusercontent.com/40383042/147571123-38bb0e25-e93a-4e1d-826a-a3a66af5160e.png">

After a bit of searching and [confirmation using Street View](https://goo.gl/maps/952vEBDYe12Wzab5A), we can find the bus stop where the image was taken, and identify the postal code of the adjacent building, `609961`.

<img width="746" alt="Screenshot of Google Maps information panel containing postal code of adjacent building" src="https://user-images.githubusercontent.com/40383042/147571266-cc6c8558-29a1-46bf-ac70-e2b72f508e70.png">


### 1.jpeg

This was the most challenging image to work from, in my opinion. From the other two images, we know that the location is in the west of Singapore, near Boon Lay.

Examining the image closely, we can glean a little bit of information:

* A bus in the image; the bus number is a bit difficult to ascertain from the image resolution, but it looks like 243 + a letter, and the display says the route terminates in Boon Lay.
* The bus stop is near blocks that has a number ending with `5B` and `5C`.
* The nearest visible lamp post is numbered `12`.

(My teammate initially led us on a wild goose chase to look in a lamp post database to find the location of the lamp post, but it turns out there are multiple lamp posts numbered `12`, none of which were within the area we were looking for. At least now I know there's a dataset for lamp posts in Singapore :P)

Looking for the bus service online, we find two services: `243G` and `243W`.

<img width="351" alt="Screenshot of search results for bus number 243 on Moovit" src="https://user-images.githubusercontent.com/40383042/147568488-f0cc55c7-2e1e-424c-b5a1-5baa9c3fe537.png">

The approximately five pixels in the image look more like "G", so we can start with looking up the route of bus `243G` on Google Maps.

<img width="714" alt="Screenshot of Google Maps satellite view displaying route of bus 243G" src="https://user-images.githubusercontent.com/40383042/147571413-0a0820e7-572d-4534-9954-dd723b84c6b5.png">

Following the route, we can look for nearby blocks with numbers ending with `5B` or `5C`. After some searching, we can find block `685C` labelled on the map.

<img width="396" alt="Screenshot of Google Maps satellite view showing block 685C" src="https://user-images.githubusercontent.com/40383042/147571461-9a1be9f4-b915-42e6-a489-9c5139ea0cae.png">

To confirm which bus stop it is, we can [hop into Street View](https://goo.gl/maps/8Z7R7AreGxw2KhbK6) and look for a lamp post numbered `12`. (It will likely look different from the image, since construction is temporary.)

Indeed, we can now find the bus stop, which is numbered `22599`.

<img width="629" alt="image" src="https://user-images.githubusercontent.com/40383042/147571679-6c6d2183-24d9-4175-a84d-584ef54a2601.png">

### Flag:
```
IRS{22599_EW27_609961}
```
