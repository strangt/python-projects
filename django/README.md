# Python Django with index.html and style.css

## Install Django into a virtual environment

### Install pip (if pip3 returns 'command not found')
```
pip3 --version
python3 -m ensurepip --default-pip
```

### Install venv (if virtualenv is not a installed package)
```
pip3 list
sudo -H pip3 install virtualenv
```

### Create the virtual env
```
python3 -m virtualenv venv
```

### Switch to the virtual environment
```
. ./venv/bin/activate
```

### Install Django into the virtual environment
```
pip3 install Django
python3 -m django --version
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

### Change to the project folder
```
cd mytest
```

### Update django environment
```
python3 manage.py migrate 
```

### Create the app (sub-folder to the project folder)
```
python3 manage.py startapp mytestproject
```

### Edit mytestproject/views.py
```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world!")
```

### Create mytestproject/urls.py
```
from django.contrib import admin
from django.urls import include, path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

### Edit mytest/urls.py (modify urlpatterns)
```
...
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url('', include('mytestproject.urls')),
    url('admin/', admin.site.urls),
]
```

## Run the server
```
# switch to the virtual environment if you are not already in
. ./venv/bin/activate
# make sure you are in the project folder
cd mytest
# run the server
python3 manage.py runserver
```

http://127.0.0.1:8000/ should display
> 'Hello, world!'

## Load index.html and style.css

### Edit mytestproject/views.py
```
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

### Edit mytest/settings.py (TEMPLATES and STATICFILES)
```
...
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
STATIC_URL = '/static/'

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'mytestproject/static'),
]
```

### Edit mytestproject/templates/index.html
```
{% load static %}
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

### Edit mytestproject/static/stylesheets/style.css
```
body {
  padding: 50px;
  font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
}

p {
  color: #00B7FF;
}
```

## Folder structure should look like this
```
mytest/
├── db.sqlite3
├── manage.py
├── mytest
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── mytestproject
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── static
    │   └── stylesheets
    │       └── style.css
    ├── templates
    │   └── index.html
    ├── tests.py
    ├── urls.py
    └── views.py
```
