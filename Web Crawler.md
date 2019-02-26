# Web Crawler

Simple python 3 code examples about crawler.

- [Simulate login process with *selenium*](#simulate-login-process-with-*selenium*)
- [Save cookies](#save-cookies)
- [Get a html page content with *requests* or *urllib.request*](#get-a-html-page-content-with-*requests*-or-*urllib.request*)
- [Extract useful information using *BeautifulSoup*](#extract-useful-information-using-*beautifulsoup*)

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

## Save cookies

You can save cookies to a file:

```python
import pickle

#login here

pickle.dump(driver.get_cookies(), open("cookies.pkl", "wb"))
```

and load saved cookies from a file:

```python
import pickle
import selenium.webdriver

driver = selenium.webdriver.Safari()
driver.get(url)

cookies = pickle.load(open("cookies.pkl", "rb"))
for cookie in cookies:
    driver.add_cookie(cookie)
```

If you want to load them on a requests session for web crawling:

```python
import pickle
from requests import Session

req = Session()
req.headers.clear()

cookies = pickle.load(open("cookies.pkl", "rb"))
for cookie in cookies:
    req.cookies.set(cookie['name'], cookie['value'])
```

If you don't have to save cookies to file, just ignore lines about pickle.

## Get a html page content with *requests* or *urllib.request*

To process a html page using BeautifulSoup we'll need those information in type *bytes*.

Using *urllib.request*:

```python
import urllib.request

url = 'a-link'
urlrequest = urllib.request.Request(url)
response = urllib.request.urlopen(urlrequest)
html = response.read() #<class 'bytes'>
```

Using *requests*:

```python
#if loading cookies
from requests import Session

url = 'a-link'
req = Session()
html = req.get(url).content #<class 'bytes'>
```

```python
#simple session case
import requests

url = 'a-link'
req = requests.get(url)
html = req.content #<class 'bytes'>
```

## Extract useful information using *BeautifulSoup*

```python
from bs4 import BeautifulSoup #obviously

soup = BeautifulSoup(html, 'html.parser') #html a bytes type
```

Here are some of the most used BeautifulSoup methods:

**`soup.find_all('img')`** returns a bs4 set of all elements containing the tag 'img', which means all the images.

**`div_list = soup.find('div', attrs={'class': 'classname'})`** 

**`tables = soup.select('table')`**

These bs4 set elements are individually accessible using `set_name[index]`.

```python
imgs_link_list = []
id_list = []
name_list = []
value_list = []

imgs = soup.find_all('img')
#Obtain actual image link
domain_addr = "www.example.com"
for img in imgs:
    img['src'] = domain_addr + img['src']
	imgs_link_list.append(img['src'])
    
#html where in a table there are 4 columns: id, img, name, and value
tables = soup.select('table')
for table in tables:
    for row in table.select('tr'):
        id_list.append(row.select('td')[0].get_text())
        #img_list already taken care of
        name_list.append(row.select('td')[2].get_text())
        value_list.append(row.select('td')[3].get_text())
```

I think `.select()` moves the file cursor to the next given tag returning a `list` while `.find()` and `.find_all() ` return a bs4 set which can be selected again.