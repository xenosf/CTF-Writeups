# DevSecOps Scenario II

## Challenge Description
Testing automation with Selenium can improve the DevOps process by taking out the required human effort and it also prevents human testing errors.

In this challenge, the user has been provided access to VSCode IDE (with selenium and selenium python binding installed on it). The user can observe the browser automation in action by checking the output Firefox window and the target webapp can also be inspected by clicking the webapp link. The target webapp can also be accessed on the domain name "webapp" (from VSCode machine).

**Objective:** Write a script to automate the dictionary attack on the webapp!

## Provided Files
* template-code.py
* password-list.txt
### template-code.py
```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import os

os.environ["DISPLAY"] = ":10"

target_url = "https://"

driver = webdriver.Firefox()
driver.get(target_url)
```

---

## Solution

> **Note:** This is not the full solution.
> 
> Also, I forgot to take screenshots for this or copy anything in the lab itself until the end, so I couldn't add them here. 

Running the lab opens up a page with links to a VS Code instance which is running locally on the lab machine, the web app, as well as a page where we can view the desktop.

We have to find a way to log in, which is likely through the administrative panel.

Poking around on the web app, we can see that it uses Wolf CMS.

Now that we know what CMS it uses, we can find out that it adds ? before page names by default. This means that the admin page can be found at `/?admin`.

I tried to enter a random password as a test, and an error message popped up that said failing to log in more than 3 times would require me to wait 30 seconds before trying again.

I opened up VSCode and tried to use provided template code to automate entering the passwords, with username `admin`. Upon running my code, the browser opened up and text showed up in the textbox, but it never managed to crack the password, even when trying different variations of "admin(istrator)" for the username. I'm not sure why it didn't work - could be the 30s delay, or the loop, or something else entirely - but the text entry & form submission seemed to work fine.

In the end, I didn't have any time left to try and get the code to work, so... I just ended up bruteforcing it manually... on the flag submission page. Afterwards, I went back to check, and the login works with username `admin`.

Would love to read a proper write-up on this!

### Flag:
```
12cfc9943bK
```