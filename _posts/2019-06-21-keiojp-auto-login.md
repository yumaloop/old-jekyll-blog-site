---
layout: post
title: Automatically Login to keio.jp using Selenium on Python
categories:
    - Tips
tags:
    - hoge
    - foo
---

Have you ever want to login to keio.jp automatically? Don't you think it is cool? At least I think so and I write down the way to achieve that with Python.



<br>

<div style="text-align:center">
  <video style="width: 90%;" controls autoplay loop muted>
    <source src="{{ "/assets/video/keio_login.mp4" | relative_url }}" type="video/mp4">
    <p>Your browser does not support the video tag.</p>
  </video>
</div>


<br>



In order to login to keio.jp (Keio Single Sign-On System), it is necessary to satisfy the page transition as below. 

1. https://auth.keio.jp (SSO login page)

2. https://gslbs.adst.keio.ac.jp/student/index.html (Syllabus page)

3. https://www.edu.keio.jp/ess2/login? (Class support page)

So, this time, a static web-scraping library like BeautifulSoup is not enough, because it doesn't support the dynamic site with Javascript or page redirection. Then I use [Selenium](https://selenium.dev/documentation/en/) and [ChromeDriver](http://chromedriver.chromium.org/getting-started) in python.



<br>




#### Example 1 : Auto login to keio.jp

If you are the student in Keio University, you can login to keio.jp automatically. All you need is to assign your email address and password in the below script and run it in command line.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

ID = "*****" # your email in keio.jp (ex. example@keio.jp)  #
PW = "*****" # your password in keio.jp #

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



<br>



#### Example 2 : Auto login to twitter.com

If you have twitter account, you can also login to twitter.com automatically. All you need is to assign your username (@\*\*\*\*\*\*\*\*) and password (\*\*\*\*\*\*\*\*) in the below script and run it in command line.


```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

USERNAME="*****" # your username in twitter #
PASSWORD="*****" # your password in twitter #

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



<br>



#### Example 3 : Auto search in google.com

If you refer to Selenium Getting started guide, you can aquire the search result with the keyword "cheese" at google.com with Selenium on Python. (This requires Selenium WebDriver 3.13 or newer.)

```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support.expected_conditions import presence_of_element_located

# Open web driver (Google Chrome)
driver = webdriver.Firefox()

wait = WebDriverWait(driver, 10) 

# GET HTML page source of google.com
driver.get("https://google.com/ncr") # GET 
# POST the keyword "cheese" in "q" element in google.com
driver.find_element_by_name("q").send_keys("cheese" + Keys.RETURN) # POST 

first_result = wait.until(presence_of_element_located((By.CSS_SELECTOR, "h3>div")))

# Search result as text
print(first_result.get_attribute("textContent"))

# Close web driver (Google Chrome)
driver.quit()
```



<br>



### References

- The Selenium Browser Automation Project > Getting started > Quick tour

  https://selenium.dev/documentation/en/getting_started/quick/#webdriver



