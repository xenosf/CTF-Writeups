# Duck Delivery

**Category**: Forensics

77 points | 34 solves

----

## Challenge Description
I ordered duck for dinner but all I got was an empty box! Can you help me find where it went? 

## Attached files
* DuckBox.jpg

----

## Solution
The provided jpg initially just looks like a photo of an empty KFC bucket, but it's suspiciously large in file size for how low resolution it is.

[screenshot]

Upon `binwalk`ing the file, it is revealed that there is a zip file embedded within the file:

```sh
$ binwalk DuckBox.jpg
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.02
36881         0x9011          Zip archive data, at least v1.0 to extract, compressed size: 12519723, uncompressed size: 12519723, name: duck.gif
12556732      0xBF99BC        End of Zip archive, footer length: 22

```

We can then unzip the file to get `duck.gif`:

```sh
$ unzip DuckBox.jpg
Archive:  DuckBox.jpg
warning [DuckBox.jpg]:  36881 extra bytes at beginning or within zipfile
  (attempting to process anyway)
replace duck.gif? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
 extracting: duck.gif    
```

[duck.gif]

The gif file contains the flag:

[still frame]


### Flag:
```
IRS{H1dD3n_dUck}
```
