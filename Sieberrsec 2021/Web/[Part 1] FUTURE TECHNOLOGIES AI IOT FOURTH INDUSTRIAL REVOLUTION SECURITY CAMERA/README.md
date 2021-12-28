# [Part 1] FUTURE TECHNOLOGIES AI IOT FOURTH INDUSTRIAL REVOLUTION SECURITY CAMERA

**Category**: Web

63 points | 44 solves

----

## Challenge Description
In our recent investigations, Siebersec got hold of a

***FUTURE TECHNOLOGIES AI IOT FOURTH INDUSTRIAL REVOLUTION SECURITY CAMERA*** .

We suspect that it's yet another one of those vulnerable IoT devices with a web interface that's basically asking to be attacked.

Try logging in as a camera viewer.


## Attached files
* link to webpage

----

## Solution
Visiting this link takes us to a login page.

[image]

This seems like a prime target for an SQL injection, and it is indeed: By entering `' or 1=1;--` in the password field, we can easily bypass the login and access the camera page containing the flag.

[image]

### Flag:
```
IRS{w4y_t00_eZ_1nJ3c710n}
```
[Part 2 >](../%5BPart%202%5D%20FUTURE%20TECHNOLOGIES%20AI%20IOT%20FOURTH%20INDUSTRIAL%20REVOLUTION%20SECURITY%20CAMERA)