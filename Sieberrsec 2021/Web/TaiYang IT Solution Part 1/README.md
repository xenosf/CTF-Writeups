# TaiYang IT Solution Part 1

**Category**: Web

470 points | 13 solves

----

## Challenge Description

TaiYang IT Solution offers a variety of services, including one that is put behind a supposedly secure Google Log In. However, I heard that it uses... ***questionable*** validation code.

Log in as the admin, and get the flag!


## Attached files
* link to webpage
* 7z file containing source code of page

----

## Solution

Opening the link provided takes us to this dubious-looking website. For part 1 of this challenge, we have to gain access to the v1 portal.

[screenshot]

Poking around the site a bit, we find a press release, which gives us hints on how to solve the challenge:

> **Press Release on Cybersecurity Incident**
> 
> Our preliminary investigations revealed that our systems did not perform adequate checks for **JSON WEB tokens** issued from SSO providers like Google.
> 
> This allowed attackers to login as TaiYang IT Solution users accounts such as taiyang.it.solution [ at ] gmail.com.
> 
> However, due to adequate IP address protection on our end, no user data was disclosed.
> 
> TaiYang IT Solution is rolling out a v2 portal which addresses the original security vulnerability.
> 
> TaiYang IT Solution.

Essentially, JSON Web Tokens (JWTs) encode data and allow for verification. However, as seen from this press release, this website does not verify the tokens properly, allowing us to just use a doctored one to trick the website into thinking we are signing in using their user account.

Moving onto the login portal itself, we see a link to sign in with Google.

[screenshot]

When we try to sign in with another Google account, we are denied access to the flag:

[screenshot]

Using our browser's Developer Tools network tab, we can see that the page sends a POST request with a token. This token is a JWT, which we can examine and edit using tools such as [jwt.io](https://jwt.io).

Currently, the token being sent belongs to the Google account we used to sign in, and thus contains our own credentials.

[screenshot]

Now, let us examine the source code for the login page, `login-v1.php`. solution@gmail.com`.

### login-v1.php
```php
<?php
  session_start();

  try {
    if (empty($_POST["token"])) {
      if (empty($_SESSION["email"]) || !($_SESSION["email"] === "taiyang.it.solution@gmail.com")) {
        echo "You are not a verified TaiYang IT Solution employee or partner. Please log out and login again with the correct account!";
        $_SESSION["email"] = "";
      }
      else {
        echo "Welcome to IT Solution portal! Here is a flag: IRS{FLAG_REDACTED}";
      }
    }
    else {
      $data1 = explode(".", $_POST["token"]);
      $data2 = base64_decode($data1[1]);
      $data3 = json_decode($data2, true);
      $_SESSION["email"] = $data3["email"];
      die("ok");
    }
  }
  
...
...
```

If the POST request's `token` contains a JWT, it will save the token's `"email"` to a session variable. When the page is (re)loaded afterwards with an empty `token`, it will check to see if the email stored is `taiyang.it.

Additionally, this code only uses the part of the JWT containing the data, allowing us to randomly put in a header that does not require a private key, or just put in gibberish:

[screenshot]

With all this information, we can now edit the token being sent so that it contains TaiYang IT Solution's email address instead of our own:

[screenshot]

Sending this doctored token to the login page and then refreshing the page allows us to access the flag:

[screenshot]


### Flag:
```
IRS{g00g13_s1gn_1n_d035nt_m3aN_y0uR3_sAf3}
```

[Part 2 >](../TaiYang%20IT%20Solution%20Part%202%3A%20Electric%20Boogaloo)
