# 🧑‍🎓 Sourceless Guessy Web (Baby Flag)

**Category**: Web

100 points | 435 solves

----

## Challenge Description

WHY SO SERIOUS? WHY NEED SOURCE?

Contrary to the title of this challenge, you do not need to guess. As usual, do not bruteforce or scan our infrastructure, it is not allowed.

Note: There are two flags for this challenge.

`http://sourcelessguessyweb.chall.seetf.sg:1337`

For beginners:

* [https://portswigger.net/web-security/file-path-traversal](https://portswigger.net/web-security/file-path-traversal)

----

## Solution

Opening the link to the website provided takes us to a page with some Batman-themed text art.

![Screenshot of webpage with text art in the shape of the Batman logo](https://user-images.githubusercontent.com/40383042/173172808-d07e7cb5-2050-4bf6-920d-7687c594854b.png)

A hyperlink labelled "WHY NEED SOURCE?" is present on the page. Clicking it takes us to the PHP info page.

![Screenshot of phpinfo page](https://user-images.githubusercontent.com/40383042/173172839-1a09c778-83f0-4d65-b3e8-c4efeaf55155.png)

Under `http_referer`, we find `http://sourcelessguessyweb.chall.seetf.sg:1337/?page=whysoserious`.

![Screenshot of phpinfo page with http_referer field](https://user-images.githubusercontent.com/40383042/173172812-a862748e-d612-4b73-80ad-95eb299a4728.png)

Visiting that URL, we end up at the original page. This seems vulnerable to a directory traversal attack; we can try to access `/etc/passwd` by setting `page` to something like `../../../../etc/passwd`. After doing this, the text in the Batman text art changes to the contents of `/etc/passwd`:

![Screenshot of web page containing the contents of /etc/passwd](https://user-images.githubusercontent.com/40383042/173172821-f8f459ef-be3f-479e-b0f4-b0eca335ce8c.png)

![Screenshot of web page containing the contents of /etc/passwd with text highlighted to show the white text](https://user-images.githubusercontent.com/40383042/173172916-0c60f890-d581-4f17-a78e-0f771909e9b6.png)

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
