# A Wealth of Information Part 2

**Category**: OSINT

243 points | 24 solves

----

## Challenge Description
Where was I?

Submit the flag as follows:

IRS{*A*\_ROUTE_*X_B*} without any spacing and all capitals, copying "ROUTE" as is.

Where:

A = Postal code of the nearest visitor centre

B = Name of the closest building to the place

X = The name of the route/road

----

## Solution
Using the GPS data of the image from [Part 1](../A%20Wealth%20of%20Information%20Part%201), we know where the photo was taken:

```sh
$ exiftool trees.jpg
...
GPS Latitude                    : 1 deg 21' 6.71" N
GPS Longitude                   : 103 deg 46' 35.53" E
...
```

We can enter the latitude and longitude `1° 21' 6.71" N 103° 46' 35.53" E` into Google Maps to see where it is:

<img width="1298" alt="Screenshot of Google Maps showing the found location" src="https://user-images.githubusercontent.com/40383042/147556658-fd239be5-3001-4658-ba9f-78f4a2aacbf7.png">

From this, we know that the photo was taken in Bukit Timah Nature Reserve, and that the nearest visitor centre is Bukit Timah Nature Reserve Visitor Centre, which has the postal code `589333`.

<img width="368" alt="Screenshot of Google Maps information panel for visitor centre" src="https://user-images.githubusercontent.com/40383042/147556669-560c04b0-72c7-45c9-95ec-c7d5f81ef369.png">

However, the information available on Google Maps is insufficient to solve the other parts of the challenge. For this, we must look up a map of Bukit Timah Nature Reserve, such as this map from [NParks](https://www.nparks.gov.sg/gardens-parks-and-nature/parks-and-nature-reserves/bukit-timah-nature-reserve):

![Map of Bukit Timah Nature Reserve](https://www.nparks.gov.sg/-/media/nparks-real-content/gardens-parks-and-nature/parks-and-nature-reserve/bukit-timah-nature-reserve/btmapnov2016.jpg?la=en&hash=9BBF71EFDBFB076D8DB632D8DA1FD976282C398E)

Using the above map, we can see that the name of the route is `Route 2` and the nearest building is `Telecom`.

### Flag:
```
IRS{589333_ROUTE_2_TELECOM}
```

[< Part 1](../A%20Wealth%20of%20Information%20Part%201)
