[W3Schools Django Tutorial Page](https://www.w3schools.com/django/index.php)

# Python Django Tutorial

## Create Virtual Environment
It is suggested to have a dedicated virtual environment for each Django project, and one way to manage a virtual environment is venv, which is included in Python. The name of the virtual environment is your choice, in this tutorial we will call it myworld. Type the following in the command prompt, remember to navigate to where you want to create your project:
```sh
python -m venv myworld
```
This will set up a virtual environment, and create a folder named "myworld" with subfolders and files. Then you have to activate the environment, by typing this command:
```sh
source myworld/bin/activate
```
Note: You must activate the virtual environment every time you open the command prompt to work on your project. It will look something like:
```sh
(myworld) ... $
```

## Install Django
Django is installed using pip, with this command:
```sh
(myworld) ... $ python -m pip install Django
```
You can check if Django is installed by asking for its version number like this:
```sh
django-admin --version
```

## Django Create Project
Once you have come up with a suitable name for your Django project, like mine: my_tennis_club, navigate to where in the file system you want to store the code (in the virtual environment), I will navigate to the myworld folder, and run this command in the command prompt:
```sh
django-admin startproject my_tennis_club
```
Django creates a my_tennis_club folder on my computer. These are all files and folders with a specific meaning, you will learn about some of them later in this tutorial, but for now, it is more important to know that this is the location of your project, and that you can start building applications in it.

Now that you have a Django project, you can run it, and see what it looks like in a browser. Navigate to the /my_tennis_club folder and execute this command in the command prompt:
```sh
python manage.py runserver
```
Open a new browser window and type 127.0.0.1:8000 in the address bar.

## Django Create App
An app is a web application that has a specific meaning in your project, like a home page, a contact form, or a members database. In this tutorial we will create an app that allows us to list and register members in a database. But first, let's just create a simple Django app that displays "Hello World!".

I will name my app members. Start by navigating to the selected location where you want to store the app, in my case the my_tennis_club folder, and run the command below.
```sh
python manage.py startapp members
```
Django creates a folder named members in my project. These are all files and folders with a specific meaning. You will learn about most of them later in this tutorial.

## Django Views
Django views are Python functions that take http requests and return http response, like HTML documents. A web page that uses Django is full of views with different tasks and missions. Views are usually put in a file called views.py located on your app's folder. Find it and open it, and replace the content with this:
```python
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```
This is a simple example on how to send a response back to the browser. But how can we execute the view? Well, we must call the view via a URL.

## Django URLs
Create a file named urls.py in the same folder as the views.py file, and type this code in it:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```
The urls.py file you just created is specific for the members application. We have to do some routing in the root directory my_tennis_club as well. This may seem complicated, but for now, just follow the instructions below. There is a file called urls.py on the my_tennis_club folder, open that file and add the include module in the import statement, and also add a path() function in the urlpatterns[] list, with arguments that will route users that comes in via 127.0.0.1:8000/. Then your file will look like this:
```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```
If the server is not running, navigate to the /my_tennis_club folder and execute this command in the command prompt:
```sh
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/members/ in the address bar.

## Django Templates
In the Django Intro page, we learned that the result should be in HTML, and it should be created in a template, so let's do that. Create a templates folder inside the members folder, and create a HTML file named myfirst.html. Open the HTML file and insert the following:
```html
<!DOCTYPE html>
<html>
<body>

<h1>Hello World!</h1>
<p>Welcome to my first Django project!</p>

</body>
</html>
```
Open the views.py file and replace the members view with this:
```python
from django.http import HttpResponse
from django.template import loader

def members(request):
  template = loader.get_template('myfirst.html')
  return HttpResponse(template.render())
```
To be able to work with more complicated stuff than "Hello World!", We have to tell Django that a new app is created. This is done in the settings.py file in the my_tennis_club folder. Look up the INSTALLED_APPS[] list and add the members app like this:
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'members'
]
```
Then run this command:
```sh
python manage.py migrate
```
Start the server by navigating to the /my_tennis_club folder and execute this command:
```sh
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/members/ in the address bar.

## Django Models
A Django model is a table in your database. Up until now in this tutorial, output has been static data from Python or HTML templates. Now we will see how Django allows us to work with data, without having to change or upload files in the process. In Django, data is created in objects, called Models, and is actually tables in a database.

To create a model, navigate to the models.py file in the /members/ folder. Open it, and add a Member table by creating a Member class, and describe the table fields in it:
```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
```
When we created the Django project, we got an empty SQLite database. It was created in the my_tennis_club root folder, and has the filename db.sqlite3. By default, all Models created in the Django project will be created as tables in this database.

Now when we have described a Model in the models.py file, we must run a command to actually create the table in the database. Navigate to the /my_tennis_club/ folder and run this command:
```sh
python manage.py makemigrations members
```
Django creates a file describing the changes and stores the file in the /migrations/ folder. Note that Django inserts an id field for your tables, which is an auto increment number (first record gets the value 1, the second record 2 etc.), this is the default behavior of Django, you can override it by describing your own id field.

