---
layout: post
title: Automatically Login to keio.jp using Selenium on Python
categories:
    - Tips
tags:
    - hoge
    - foo
---

Have you ever want to login to keio.jp automatically? Don't you think it is cool? At least I think so and I write its Python code here.

<div style="text-align:center">
  <video style="width: 90%;" controls autoplay loop muted>
    <source src="{{ "/assets/video/keio_login.mp4" | relative_url }}" type="video/mp4">
    <p>Your browser does not support the video tag.</p>
  </video>
</div>

<br>

# Auto Login to keio.jp

```python
import os
import sys
from pprint import pprint
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

ID = # Add your email in keio.jp (ex. example@keio.jp) #
PW = # Add your password in keio.jp #

# Optional settings of chrome driver
options = webdriver.ChromeOptions()
options.add_argument('--headless')

# Boot chrome driver
driver = webdriver.Chrome("/usr/local/bin/chromedriver", options=options)
driver.set_page_load_timeout(15) # Time out 15 sec

# GET (HTML Page)
driver.get("https://auth.keio.jp")

# Find elements and POST (send keys to the input tag)
id_element = driver.find_element_by_name("j_username")
id_element.send_keys(ID)
pw_element = driver.find_element_by_name("j_password")
pw_element.send_keys(PW)

# Click login button
login_button = driver.find_element_by_name("_eventId_proceed")
login_button.click()

# GET (HTML Page)
driver.get("https://gslbs.adst.keio.ac.jp/student/index.html")

# GET (HTML Page)
driver.get("https://www.edu.keio.jp/ess2/login?")

# Close chrome driver
driver.quit()
```


# Auto Login to twitter.com

ちなみに，twitterの自動ログインも同様にできる．



```python
import os
import sys
from pprint import pprint
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

USERNAME=# Add your username in twitter #
PASSWORD=# Add your password in twitter #

# Optional settings of chrome driver
options = webdriver.ChromeOptions()
options.add_argument('--headless')

# Boot chrome driver
driver = webdriver.Chrome("/usr/local/bin/chromedriver", options=options)
driver.set_page_load_timeout(15) # Time out 15 sec

# GET (HTML Page)
driver.get("https://twitter.com/login")

# Find elements and POST (send keys to the input tag)
username_element = driver.find_element_by_class_name('js-username-field')
username_element.send_keys(USERNAME)
password_element = driver.find_element_by_class_name('js-password-field')
password_element.send_keys(PASSWORD)

# Click login button
login_button = driver.find_element_by_css_selector('button.submit.EdgeButton.EdgeButton--primary.EdgeButtom--medium')
login_button.click()

# Close chrome driver
driver.quit()
```
