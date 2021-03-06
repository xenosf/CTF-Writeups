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

```text
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

![Screenshot of webpage with text field to enter URL to visit](https://user-images.githubusercontent.com/40383042/173179714-2b1f7513-8ea3-4ce0-817c-f7cdc52af0cb.png)

Viewing the provided source, we can see that this app is a Flask server. We can see that there is a route `/flag` that shows the flag, only if the request remote address is `127.0.0.1` (localhost).

```py
@app.route('/flag')
def flag():
    if request.remote_addr == '127.0.0.1':
        return render_template('flag.html', FLAG=os.environ.get("FLAG"))

    else:
        return render_template('forbidden.html'), 403
```

To get the flag, we must visit `/flag` using the proxy so that the request comes from the server itself (thus localhost).

However, entering `http://127.0.0.1/flag` or `http://localhost/flag` into the address bar of the page gets blocked:

![Screenshot of page with error after visiting localhost](https://user-images.githubusercontent.com/40383042/173179733-e3799023-6e09-4600-b5b6-4b4e767c8da0.png)

Examining the source code further, we can see that it uses a library called `advocate`. When making a request, it visits the URL using `advocate` first, which checks if the URL leads to the local machine. If not, it makes the request using the normal `requests` module and displays the page.

```py
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
```

Since it makes two requests, if the server response from the same domain changes in between the two requests, then we can bypass this SSRF prevention.

To change the server response halfway, we can use a [DNS rebinding attack](https://highon.coffee/blog/ssrf-cheat-sheet/#dns-rebinding-attempts).

DNS (Domain Name System) servers help tell browsers which IP address a domain (for example, `blog.xenosf.io`) points to.

![Diagram of how DNS works](https://user-images.githubusercontent.com/40383042/173181572-3f650c8a-8ffb-48f7-944d-50bef40ea44b.png)

By changing the IP address for a particular domain, we can change where a particular domain leads to. The DNS server can switch between IP addresses extremely quickly, which allows us to bypass certain barriers, such as the SSRF prevention check in this challenge.

![Diagram of how DNS works with 2 cases](https://user-images.githubusercontent.com/40383042/173181577-cd592ad5-8cab-479a-93a6-3c403baee0d6.png)

There are multiple ways to do this, but we can use an [online DNS rebinding service](https://lock.cmpxchg8b.com/rebinder.html). Set one IP to `127.0.0.1` (localhost), and the other one to an arbitrary website's IP address.

<img width="1196" alt="Screenshot of online DNS rebinding service with IP addresses filled in" src="https://user-images.githubusercontent.com/40383042/173179873-d858cd40-7ac5-4ea5-9974-c5e25e00acff.png">

We can then enter the URL `http://[url]/flag` into the challenge page, and try a few times to get the timing right and get the flag.

This is what happens when the domain points to localhost when the first request is done:

![Screenshot of challenge webpage with error upon trying to visit localhost](https://user-images.githubusercontent.com/40383042/173179769-79d6195b-cc6a-45c5-bc2d-41d9e65899b0.png)

This is what happens when the domain points to the other IP in both the first and second request (passing the SSRF check but not showing the flag page):

![Screenshot of challenge webpage showing the wrong website](https://user-images.githubusercontent.com/40383042/173179773-922d69de-6798-40eb-bff9-677dd7c33cdd.png)

And this is what happens when the domain points to the other IP in the first request (passing the SSRF check) and switches to the localhost page in the second request, showing the flag page:

![Screenshot of challenge page with the flag](https://user-images.githubusercontent.com/40383042/173179772-b5a74459-9a5c-4322-9e55-cccfc05c6336.png)

### Flag

```text
SEE{y0u_m34n_7h3_53rv3r_r35p0n53_c4n_ch4n63?_369e7e3531c987fa4a4c9cfd4f97f2f6}
```
