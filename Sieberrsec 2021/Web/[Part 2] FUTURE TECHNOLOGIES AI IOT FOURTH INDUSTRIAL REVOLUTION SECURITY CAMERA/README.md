# [Part 2] FUTURE TECHNOLOGIES AI IOT FOURTH INDUSTRIAL REVOLUTION SECURITY CAMERA

**Category**: Web

76 points | 35 solves

----

## Challenge Description

In our recent investigations, Siebersec got hold of a

***FUTURE TECHNOLOGIES AI IOT FOURTH INDUSTRIAL REVOLUTION SECURITY CAMERA*** .

I think that the camera is intentionally pointed away from something we need to see.

Find a way to rotate the camera (pan the camera) and let us see more things.

## Attached files

* link to same webpage as [Part 1](../%5BPart%201%5D%20FUTURE%20TECHNOLOGIES%20AI%20IOT%20FOURTH%20INDUSTRIAL%20REVOLUTION%20SECURITY%20CAMERA)

----

## Solution

After gaining access to the camera page `/camera.php` in [Part 1](../%5BPart%201%5D%20FUTURE%20TECHNOLOGIES%20AI%20IOT%20FOURTH%20INDUSTRIAL%20REVOLUTION%20SECURITY%20CAMERA), we can see that only admins can pan the camera, and that we currently do not have the role of admin:

![Screenshot of camera webpage](https://user-images.githubusercontent.com/40383042/147543125-26237d6c-a334-40f2-9d02-12013e7a360b.png)

Let's open the browser developer tools and explore a bit. After some poking around, we find that the page has stored a cookie `admin_role`, which is currently set to `false`:

![Screenshot of developer tools panel showing admin_role as false](https://user-images.githubusercontent.com/40383042/147543149-25bb3eb5-ffee-442f-8fa2-bf8968957d78.png)

Using the same developer tools panel, we can edit the value of `admin_role` and change it from `false` to `true`.

After refreshing the page, we now have the admin role, and can pan the camera to see the hidden flag:

![Screenshot of camera page with flag revealed](https://user-images.githubusercontent.com/40383042/147543174-cc05342b-a6d3-4f96-8d81-6734194a319a.png)

### Flag

```text
IRS{7h3_In73rn37_0f_vULn3erable_7hIng5}
```

[< Part 1](../%5BPart%201%5D%20FUTURE%20TECHNOLOGIES%20AI%20IOT%20FOURTH%20INDUSTRIAL%20REVOLUTION%20SECURITY%20CAMERA)
