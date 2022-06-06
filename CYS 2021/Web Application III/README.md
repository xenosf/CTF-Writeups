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

![Screenshot of http request result saying "bugger off"](https://user-images.githubusercontent.com/40383042/126046260-1d28cdbb-d284-4227-81bb-2fc0b8548f5f.png)

Pictured: Response of a HTTP request I tried to exploit. Clearly, it had been fixed.

**Disclaimer for transparency:** I ended up finding [the same question](https://www.attackdefense.com/challengedetailsnoauth?cid=501) (*and a walkthrough*) on Pentester Academy's public labs. It's scammy, but they're reusing flags so eh.

Basically, the version of WebCalendar that they are using can be exploited due to improper input sanitization in the settings page, which allows us to inject a PHP web shell.

The settings page can be accessed at `/install/index.php`, where we can enter the provided password:

![Screenshot 2021-06-23 at 14-27-23 WebCalendar Setup Wizard](https://user-images.githubusercontent.com/40383042/126046267-ce0aa681-9a75-4377-b35d-8ab8a5ed38b4.png)

![Screenshot 2021-06-23 at 14-27-42 WebCalendar Setup Wizard](https://user-images.githubusercontent.com/40383042/126046269-bc3e74c0-9fff-4145-a9cc-f98a864d148a.png)

Out of all the possible fields, the only one we can change arbitrarily without blocking our access to the calendar itself is the cache directory.

We can inject some code to find a flag file, and output its contents:

```php
​*/?><?php echo `SHELL COMMAND HERE`; ?>
```

![Screenshot 2021-06-23 at 14-28-06 WebCalendar Setup Wizard](https://user-images.githubusercontent.com/40383042/126046376-b72153bc-745f-4371-865b-b39e1a6a2c75.png)

The code injected escapes the PHP comments since the settings are stored in a commented PHP block.

To find the flag, I injected `find / -name flag`.

![Screenshot 2021-06-23 at 14-30-05 https w993fxrol9cogib1361p ap-southeast-11 attackdefensecloudlabs com](https://user-images.githubusercontent.com/40383042/126046464-d9ed60b8-d284-49af-931b-1a334e0077c0.png)

The junk at the end after `/tmp/flag` is the remainder of the settings file.

After finding the flag file `/tmp/flag`, I used `cat` to output its contents. Then, we can visit `/includes/settings.php`, which runs the code we injected, giving us the flag:

```php
86f1d2cd0ff916575562da9eebba802b readonly: false user_inc: user.php use_http_auth: false single_user: true single_user_login: admin # end settings.php */ ?> 
```

I could probably have injected an interactive web shell, since injecting something into `settings.php` prevented me from accessing the calendar afterwards, requiring me to restart the lab to inject another command. Regardless, I ended up only needing 2 commands.

### Flag

```text
86f1d2cd0ff916575562da9eebba802b
```
