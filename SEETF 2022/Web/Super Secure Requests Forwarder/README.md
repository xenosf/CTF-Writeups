# 🧑‍🎓 Super Secure Requests Forwarder

**Category**: Web

100 points | 74 solves

----

## Challenge Description

Hide your IP address and take back control of your privacy! Visit websites through our super secure proxy.

`http://ssrf.chall.seetf.sg:1337`

For beginners:

* [https://portswigger.net/web-security/ssrf](https://portswigger.net/web-security/ssrf)

## Attached files/Instructions

* source code of web server

```sh
$ tree 
distrib
  ├── Dockerfile
  ├── app.py
  ├── docker-compose.yml
  ├── requirements.txt
  ├── static
  │   └── main.css
  └── templates
      ├── flag.html
      ├── forbidden.html
      └── index.html
```

----

## Solution

Visiting the provided URL takes us to a page with a text field to enter a URL to visit.

Viewing the provided source, we can see that this app is a Flask server:

```py
from flask import Flask, request, render_template
import os
import advocate
import requests

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():

    if request.method == 'POST':
        url = request.form['url']

        # Prevent SSRF
        try:
            advocate.get(url)

        except:
            return render_template('index.html', error=f"The URL you entered is dangerous and not allowed.")

        r = requests.get(url)
        return render_template('index.html', result=r.text)

    return render_template('index.html')


@app.route('/flag')
def flag():
    if request.remote_addr == '127.0.0.1':
        return render_template('flag.html', FLAG=os.environ.get("FLAG"))

    else:
        return render_template('forbidden.html'), 403


if __name__ == '__main__':
    app.run(host="0.0.0.0", port=80, threaded=True)
```

We can see that there is an endpoint `/flag` that shows the flag, only if the request remote address is `127.0.0.1` (localhost).

To get the flag, we must visit `/flag` using the proxy.

However, entering `http://127.0.0.1/flag` or `http://localhost/flag` into the address bar of the page gets blocked:

<!--screenshot-->

Examining the source code further, we can see that it uses a library called `advocate`. When making a request, it visits the URL using `advocate` first, which checks if the URL leads to the local machine. If not, it makes the request using the normal `requests` module and displays the page.

Since it makes two requests, if the server response from the same domain changes in between the two requests, then we can bypass this SSRF prevention.

To change the server response halfway, we can use a [DNS rebinding attack](https://highon.coffee/blog/ssrf-cheat-sheet/#dns-rebinding-attempts).

DNS helps tell browsers which IP address a domain (for example, `blog.xenosf.io`) points to. By changing the IP address for a particular domain, we can change where a particular domain leads to. By doing this extremely quickly, we can bypass certain barriers, such as the SSRF prevention check in this challenge.

There are multiple ways to do this, but we can use an [online DNS rebinding service](https://lock.cmpxchg8b.com/rebinder.html). Set one IP to `127.0.0.1` (localhost), and the other one to an arbitrary website's IP address. I used the IP of `google.com`.

We can then enter the URL into the challenge page, and try a few times to get the timing right and get the flag.

<!--screenshots-->

### Flag

```text
SEE{y0u_m34n_7h3_53rv3r_r35p0n53_c4n_ch4n63?_369e7e3531c987fa4a4c9cfd4f97f2f6}
```
