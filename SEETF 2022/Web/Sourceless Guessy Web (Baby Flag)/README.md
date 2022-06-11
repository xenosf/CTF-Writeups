# 🧑‍🎓 Sourceless Guessy Web (Baby Flag)

**Category**: Web

100 points | 435 solves

----

## Challenge Description

WHY SO SERIOUS? WHY NEED SOURCE?

Contrary to the title of this challenge, you do not need to guess. As usual, do not bruteforce or scan our infrastructure, it is not allowed.

Note: There are two flags for this challenge.

`http://[url]`

For beginners:

* https://portswigger.net/web-security/file-path-traversal

----

## Solution

Opening the link to the website provided takes us to a page with some Batman-themed text art.

A hyperlink labelled "WHY NEED SOURCE?" is present on the page. Clicking it takes us to the PHP info page.

Under `http_referer`, we find `http://[url]/?page=whysoserious`.

Visiting that URL, we end up at the original page. This seems vulnerable to a directory traversal attack; we can try to access `/etc/passwd` by setting `page` to something like `../../../../etc/passwd`. After doing this, the text in the Batman text art changes to the contents of `/etc/passwd`:

```text
✂️--- SNIP ---✂️
usr sbin nologin SEE 2nd fl4g n33ds RCE g00d luck h4x0r
✂️--- SNIP ---✂️
```

We can combine the words after `SEE` to form the flag.

### Flag

```text
SEE{2nd_fl4g_n33ds_RCE_g00d_luck_h4x0r}
```
