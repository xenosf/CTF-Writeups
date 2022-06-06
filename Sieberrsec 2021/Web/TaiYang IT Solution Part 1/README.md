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

Opening the link provided takes us to this mildly-dubious-looking website with links to 2 portals. For part 1 of this challenge, we have to gain access to the v1 portal.

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

![Screenshot of web page saying 'You are not a verified TaiYang IT Solution employee or partner. Please log out and login again with the correct account!'](https://user-images.githubusercontent.com/40383042/147594676-faee116b-bb52-4b23-a53b-112676b2a471.png)

When we try to sign in with a random Google account, we are denied access to the flag:

![Screenshot 2021-12-29 at 01-39-44 Screenshot](https://user-images.githubusercontent.com/40383042/147594692-f7aef21e-953d-4605-a8dd-4dcce31c555a.png)

Using our browser's Developer Tools network tab, we can see that the page sends a POST request with a token. This token is a JWT, which we can examine and edit using tools such as [jwt.io](https://jwt.io).

Currently, the token being sent belongs to the Google account we used to sign in, and thus contains our own credentials.

<img width="1398" alt="Screenshot of web page with developer tools tab open, showing the POST request used to send the token" src="https://user-images.githubusercontent.com/40383042/147594777-be7292e7-980c-4417-8936-71ba5cbc6f2f.png">

![Screenshot of token decoded using jwt.io, with personal info redacted](https://user-images.githubusercontent.com/40383042/147595357-19f4b617-d5d0-47b1-b3c8-106fe0ce7608.png)
(Personal information redacted)

Now, let us examine the source code for the login page, `login-v1.php`. This part is of interest to us:

```php
...
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
```

If the POST request's `token` contains a JWT, it will save the token's `"email"` to a session variable. When the page is (re)loaded afterwards with an empty `token`, it will check to see if the email stored is `taiyang.it.solution@gmail.com`.

Additionally, this code only uses the part of the JWT containing the data, allowing us to put in anything for the header and signature. (This means we can use a different algorithm that does not require matching public/private keys, or just replace the header and signature sections with gibberish.)

With all this information, we can now edit the token being sent so that it contains TaiYang IT Solution's email address instead of our own:

![Screenshot of token decoded and edited to contain TaiYang IT Solution's email](https://user-images.githubusercontent.com/40383042/147595852-4faf3b02-29ea-49e6-a863-08f6b0cd1116.png)

Sending this doctored token to the login page (I used the developer tools' "edit and resend" option for the POST request) and then refreshing the page allows us to access the flag:

![Screenshot of page with flag](https://user-images.githubusercontent.com/40383042/147595874-6d54cdf0-913e-4096-9ebe-de3112196a79.png)

### Flag

```text
IRS{g00g13_s1gn_1n_d035nt_m3aN_y0uR3_sAf3}
```

[Part 2 >](../TaiYang%20IT%20Solution%20Part%202%3A%20Electric%20Boogaloo)