The table is not created yet, you will have to run one more command, then Django will create and execute an SQL statement, based on the content of the new file in the /migrations/ folder. Run the migrate command:
```sh
python manage.py migrate
```
Now you have a Member table in you database!

As a side-note: you can view the SQL statement that were executed from the migration above. All you have to do is to run this command, with the migration number:
```sh
python manage.py sqlmigrate members 0001
```

### Django Models Insert Data
The Members table created in the previous chapter is empty. We will use the Python interpreter (Python shell) to add some members to it. To open a Python shell, type this command:
```sh
python manage.py shell
```
At the bottom, after the three >>> write the following:
```python
>>> from members.models import Member
>>> Member.objects.all()
```
A QuerySet is a collection of data from a database. Add a record to the table, by executing these two lines:
```python
>>> member = Member(firstname='Emil', lastname='Refsnes')
>>> member.save()
>>> Member.objects.all().values()
```
You can add multiple records by making a list of Member objects, and execute .save() on each entry:
```python
>>> member1 = Member(firstname='Tobias', lastname='Refsnes')
>>> member2 = Member(firstname='Linus', lastname='Refsnes')
>>> member3 = Member(firstname='Lene', lastname='Refsnes')
>>> member4 = Member(firstname='Stale', lastname='Refsnes')
>>> member5 = Member(firstname='Jane', lastname='Doe')
>>> members_list = [member1, member2, member3, member4, member5]
>>> for x in members_list: x.save()
... 
>>> Member.objects.all().values()
```

### Django Models Update Data
To update records that are already in the database, we first have to get the record we want to update and then change the value:
```python
>>> from members.models import Member
>>> x = Member.objects.all()[4]
>>> x.firstname
>>> x.firstname = "Stalikken"
>>> x.save()
>>> Member.objects.all().values()
```

### Django Models Delete Data
To delete a record in a table, start by getting the record you want to delete and then delete it:
```python
>>> from members.models import Member
>>> x = Member.objects.all()[5]
>>> x.firstname
>>> x.delete()
>>> Member.objects.all().values()
```

### Django Update Model
To add a field to a table after it is created, open the models.py file, and make your changes:
```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField(null=True)
  joined_date = models.DateField(null=True)
```
As you can see, we want to add phone and joined_date to our Member Model. This is a change in the Model's structure, and therefor we have to make a migration to tell Django that it has to update the database:
```sh
python manage.py makemigrations members
```
Run the migrate command:
```sh
python manage.py migrate
```
We can insert data to the two new fields with the same approach as we did in the Update Data chapter. First we enter the Python Shell:
```sh
python manage.py shell
```
At the bottom, after the three >>> write the following (and hit [enter] for each line):
```python
>>> from members.models import Member
>>> x = Member.objects.all()[0]
>>> x.phone = 5551234
>>> x.joined_date = '2022-01-05'
>>> x.save()
>>> Member.objects.all().values()
```

# Display Data

## Prepare Template and View
After creating Models, with the fields and data we want in them, it is time to display the data in a web page. Start by creating an HTML file named all_members.html and place it in the /templates/ folder:
```html
<!DOCTYPE html>
<html>
<body>

<h1>Members</h1>
  
<ul>
  {% for x in mymembers %}
    <li>{{ x.firstname }} {{ x.lastname }}</li>
  {% endfor %}
</ul>

</body>
</html>
```
Do you see the {% %} brackets inside the HTML document? They are Django Tags, telling Django to perform some programming logic inside these brackets.

