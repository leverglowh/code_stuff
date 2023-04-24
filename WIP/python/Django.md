# Django
Python web framework, generating html/css from python.

---

### Table of Contents

- [Project](#project)
  - [New project](#new-project)
  - [Session table](#session-table)
  - [Python shell](#python-shell)
    - [Manipulate database](#manipulate-database)
- [Applications](#applications)
  - [Admin APP](#admin-app)
    - [Create superuser](#create-superuser)
    - [Register models in the admin app](#register-models-in-the-admin-app)
  - [Create new application](#create-new-application)
  - [Install application](#install-application)
  - [`views.py`](#viewspy)
    - [Redirect to other paths](#redirect-to-other-paths)
  - [`urls.py`](#urlspy)
  - [`models.py`](#modelspy)
  - [Templates](#templates)
    - [Forms](#forms)
  - [Static files](#static-files)
  - [Template inheritance](#template-inheritance)
  - [Tests](#tests)
    - [Selenium](#selenium)
      - [In Django](#in-django)
- [HTTP](#http)
  - [Requests](#requests)
  - [Responses](#responses)
    - [Status Codes](#status-codes)
- [CI/CD](#cicd)
  - [Github Actions](#github-actions)
  - [Docker](#docker)
    - [Docker Compose](#docker-compose)
    - [Other Docker Commands](#other-docker-commands)

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

### Python shell
To open Django's shell where I can write python commands on this project:
```
python3 manage.py shell
```

#### Manipulate database
```
python3 manage.py shell
```
```python
from flights.models import *
f = Flight(origin="New York", destination="London", duration=415)
f.save() # I have now created a row in the Flight table.

# Get rows
flights = Flight.objects.all()
flight = flights.first()

# Filter
new_york_airports = Airport.objects.filter(city="New York")

# All passengers that are not on the `flight`
non_passengers = Passenger.objects.exclude(flights=flight).all()

# where passengers is a related name in a many to many relationship
passengers = flight.passengers.all()

# Get the one row, throws error if more than one are found
the_ny_airport = Airport.objects.get(city="New York")
the_london_airport = Airport.objects.get(city="London")

# Add row with references to foreign table rows
f1 = Flight(origin=the_ny_airport, destination=the_london_airport, duration=435)

# Delete row
flight.delete()
```

---

## Applications
A Django project can have multiple applications.

### Admin APP

#### Create superuser
```
python3 manage.py createsuperuser
<!-- enter username -->
<!-- enter email -->
<!-- enter password, twice -->
```

#### Register models in the admin app
```python
# admin.py
from django.contrib import admin

from .models import Flight, Airport, Passenger

# Register your models here.
class FlightAdmin(admin.ModelAdmin):
    list_display = ("id", "origin", "destination", "duration")

class PassengerAdmin(admin.ModelAdmin):
    filter_horizontal = ("flights",)

admin.site.register(Airport)
admin.site.register(Flight, FlightAdmin)
admin.site.register(Passenger, PassengerAdmin)
```

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

### `models.py`
In every application I can define a number of data models.
```python
from django.db import models

# Create your models here.
class Airport(models.Model):
    code = models.CharField(max_length=3)
    city = models.CharField(max_length=64)

    def __str__(self):
        return f"{self.city} ({self.code})"


class Flight(models.Model):
    # Airport 1 - N Flights
    # Airport has many flights, a flight only has one origin airport.
    origin = models.ForeignKey(Airport, on_delete=models.CASCADE, related_name="departures")
    destination = models.ForeignKey(Airport, on_delete=models.CASCADE, related_name="arrivals")
    duration = models.IntegerField()

    def __str__(self):
        return f"{self.id}: {self.origin} to {self.destination}."

class Passenger(models.Model):
    first = models.CharField(max_length=64)
    last = models.CharField(max_length=64)
    # Flights N - M Passengers
    # Many to many
    flights = models.ManyToManyField(Flight, blank=True, related_name="passengers")

    def __str__(self):
        return f"{self.first} {self.last}"
```
For this data model to be included and processed by DB (migration), I have to type this command in terminal:
```
python3 manage.py makemigrations
<!-- 
Migrations for 'flights':
  flights/migrations/0001_initial.py
    - Create model Flight
-->
```

To apply the migration:
```
python3 manage.py migrate
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

### Tests

`python manage.py test` will run all tests in the project.

```python
# tests.py
from django.test import TestCase
from .models import Flight, Airport, Passenger

# Create your tests here.
class FlightTestCase(TestCase):

    def setUp(self):
        a1 = Airport.objects.create(code="AAA", city="City A")
        a2 = Airport.objects.create(code="BBB", city="City B")

        Flight.objects.create(origin=a1, destination=a2, duration=100)
        Flight.objects.create(origin=a1, destination=a1, duration=200)
        Flight.objects.create(origin=a1, destination=a2, duration=-100)
    
    def test_departures_count(self):
        a = Airport.objects.get(code="AAA")
        self.assertEqual(a.departures.count(), 3)
    
    def test_arrivals_count(self):
        a = Airport.objects.get(code="AAA")
        self.assertEqual(a.departures.count(), 1)

    def test_valid_flight(self):
        a = Airport.objects.get(code="AAA")
        b = Airport.objects.get(code="BBB")
        f = Flight.objects.get(origin=a, destination=b, duration=100)
        self.assertTrue(f.is_valid_flight())

    def test_invalid_flight_destination(self):
        a = Airport.objects.get(code="AAA")
        f = Flight.objects.get(origin=a, destination=a)
        self.assertFalse(f.is_valid_flight())

    def test_invalid_flight_duration(self):
        a = Airport.objects.get(code="AAA")
        b = Airport.objects.get(code="BBB")
        f = Flight.objects.get(origin=a, destination=b, duration=-100)
        self.assertFalse(f.is_valid_flight())

    # test client

    def test_index(self):
        c = Client()
        response = c.get("/flights/")
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.context["flights"].count(), 3)

    def test_valid_flight_page(self):
        a = Airport.objects.get(code="AAA")
        f = Flight.objects.get(origin=a, destination=a)
        c = Client()
        response = c.get(f"/flights/{f.id}")
        self.assertEqual(response.status_code, 200)
        
    def test_invalid_flight_page(self):
        max_id = Flight.objects.all().aggregate(Max("id"))["id__max"]
        c = Client()
        response = c.get(f"/flights/{max_id + 1}")
        self.assertEqual(response.status_code, 404)
```

#### Selenium
Selenium is a tool for testing web applications. It can be used to automate actions in a browser, like clicking buttons, filling out forms, and checking returned values.

```python
from tests import *
uri = file_uri("index.html")
driver.get(uri)
driver.title # page title
driver.page_source # page source
increase = driver.find_element_id("increase")
increase.click()
```

##### In Django
```python
import os
import pathlib
import unittest

from selenium import webdriver

def file_uri(filename):
    return pathlib.Path(os.path.abspath(filename)).as_uri()

driver = webdriver.Chrome()

class WebpageTests(unittest.TestCase):

    def test_title(self):
        driver.get(file_uri("counter.html"))
        self.assertEqual(driver.title, "Hello, world!")

    def test_increase(self):
        driver.get(file_uri("counter.html"))
        increase = driver.find_element_by_id("increase")
        increase.click()
        self.assertEqual(driver.find_element_by_tag_name("h1").text, "1")
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

---

## CI/CD
- CI: Continuous Integration
  - Frequent merges to main branch
  - Automated unit testing
- CD: Continuous Delivery
  - Short release schedules

### Github Actions
Used to accomplish CI.
Actions are small programs that can be run directly in your repository. They are written in YAML and can be triggered by events, like a push or a pull request.

```yaml
# syntax:
# key: value
# key:
#   - value
#   - value

# .github/workflows/ci.yml
name: Testing
on: push

jobs:
    test_project:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v2
            - name: Run Django unit tests
                run: |
                    pip3 install --user django
                    python3 manage.py test
```

### Docker
Docker is a tool that allows you to create, deploy, and run applications by using containers. A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

```
# Dockerfile
FROM python:3
COPY . /usr/src/app
WORKDIR /usr/src/app
RUN pip install -r requirements.txt
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
```

#### Docker Compose
I use it so I can run multiple containers at once. For example, I can run a database and a web server at the same time.

`docker-compose up`

```yaml
# docker-compose.yml
version: "3"
services:
    db:
        image: postgres
    
    web:
        build: .
        volumes:
            - .:/usr/src/app
        ports:
            - "8000:8000"
```

#### Other Docker Commands
- `docker ps` - list running containers
- `docker exec -it <container_id> bash` - run bash in container
  - `ctrl + d` to exit
- `docker build -t <image_name> .` - build image