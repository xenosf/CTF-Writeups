# Quick

## Challenge Description

One of the Keepers founded an interesting website used by the GDC as a command and control (C2) system. I tried to use it, but I think I just too slow…

Target URL: `http://X.X.X.X/UMJVHRV5` [IP redacted]

---

## Solution

Visiting the URL takes us to a GDC page, where we have to enter in the hash of the provided beacon, which changes every time the page reloads.

However, when doing it manually, it tells us we are too slow.

To automate this process, I had to write a script. Since the beacon is randomised for every page load, I used Selenium (which I was introduced to during [CYS](/CYS%202021)). Prior to this, I investigated the page using inspect element and found that the beacon was in a `<h3>` element.

```python
from selenium import webdriver
import hashlib
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support.expected_conditions import presence_of_element_located

driver = webdriver.Firefox()
wait = WebDriverWait(driver, 10)
driver.get("http://X.X.X.X/UMJVHRV5/")
first_result = wait.until(presence_of_element_located((By.CSS_SELECTOR, "h3")))
beacon = first_result.get_attribute("textContent")
print(beacon)
encoded=beacon.encode()
result = hashlib.sha256(encoded).hexdigest()
driver.find_element_by_name("hash").send_keys(result)
driver.find_element_by_name("execute").click()
```

<img width="1392" alt="Screenshot 2021-06-24 at 18 50 31" src="https://user-images.githubusercontent.com/40383042/126438796-ef360a73-6ce2-4e6d-b4dd-ce2aed667268.png">

### Flag

```text
CDDC21{!t_wAs-S0_fasT!}
```
