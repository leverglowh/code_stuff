# Web Crawler stuff

Simple python 3 code examples about crawler.

- [Simulate login process with *selenium*](#Simulate-log-in-process-with-*selenium*)
- Save cookies
- Get a html page content with *requests* or *urllib.request*
- Extract useful information using *BeautifulSoup*
- Format and save extracted information

<br/>

## Simulate login process with *selenium*

```
pip3 install -U selenium
```

From [Selenium's website][seleniumsite]:

> Selenium requires a driver to interface with the chosen browser. Firefox, for example, requires [geckodriver](https://github.com/mozilla/geckodriver/releases), which needs to be installed before the below examples can be run. Make sure it’s in your PATH, e. g., place it in /usr/bin or /usr/local/bin.

Here I'll be using **Safari**.

From [Apple Developer Documentation][appledevWebdriver]:

>Safari’s WebDriver support for developers is turned off by default. How you enable it depends on your operating system.
>
>**High Sierra and later:**
>
>Run ``safaridriver --enable`` once. (If you’re upgrading from a previous macOS release, you may need to use `sudo`.)

Also **"Allow Remote Automation" must be enabled** from Safari's Develope menu.

```python
from selenium import webdriver
from time import sleep

driver = webdriver.Safari()
url = 'something/to/log/into'
driver.get(url)
#terminal will now open a Safari window using webdriver

driver.find_element_by_xpath('xpath').send_keys('username')
driver.find_element_by_xpath('xpath').send_keys('password')

#if there's a captcha we'll have to input it on the terminal
captcha = input('captcha: ')
driver.find_element_by_xpath('captcha-text-box-xpath').send_keys(captcha)

sleep(5) #may be optional idk

driver.find_element_by_xpath('login-button-xpath').click()
```

To find the correct xpath we'll have to use Safari's web inspector ( `⌥+⌘+I`)  Elements tab, navigate to the correct text box (where you input the username and password) and right click it, select `Copy->XPath` , it looks something like this: "`//*[@id="username"]`".





[seleniumsite]: https://pypi.org/project/selenium/
[appledevWebdriver]: https://developer.apple.com/documentation/webkit/testing_with_webdriver_in_safari

