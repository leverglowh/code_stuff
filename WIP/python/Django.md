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

