# Web Application III

## Challenge Description
An attacker might get administrative access to a web application. However, this does not automatically mean that the web server can be compromised. In cases where a SaaS application is made available to users, it is routine to give each user admin access to his own instance of the web application e.g. a managed hosted Wordpress site. In such a scenario, the attacker who will begin accessing the application as a managed administrative user will have to figure out how to exploit the administrative interface to get a shell on the server. In some cases, it might be possible to do privilege escalation as well.

In the exercise below, the attacker has administrative access to the web application and needs to find a command injection attack to run arbitrary commands on the server.

A version of WebCalendar is vulnerable to a command injection attack.

**Objective:** Your task is to find and exploit this vulnerability.

## Instructions
The following password may be used to explore the application and/or find a vulnerability which might require authenticated access:
* WebCalendar password

---

## Solution

Honestly... I had no clue how to do this one. I searched for "WebCalendar vulnerability" and tried to use what came up, to no avail:
<!--screenshot-->

Disclaimer for transparency: I ended up finding [the same question](https://www.attackdefense.com/challengedetailsnoauth?cid=501) (*and a walkthrough*) on Pentester Academy's public labs. It's scammy, but they're reusing flags so eh.

Basically, the version of WebCalendar that they are using can be exploited due to improper input sanitization in the settings page, which allows us to inject a PHP web shell.

The settings page can be accessed at `/install/index.php`, where we can enter the provided password:
<!--screenshot-->
<!--screenshot-->

Out of all the possible fields, the only one we can change arbitrarily without blocking our access to the calendar itself is the cache directory. 

We can inject some code to find a flag file, and output its contents:
```php
​*/?><?php echo `find / -name flag | cat`; ?>
```
<!--screenshot-->

The code injected escapes the PHP comments since the settings are stored in a commented PHP block.

Then, we can visit `/includes/settings.php`, which runs the code we injected:
<!--screenshot-->

### Flag:
```
86f1d2cd0ff916575562da9eebba802b
```