Next we need to make the model data available in the template. This is done in the view. In the view we have to import the Member model, and send it to the template like this:
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def members(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('all_members.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
```
The members view does the following:
* Creates a mymembers object with all the values of the Member model.
* Loads the all_members.html template.
* Creates an object containing the mymembers object.
* Sends the object to the template.
* Outputs the HTML that is rendered by the template.

Start the server by navigating to the /my_tennis_club/ folder and execute this command:
```sh
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/members/ in the address bar.

## Add Link to Details
The next step in our web page will be to add a Details page, where we can list more details about a specific member. Start by creating a new template called details.html:
```html
<!DOCTYPE html>
<html>

<body>

<h1>{{ mymember.firstname }} {{ mymember.lastname }}</h1>
  
<p>Phone: {{ mymember.phone }}</p>
<p>Member since: {{ mymember.joined_date }}</p>

<p>Back to <a href="/members">Members</a></p>

</body>
</html>
```
The list in all_members.html should be clickable, and take you to the details page with the ID of the member you clicked on:
```html
<!DOCTYPE html>
<html>
<body>

<h1>Members</h1>
  
<ul>
  {% for x in mymembers %}
    <li><a href="details/{{ x.id }}">{{ x.firstname }} {{ x.lastname }}</a></li>
  {% endfor %}
</ul>

</body>
</html>
```
Then create a new view in the views.py file, that will deal with incoming requests to the /details/ url:
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def members(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('all_members.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
  
def details(request, id):
  mymember = Member.objects.get(id=id)
  template = loader.get_template('details.html')
  context = {
    'mymember': mymember,
  }
  return HttpResponse(template.render(context, request))
```
The details view does the following:
* Gets the id as an argument.
* Uses the id to locate the correct record in the Member table.
* Loads the details.html template.
* Creates an object containing the member.
* Sends the object to the template.
* Outputs the HTML that is rendered by the template.

Now we need to make sure that the /details/ url points to the correct view, with id as a parameter. Open the urls.py file and add the details view to the urlpatterns list:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
    path('members/details/<int:id>', views.details, name='details'),
]
```
If the server is down, you have to start it again with the runserver command:
```sh
python manage.py runserver
```

## Add Master Template
In the previous pages we created two templates, one for listing all members, and one for details about a member. The templates have a set of HTML code that are the same for both templates. Django provides a way of making a "parent template" that you can include in all pages to do the stuff that is the same in all pages. Start by creating a template called master.html, with all the necessary HTML elements:
```html
<!DOCTYPE html>
<html>
<head>
  <title>{% block title %}{% endblock %}</title>
</head>
<body>

{% block content %}
{% endblock %}

</body>
</html>
```
Do you see Django block Tag inside the <title> element, and the <body> element? They are placeholders, telling Django to replace this block with content from other sources.

Now the two templates (all_members.html and details.html) can use this master.html template.

This is done by including the master template with the {% extends %} tag, and inserting a title block and a content block:
```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club - List of all members
{% endblock %}


{% block content %}
  <h1>Members</h1>
  
  <ul>
    {% for x in mymembers %}
      <li><a href="details/{{ x.id }}">{{ x.firstname }} {{ x.lastname }}</a></li>
    {% endfor %}
  </ul>
{% endblock %}
```
```html
{% extends "master.html" %}

{% block title %}
  Details about {{ mymember.firstname }} {{ mymember.lastname }}
{% endblock %}


{% block content %}
  <h1>{{ mymember.firstname }} {{ mymember.lastname }}</h1>
  
  <p>Phone {{ mymember.phone }}</p>
  <p>Member since: {{ mymember.joined_date }}</p>
  
  <p>Back to <a href="/members">Members</a></p>
  
{% endblock %}
```
If the server is down, you have to start it again with the runserver command:
```sh
python manage.py runserver
```

## Add Main Index Page
Our project needs a main page. The main page will be the landing page when someone visits the root folder of the project. Now, you get an error when visiting the root folder of your project: 127.0.0.1:8000/. Start by creating a template called main.html:
```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club
{% endblock %}


{% block content %}
  <h1>My Tennis Club</h1>

  <h3>Members</h3>
  
  <p>Check out all our <a href="members/">members</a></p>
  
{% endblock %}
```
Then create a new view called main, that will deal with incoming requests to root of the project:
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def members(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('all_members.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
  
def details(request, id):
  mymember = Member.objects.get(id=id)
  template = loader.get_template('details.html')
  context = {
    'mymember': mymember,
  }
  return HttpResponse(template.render(context, request))
  
def main(request):
  template = loader.get_template('main.html')
  return HttpResponse(template.render())
```
The main view does the following:
* Loads the main.html template.
* Outputs the HTML that is rendered by the template.

Now we need to make sure that the root url points to the correct view. Open the urls.py file and add the main view to the urlpatterns list:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.main, name='main'),
    path('members/', views.members, name='members'),
    path('members/details/<int:id>', views.details, name='details'),
]
```
The members page is missing a link back to the main page, so let us add that in the all_members.html template, in the content block:
```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club - List of all members
{% endblock %}


{% block content %}

  <p><a href="/">HOME</a></p>

  <h1>Members</h1>
  
  <ul>
    {% for x in mymembers %}
      <li><a href="details/{{ x.id }}">{{ x.firstname }} {{ x.lastname }}</a></li>
    {% endfor %}
  </ul>
{% endblock %}
```
If the server is down, you have to start it again with the runserver command:
```sh
python manage.py runserver
```

## Django 404 Template
If you try to access a page that does not exist (a 404 error), Django directs you to a built-in view that handles 404 errors. You will learn how to customize this 404 view later in this chapter, but first, just try to request a page that does not exist. In the browser window, type 127.0.0.1:8000/masfdfg/ in the address bar.

You will get one of two results. If you get the dubug windown then then DEBUG is set to True in your settings, and you must set it to False to get directed to the 404 template. This is done in the settings.py file, which is located in the project folder, in our case the my_tennis_club folder, where you also have to specify the host name from where your project runs from:
```python
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = ['*']
```
Note. When DEBUG = False, Django requires you to specify the hosts you will allow this Django project to run from. In production, this should be replaced with a proper domain name: ALLOWED_HOSTS = ['yourdomain.com'].

Django will look for a file named 404.html in the templates folder, and display it when there is a 404 error. If no such file exists, Django shows the "Not Found" that you saw in the example above. To customize this message, all you have to do is to create a file in the templates folder and name it 404.html, and fill it with whatever you want:
```html
<!DOCTYPE html>
<html>
<title>Wrong address</title>
<body>

<h1>Ooops!</h1>

<h2>I cannot find the file you requested!</h2>

</body>
</html>
```

## Add Test View
