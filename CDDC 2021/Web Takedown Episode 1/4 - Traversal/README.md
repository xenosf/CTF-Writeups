# Traversal

## Challenge Description
I know that this server is vulnerable, but I can’t exploit it. Show your skills and find the right file.

Target URL: http://X.X.X.X/4HOF3DTV [IP redacted]

---

## Solution
Visiting the provided URL takes you to `http://X.X.X.X/4HOF3DTV/?page=home.php`, which looks like it's vulnerable to a directory traversal attack.

I tried `?page=../../../` and other simple attacks, but it did not work, which means they had some simple filtering in place to prevent such attacks.

However, let's press on – to help me, I used the `dotdotpwn` fuzzing tool to generate and check for possible traversals:

<!--screenshot-->

I set the `-q` switch so it would only print the vulnerable results.

This tool gave us multiple vulnerable URLs, but we only need 1.

By default, this tool looks for the string `root:` in `/etc/passwd` to find the root password. I wasn't sure if the flag would be in that file, but I figured that if it wasn't, I could explore more after finding a suitable method of entry.

However, it turns out that the flag was in `/etc/passwd`, and visiting the first URL found by the tool gives us the flag.

### Flag:
```
CDDC21{!_like_the_PASSWD-F!le!}
```
