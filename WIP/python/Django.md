# Django
Python web framework, generating html/css from python.

---

### Table of Contents

- [Project](#project)
  - [New project](#new-project)
  - [Session table](#session-table)
- [Applications](#applications)
  - [Create new application](#create-new-application)
  - [Install application](#install-application)
  - [`views.py`](#viewspy)
    - [Redirect to other paths](#redirect-to-other-paths)
  - [`urls.py`](#urlspy)
  - [Templates](#templates)
    - [Forms](#forms)
  - [Static files](#static-files)
  - [Template inheritance](#template-inheritance)
- [HTTP](#http)
  - [Requests](#requests)
  - [Responses](#responses)
    - [Status Codes](#status-codes)

---

## Project

### New project
```
django-admin startproject PROJECT_NAME
cd PROJECT_NAME
python3 manage.py runserver
```

### Session table
In order to create django session tables, we have to:
```
python3 manage.py migrate
```

---

## Applications
A Django project can have multiple applications.
### Create new application
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
    # Session info
    sessionId = request.session.session_key
    return HttpResponse("Hello, world!")

def greet(request, name):
    return render(request, "hello/greet.html", {
        "name": name.capitalize()
    })
```

#### Redirect to other paths
```python
from django.http import HttpResponseRedirect
from django.urls import reverse

def i_redirect(request):
    my_name = 'hello'
    # Omit `kwargs` if no arguments needed
    return HttpResponseRedirect(reverse("tasks:index", kwargs={"name": my_name}))
```

### `urls.py`
```python
from django.urls import path
from . import views

app_name = "hello"
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

### Templates
Best practice: always have a template directory like: `APP_NAME/templates/APP_NAME/`.
```django
<!DOCTYPE html>
<html lang="en">
     <head>
        <title>Is it New Year's?</title>
    </head>
    <body>$$
        <!-- if - else -->
        {% if newyear %}
            <h1>YES</h1>
        {% else %}
           <h1>NO</h1>
        {% endif %}

        <!-- string interpolation -->
        <h2>Hello {{ name }}!</h2>

        <!-- looping through list -->
        {% for task in tasks %}
            <div>{{ task }}</div>
        {% empty %}
            <li>No tasks.</li>
        {% endfor %}

        <!-- links inside project, where 'add' is the name of path -->
        <a href="{% url 'add' %}">Add a New Task</a>

        <!-- link with app name, useful for colliding path names -->
        <a href="{% url 'hello:index' %}">Hello</a>
    </body>
</html>
```

#### Forms
Django has built in form capabilities, with validation features.
```python
# views.py
from django import forms
class NewTaskForm(forms.Form):
    task = forms.CharField(label="New Task")
    priority = forms.IntegerField(label="Priority", min_value=1, max_value=10)

def add(request):
    if request.method == "POST":
        form = NewTaskForm(request.POST)
        if form.is_valid():
            task = form.cleaned_data["task"]
            tasks.append(task)
        else:
            return render(request, "tasks/add.html", {
                "form": form
            })

    return render(request, "tasks/add.html", {
        "form": NewTaskForm()
    })
```
```django
<!-- action to 'add' view, with method POST -->
<form action="{% url 'tasks:add' %}" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit">
</form>
```


### Static files
Best practice: always have a static directory like: `APP_NAME/static/APP_NAME`.
```django
{% load static %} <!-- load static files, like css -->
<!DOCTYPE html>
<html lang="en">
     <head>
        <title>Using static css file</title>
        <link href="{% static 'newyear/styles.css' %}" rel="stylesheet">
    </head>
</html>
```
```django
{% extends "tasks/layout.html" %}

{% block body %}
<h1>Tasks</h1>
<ul>
    {% for task in tasks %}
        <li>{{ task }}</li>
    {% endfor %}
</ul>
{% endblock %}
```

### Template inheritance

```django
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Tasks</title>
    </head>
    <body>
        <!-- similar to vue slots, "body" is just an arbitrary name -->
        {% block body %}
        {% endblock %}
    </body>
</html>
```

---

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

