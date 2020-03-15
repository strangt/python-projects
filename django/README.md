# Python Django with indez.html and style.css

## Install Django into a virtual environment

### Install pip
```
pip3 --version
python3 -m ensurepip --default-pip
```

### Install venv
```
sudo -H pip3 install virtualenv
```

### Create the virtual env
```
python3 -m virtualenv venv
```

### switch to the virtual environment
```
. ./venv/bin/activate
```

### install Django into the virtual environment
```
pip3 install Django
python3 -m django --version
3.0.4
```

## Create Django project and app

>   What’s the difference between a project and an app?
>
>   An app is a Web application that does something – e.g., a Weblog system,
>   a database of public records or a small poll app. An app can be in
>   multiple projects.
>
>   A project is a collection of configuration and apps for a particular
>   website. A project can contain multiple apps.

### Initialize Django project (environment)
```
django-admin.py startproject mytest
```

### update django environment
```
cd mytest
python3 manage.py migrate 
```

### Create the app (inside mytest project environment) 
```
cd mytest
python3 manage.py startapp mytestproject
```

### edit mytestproject/views.py
```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

### create mytestproject/urls.py
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

### edit mytest/mytest/urls.py
```
urlpatterns = [
    url(r'^mytestproject/', include('mytest.mytestproject.urls')),
    url(r'^admin/', admin.site.urls),
]
```

## directory structure

>   mytest 
>       db.sqlite3
>       manage.py
>       mytest
>           __init__.py
>           settings.py
>           urls.py
>           wsgi.py
>       mytestproject
>           __init__.py
>           admin.py
>           apps.py
>           migrations
>               __init__.py
>           models.py
>           tests.py
>           urls.py
>           views.py

## Run the server

### switch to the virtual environment if you are not already in
```. ./venv/bin/activate

cd mytest
python3 manage.py runserver
```

## Load index.html and style.css

### edit mytestproject/views.py
```# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
    # main objects we will pass to the page
    my_title = 'Conversion'

    context = {
        'my_title': my_title,
    }

    # Render the HTML template index.html with the data in the context variable
    return render(request, 'index.html', context=context)
```

### edit mytest/settings.py (TEMPLATES and STATICFILES)
```...
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'mytestproject/templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
...
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/1.11/howto/static-files/

STATIC_URL = '/static/'

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'mytestproject/static'),
]
```

### edit mytestproject/templates/index.html
```{% load static %}
<!DOCTYPE html>
<html>
    <head>
        <title>Example</title>
        <link rel="stylesheet" type="text/css" href="{% static 'stylesheets/style.css' %}">
    </head>
    <body>
        <p><strong>Title:</strong> {{ my_title }}</p>
    </body>
</html>
```

### edit mytestproject/static/stylesheets/style.css
```body {
  padding: 50px;
  font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
}

p {
  color: #00B7FF;
}
```
