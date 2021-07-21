# Traversal

## Challenge Description
I know that this server is vulnerable, but I can’t exploit it. Show your skills and find the right file.

Target URL: http://X.X.X.X/4HOF3DTV [IP redacted]

---

## Solution
Visiting the provided URL takes you to `http://X.X.X.X/4HOF3DTV/?page=home.php`, which looks like it's vulnerable to a directory traversal attack.

I tried `?page=../../../` and other simple attacks, but it did not work, which means they had some simple filtering in place to prevent such attacks.

However, let's press on – to help me, I used the [DotDotPwn fuzzing tool](https://github.com/wireghoul/dotdotpwn) to generate and check for possible traversals:
<img width="776" alt="Screenshot 2021-06-25 at 09 08 19" src="https://user-images.githubusercontent.com/40383042/126441262-edac1f67-63f0-4f41-a4d3-fea0c4b42198.png">

I set the `-q` switch so it would only print the vulnerable results.

By default, this tool looks for the string `root:` in `/etc/passwd` to find the root password. I wasn't sure if the flag would be in that file, but I figured that if it wasn't, I could explore more after finding a suitable method of entry.

However, it turns out that the flag was in `/etc/passwd`, and visiting any URL found by the tool gives us the flag.

### Flag:
```
CDDC21{!_like_the_PASSWD-F!le!}
```
