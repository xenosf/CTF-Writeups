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

[image]

Let's open the browser developer tools and explore a bit. After some poking around, we find that the page has stored a cookie `admin_role`, which is currently set to `false`:

[image]

Using the same developer tools panel, we can edit the value of `admin_role` and change it from `false` to `true`.

After refreshing the page, we now have the admin role, and can pan the camera to see the hidden flag:

[image]

### Flag:
```
IRS{7h3_In73rn37_0f_vULn3erable_7hIng5}
```

[< Part 1](../%5BPart%201%5D%20FUTURE%20TECHNOLOGIES%20AI%20IOT%20FOURTH%20INDUSTRIAL%20REVOLUTION%20SECURITY%20CAMERA)