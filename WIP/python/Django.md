- [Django](#django)
  - [New project](#new-project)
    - [Applications](#applications)
      - [Create new application](#create-new-application)
    - [Install application](#install-application)
    - [`views.py`](#viewspy)
    - [`urls.py`](#urlspy)
    - [static files](#static-files)
    - [templates](#templates)
    - [static files](#static-files-1)
  - [HTTP](#http)
    - [Requests](#requests)
    - [Responses](#responses)
      - [Status Codes](#status-codes)

# Django
Python web framework, generating html/css from python.

## New project
```
django-admin startproject PROJECT_NAME
cd PROJECT_NAME
python3 manage.py runserver
```

### Applications
A Django project can have multiple applications.
#### Create new application
```
python3 manage.py startapp APP_NAME
```
### Install application
Add `APP_NAME` to `INSTALLED_APPS` in `settings.py`.

### `views.py`
```python
from django.http import HttpResponse # important!
from django.shortcuts import render

# Create your views here.
# Every view is a custom function.
def index(request):
    return HttpResponse("Hello, world!")

def greet(request, name):
    return render(request, "hello/greet.html", {
        "name": name.capitalize()
    })
```

### `urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"), # url="" view=index
    path("<str:name>", views.greet, name="greet")
]
```
Update the project `urls.py` file with application urls:
```python
# project urls.py
from django.urls import include, path

urlpatterns = [
    path('hello/', include('hello.urls'))
]
```

### static files
Best practice: always have a template directory like: `APP_NAME/static/APP_NAME/`.

### templates
Best practice: always have a template directory like: `APP_NAME/templates/APP_NAME/`.
```django
{% load static %}
<!DOCTYPE html>
<html lang="en">
        {% if newyear %}
            <h1>YES</h1>
        {% else %}
     <head>
        <title>Is it New Year's?</title>
        <link href="{% static 'newyear/styles.css' %}" rel="stylesheet">
    </head>
    <body>
           <h1>NO</h1>
        {% endif %}
        <h2>Hello {{ name }}!</h2>
    </body>
</html>
```

### static files


## HTTP

### Requests
```
GET / HTTP/1.1
Host: www.example.com
```

### Responses
```
HTTP/1.1 200 OK
Content-Type: text/html
```

#### Status Codes
|Code|Description|
|-|-|
|200|OK|
|301|Moved Permanently|
|403|Forbidden|
|404|Not Found|
|500|Internal Server Error|

