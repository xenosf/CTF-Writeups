# A Wealth of Information Part 1

**Category**: OSINT

69 points | 45 solves

----

## Challenge Description
Found this picture from somewhere, and NOW it's YOUR TURN to find something out from it!

Tell me:

*A.* Name of the Camera Brand (in PascalCase)

*B.* The Aperture of the Camera (include "f/")

*C.* Focal length of the lens (include units without space)

*D.* Is the flash on or off (ON/OFF)

*E.* Date at which this picture was taken (DDMMYY)

*F.* Is the place above or below sea level (ABOVE/BELOW)

Submit the flag as follows:

IRS{*A_B_C_D_E_F*}

## Attached files
* trees.jpg

----

## Solution
The information we seek can be found in the EXIF data of the provided image. To view it, we can use a photo viewing/editing program, or use a command line tool.

Using Preview.app, we can open the image and use `Tools > Show Location Info` to look at the information:

[screenshots]

Alternatively, we can use the command line tool `exiftool` to view the EXIF information (only the relevant information has been pasted below):

```sh
$ exiftool trees.jpg
...
Make                            : Apple
Camera Model Name               : iPhone 13
...
Aperture Value                  : 1.6
...
Flash                           : Off, Did not fire
Focal Length                    : 5.1 mm
...
GPS Altitude Ref                : Above Sea Level
...
Create Date                     : 2021:10:16 14:04:20.578+08:00
Date/Time Original              : 2021:10:16 14:04:20.578+08:00
Modify Date                     : 2021:10:16 14:04:20+08:00
...

```

### Flag:
```
IRS{Apple_f/1.6_5.1mm_OFF_161021_ABOVE}
```

[Part 2 >](../A%20Wealth%20of%20Information%20Part%202)
