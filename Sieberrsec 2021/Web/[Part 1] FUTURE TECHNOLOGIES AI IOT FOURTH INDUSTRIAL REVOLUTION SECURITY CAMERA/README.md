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

![Screenshot of login page](https://user-images.githubusercontent.com/40383042/147542959-c984b4bc-c32d-42e1-8090-c9f668286c4d.png)

This seems like a prime target for an SQL injection, and it is indeed: By entering `' or 1=1;--` in the password field, we can easily bypass the login and access the camera page containing the flag.

![Screenshot 2021-12-28 at 15-33-27 FUTURE TECHNOLOGIES CAMERA](https://user-images.githubusercontent.com/40383042/147542989-303cdb97-8096-473f-8457-f7ddb9e2cd73.png)

### Flag

```text
IRS{w4y_t00_eZ_1nJ3c710n}
```

[Part 2 >](../%5BPart%202%5D%20FUTURE%20TECHNOLOGIES%20AI%20IOT%20FOURTH%20INDUSTRIAL%20REVOLUTION%20SECURITY%20CAMERA)
