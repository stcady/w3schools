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
When testing different aspects of Django, it can be a good idea to have somewhere to test code without destroying the main project. Start by adding a view called "testing" in the views.py file:
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

def testing(request):
  template = loader.get_template('template.html')
  context = {
    'fruits': ['Apple', 'Banana', 'Cherry'],   
  }
  return HttpResponse(template.render(context, request))
```
We have to make sure that incoming urls to /testing/ will be redirected to the testing view. This is done in the urls.py file in the members folder:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.main, name='main'),
    path('members/', views.members, name='members'),
    path('members/details/<int:id>', views.details, name='details'),
    path('testing/', views.testing, name='testing'),    
]
```
We also need a template where we can play around with HTML and Django code. Create a template called "template.html" in the templates folder. Open the template.html file and insert the following:
```html
<!DOCTYPE html>
<html>
<body>

{% for x in fruits %}
  <h1>{{ x }}</h1>
{% endfor %}

<p>In views.py you can see what the fruits variable looks like.</p>

</body>
</html>
```
If the server is not running, navigate to the /my_tennis_club folder and execute this command in the command prompt:
```sh
python manage.py runserver 
```

# Admin

## Django Admin
Django Admin is a really great tool in Django, it is actually a CRUD* user interface of all your models!

To enter the admin user interface, start the server by navigating to the /my_tennis_club folder and execute this command:
```sh
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/admin/ in the address bar. If the page does not fully render then reset the debug mode to True.

The reason why this URL goes to the Django admin log in page can be found in the urls.py file of your project:
```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```
The urlpatterns[] list takes requests going to admin/ and sends them to admin.site.urls, which is part of a built-in application that comes with Django, and contains a lot of functionality and user interfaces, one of them being the log-in user interface.

## Create User
To be able to log into the admin application, we need to create a user. This is done by typing this command in the command view:
```sh
python manage.py createsuperuser
```
Here you must enter: username, e-mail address, (you can just pick a fake e-mail address), and password.

Now start the server again:
```sh
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/admin/ in the address bar. And fill in the form with the correct username and password. Which should result in this user interface. Here you can create, read, update, and delete groups and users, but where is the Members model? The Members model is missing, as it should be, you have to tell Django which models that should be visible in the admin interface.

## Include Models
To include the Member model in the admin interface, we have to tell Django that this model should be visible in the admin interface. This is done in a file called admin.py, and is located in your app's folder, which in our case is the members folder. Open it, and it should look like this:
```python
from django.contrib import admin

# Register your models here.
```
Insert a couple of lines here to make the Member model visible in the admin page:
```python
from django.contrib import admin
from .models import Member

# Register your models here.
admin.site.register(Member)
```
Now go back to the browser and you should see Member in the admin page. Click Members and see the five records we inserted earlier in this tutorial.

In the list we see "Member object (1)", "Member object (2)" etc. which might not be the data you wanted to be displayed in the list. It would be better to display "firstname" and "lastname" instead. This can easily be done by changing some settings in the models.py and/or the admin.py files.

## Set List Display
When you display a Model as a list, Django displays each record as the string representation of the record object, which in our case is "Member object (1)", "Member object(2)" etc.

To change this to a more reader-friendly format, we have two choices:
* Change the string representation function, __str__() of the Member Model
* Set the list_details property of the Member Model

To change the string representation, we have to define the __str__() function of the Member Model in models.py, like this:
```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField(null=True)
  joined_date = models.DateField(null=True)

  def __str__(self):
    return f"{self.firstname} {self.lastname}"
```
Defining our own __str__() function is not a Django feature, it is how to change the string representation of objects in Python.

We can control the fields to display by specifying them in in a list_display property in the admin.py file. First create a MemberAdmin() class and specify the list_display tuple, like this:
```python
from django.contrib import admin
from .models import Member

# Register your models here.

class MemberAdmin(admin.ModelAdmin):
  list_display = ("firstname", "lastname", "joined_date",)
  
admin.site.register(Member, MemberAdmin)
```
Remember to add the MemberAdmin as an argumet in the admin.site.register(Member, MemberAdmin).

## Create, Update, Deleted Members
Now we are able to create, update, and delete members in our database, and we start by giving them all a date for when they became members.

# Django Syntax

## Django Variables
In Django templates, you can render variables by putting them inside {{ }} brackets:
```html
<h1>Hello {{ firstname }}, how are you?</h1>
```
The variable firstname in the example above was sent to the template via a view:
```python
from django.http import HttpResponse
from django.template import loader

def testing(request):
  template = loader.get_template('template.html')
  context = {
    'firstname': 'Linus',
  }
  return HttpResponse(template.render(context, request))
```
As you can see in the view above, we create an object named context and fill it with data, and send it as the first parameter in the template.render() function.

You can also create variables directly in the template, by using the {% with %} template tag. The variable is available until the {% endwith %} tag appears:
```html
{% with firstname="Tobias" %}
<h1>Hello {{ firstname }}, how are you?</h1>
{% endwith %}
```

The example above showed a easy approach on how to create and use variables in a template. Normally, most of the external data you want to use in a template, comes from a model. To get data from the Member model, we will have to import it in the views.py file, and extract data from it in the view:
```python
from django.http import HttpResponse, HttpResponseRedirect
from django.template import loader
from .models import Member

def testing(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('template.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
```
Now we can use the data in the template:
```html
<ul>
{% for x in mymembers %}
  <li>{{ x.firstname }}</li>
{% endfor %}
</ul>
```
We use the Django template tag {% for %} to loop through the members.

## Django Tags
In Django templates, you can perform programming logic like executing if statements and for loops. These keywords, if and for, are called "template tags" in Django. To execute template tags, we surround them in {% %} brackets.
```html
{% if greeting == 1 %}
  <h1>Hello</h1>
{% else %}
  <h1>Bye</h1>
{% endif %}
```
The template tags are a way of telling Django that here comes something else than plain HTML. The template tags allows us to do some programming on the server before sending HTML to the client.
```html
<ul>
  {% for x in mymembers %}
    <li>{{ x.firstname }}</li>
  {% endfor %}
</ul>
```
A list of all template tags:
* autoescape - Specifies if autoescape mode is on or off 
* block	- Specifies a block section
* comment	- Specifies a comment section
* csrf_token - Protects forms from Cross Site Request Forgeries
* cycle - Specifies content to use in each cycle of a loop
* debug - Specifies debugging information
* extends - Specifies a parent template
* filter - Filters content before returning it
* firstof - Returns the first not empty variable
* for - Specifies a for loop
* if - Specifies a if statement
* ifchanged - Used in for loops. Outputs a block only if a value has changed since the last iteration
* include - Specifies included content/template
* load - Loads template tags from another library
* lorem - Outputs random text
* now - Outputs the current date/time
* regroup - Sorts an object by a group
* resetcycle - Used in cycles. Resets the cycle
* spaceless - Removes whitespace between HTML tags
* templatetag - Outputs a specified template tag
* url	- eturns the absolute URL part of a URL
* verbatim - Specifies contents that should not be rendered by the template engine
* widthratio - Calculates a width value based on the ratio between a given value and a max value
* with - Specifies a variable to use in the block

## Django If Else
An if statement evaluates a variable and executes a block of code if the value is true.
```html
{% if greeting == 1 %}
  <h1>Hello</h1>
{% endif %} 
```
The elif keyword says "if the previous conditions were not true, then try this condition".
```html
{% if greeting == 1 %}
  <h1>Hello</h1>
{% elif greeting == 2 %}
  <h1>Welcome</h1>
{% endif %} 
```
The else keyword catches anything which isn't caught by the preceding conditions.
```html
{% if greeting == 1 %}
  <h1>Hello</h1>
{% elif greeting == 2 %}
  <h1>Welcome</h1>
{% else %}
  <h1>Goodbye</h1>
{% endif %} 
```

## Django Operators
The above examples uses the == operator, which is used to check if a variable is equal to a value, but there are many other operators you can use, or you can even drop the operator if you just want to check if a variable is not empty:
```html
{% if greeting %}
  <h1>Hello</h1>
{% endif %} 
```
Is equal to.
```html
{% if greeting == 2 %}
  <h1>Hello</h1>
{% endif %} 
```
Is not equal to.
```html
{% if greeting != 1 %}
  <h1>Hello</h1>
{% endif %} 
```
Is less than.
```html
{% if greeting < 3 %}
  <h1>Hello</h1>
{% endif %} 
```
Is less than, or equal to.
```html
{% if greeting <= 3 %}
  <h1>Hello</h1>
{% endif %} 
```
Is greater than.
```html
{% if greeting > 1 %}
  <h1>Hello</h1>
{% endif %} 
```
Is greater than, or equal to.
```html
{% if greeting >= 1 %}
  <h1>Hello</h1>
{% endif %} 
```
To check if more than one condition is true.
```html
{% if greeting == 1 and day == "Friday" %}
  <h1>Hello Weekend!</h1>
{% endif %} 
```
To check if one of the conditions is true.
```html
{% if greeting == 1 or greeting == 5 %}
  <h1>Hello</h1>
{% endif %} 
```
Combine and and or.
```html
{% if greeting == 1 and day == "Friday" or greeting == 5 %}
```
Parentheses are not allowed in if statements in Django, so when you combine and and or operators, it is important to know that parentheses are added for and but not for or. Meaning that the above example is read by the interpreter like this:
```html
{% if (greeting == 1 and day == "Friday") or greeting == 5 %}
```
To check if a certain item is present in an object.
```html
{% if 'Banana' in fruits %}
  <h1>Hello</h1>
{% else %}
  <h1>Goodbye</h1>
{% endif %} 
```
To check if a certain item is not present in an object.
```html
{% if 'Banana' not in fruits %}
  <h1>Hello</h1>
{% else %}
  <h1>Goodbye</h1>
{% endif %} 
```
Check if two objects are the same. This operator is different from the == operator, because the == operator checks the values of two objects, but the is operator checks the identity of two objects. In the view we have two objects, x and y, with the same values:
```python
from django.http import HttpResponse
from django.template import loader

def testing(request):
  template = loader.get_template('template.html')
  context = {
    'x': ['Apple', 'Banana', 'Cherry'], 
    'y': ['Apple', 'Banana', 'Cherry'], 
  }
  return HttpResponse(template.render(context, request))  
```
The two objects have the same value, but is it the same object?
```html
{% if x is y %}
  <h1>YES</h1>
{% else %}
  <h1>NO</h1>
{% endif %}
```
Let us try the same example with the == operator instead:
```html
{% if x == y %}
  <h1>YES</h1>
{% else %}
  <h1>NO</h1>
{% endif %}
```
How can two objects be the same? Well, if you have two objects that points to the same object, then the is operator evaluates to true: We will demonstrate this by using the {% with %} tag, which allows us to create variables in the template:
```html
{% with var1=x var2=x %}
  {% if var1 is var2 %}
    <h1>YES</h1>
  {% else %}
    <h1>NO</h1>
  {% endif %}
{% endwith %}
```
To check if two objects are not the same.
```html
{% if x is not y %}
  <h1>YES</h1>
{% else %}
  <h1>NO</h1>
{% endif %} 
```

## Django For Loop
A for loop is used for iterating over a sequence, like looping over items in an array, a list, or a dictionary.

Loop through the items of a list:
```html
{% for x in fruits %}
  <h1>{{ x }}</h1>
{% endfor %}
```
Loop through a list of dictionaries:
```html
{% for x in cars %}
  <h1>{{ x.brand }}</h1>
  <p>{{ x.model }}</p>
  <p>{{ x.year }}</p>
{% endfor %} 
```

Data in a model is like a table with rows and columns. The Member model we created earlier has five rows, and each row has three columns. When we fetch data from the model, it comes as a QuerySet object, with a similar format as the cars example above: a list with dictionaries:
```html
<QuerySet [
  {
    'id': 1,
    'firstname': 'Emil',
    'lastname': 'Refsnes',
    'phone': 5551234,
    'joined_date': datetime.date(2022, 1, 5)
  },
  {
    'id': 2,
    'firstname': 'Tobias',
    'lastname': 'Refsnes'
    'phone': 5557777,
    'joined_date': datetime.date(2021, 4, 1)
  },
  {
    'id': 3,
    'firstname': 'Linus',
    'lastname': 'Refsnes'
    'phone': 5554321,
    'joined_date': datetime.date(2021, 12, 24)
  },
  {
    'id': 4,
    'firstname': 'Lene',
    'lastname': 'Refsnes'
    'phone': 5551234,
    'joined_date': datetime.date(2021, 5, 1)
  },
  {
    'id': 5,
    'firstname': 'Stalikken',
    'lastname': 'Refsnes'
    'phone': 5559876,
    'joined_date': datetime.date(2022, 9, 29)
  }
]> 
```
Loop through items fetched from a database:
```html
{% for x in members %}
  <h1>{{ x.id }}</h1>
  <p>
    {{ x.firstname }}
    {{ x.lastname }}
  </p>
{% endfor %} 
```

The reversed keyword is used when you want to do the loop in reversed order.
```html
{% for x in members reversed %}
  <h1>{{ x.id }}</h1>
  <p>
    {{ x.firstname }}
    {{ x.lastname }}
  </p>
{% endfor %} 
```

The empty keyword can be used if you want to do something special if the object is empty.
```html
<ul>
  {% for x in emptytestobject %}
    <li>{{ x.firstname }}</li>
  {% empty %}
    <li>No members</li>
  {% endfor %}
</ul> 
```
The empty keyword can also be used if the object does not exist:
```html
<ul>
  {% for x in myobject %}
    <li>{{ x.firstname }}</li>
  {% empty %}
    <li>No members</li>
  {% endfor %}
</ul> 
```

Django has some variables that are available for you inside a loop:
* forloop.counter
* forloop.counter0
* forloop.first
* forloop.last
* forloop.parentloop
* forloop.revcounter
* forloop.revcounter0

forloop.counter - The current iteration, starting at 1.
```html
<ul>
  {% for x in fruits %}
    <li>{{ forloop.counter }}</li>
  {% endfor %}
</ul> 
```
forloop.counter0 - The current iteration, starting at 0.
```html
<ul>
  {% for x in fruits %}
    <li>{{ forloop.counter0 }}</li>
  {% endfor %}
</ul> 
```
forloop.first - Allows you to test if the loop is on its first iteration.
```html
<ul>
  {% for x in fruits %}
    <li
      {% if forloop.first %}
        style='background-color:lightblue;'
      {% endif %}
    >{{ x }}</li>
  {% endfor %}
</ul> 
```
forloop.last - Allows you to test if the loop is on its last iteration.
```html
<ul>
  {% for x in fruits %}
    <li
      {% if forloop.last %}
        style='background-color:lightblue;'
      {% endif %}
    >{{ x }}</li>
  {% endfor %}
</ul> 
```
forloop.revcounter - The current iteration if you start at the end and count backwards, ending up at 1.
```html
<ul>
  {% for x in fruits %}
    <li>{{ forloop.revcounter }}</li>
  {% endfor %}
</ul> 
```
forloop.revcounter0 - The current iteration if you start at the end and count backwards, ending up at 0.
```html
<ul>
  {% for x in fruits %}
    <li>{{ forloop.revcounter0 }}</li>
  {% endfor %}
</ul> 
```

## Django Comment
Comments allows you to have sections of code that should be ignored.
```html
<h1>Welcome Everyone</h1>
{% comment %}
  <h1>Welcome ladies and gentlemen</h1>
{% endcomment %}
```
You can add a message to your comment, to help you remember why you wrote the comment, or as message to other people reading the code.
```html
<h1>Welcome Everyone</h1>
{% comment "this was the original welcome message" %}
    <h1>Welcome ladies and gentlemen</h1>
{% endcomment %}
```
You can also use the {# ... #} tags when commenting out code, which can be easier for smaller comments:
```html
<h1>Welcome{# Everyone#}</h1>
```
Views are written in Python, and Python comments are written with the # character:
```python
from django.http import HttpResponse
from django.template import loader

def testing(request):
  template = loader.get_template('template.html')
  #context = {
  # 'var1': 'John',
  #}
  return HttpResponse(template.render())
```

## Django Include
The include tag allows you to include a template inside the current template. This is useful when you have a block of content that is the same for many pages.
```html
<p>You have reached the bottom of this page, thank you for your time.</p>
```
```html
<h1>Hello</h1>

<p>This page contains a footer in a template.</p>

{% include 'footer.html' %} 
```

You can send variables into the template by using the with keyword. In the include file, you refer to the variables by using the {{ variablename }} syntax:
```html
<div>HOME | {{ me }} | ABOUT | FORUM | {{ sponsor }}</div>
```
```html
<!DOCTYPE html>
<html>
<body>

{% include "mymenu.html" with me="TOBIAS" sponsor="W3SCHOOLS" %}

<h1>Welcome</h1>

<p>This is my webpage</p>

</body>
</html> 
```

# QuerySets

## QuerySet Introduction
A QuerySet is a collection of data from a database and is built up as a list of objects. QuerySets makes it easier to get the data you actually need, by allowing you to filter and order the data at an early stage.

In views.py, we have a view for testing called testing where we will test different queries. In the example below we use the .all() method to get all the records and fields of the Member model:
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def testing(request):
  mydata = Member.objects.all()
  template = loader.get_template('template.html')
  context = {
    'mymembers': mydata,
  }
  return HttpResponse(template.render(context, request))
```
The object is placed in a variable called mydata, and is sent to the template via the context object as mymembers, and looks like this:

```sh
<QuerySet [
  <Member: Member object (1)>,
  <Member: Member object (2)>,
  <Member: Member object (3)>,
  <Member: Member object (4)>,
  <Member: Member object (5)>
]>
```
As you can see, our Member model contains 5 records, and are listed inside the QuerySet as 5 objects. In the template you can use the mymembers object to generate content:
```html
<table border='1'>
  <tr>
    <th>ID</th>
    <th>Firstname</th>
    <th>Lastname</th>
  </tr>
  {% for x in mymembers %}
    <tr>
      <td>{{ x.id }}</td>
        <td>{{ x.firstname }}</td>
      <td>{{ x.lastname }}</td>
    </tr>
  {% endfor %}
</table>
```

## QuerySet Get
There are different methods to get data from a model into a QuerySet.

The values() method allows you to return each object as a Python dictionary, with the names and values as key/value pairs:
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def testing(request):
  mydata = Member.objects.all().values()
  template = loader.get_template('template.html')
  context = {
    'mymembers': mydata,
  }
  return HttpResponse(template.render(context, request))
```
The values_list() method allows you to return only the columns that you specify.
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def testing(request):
  mydata = Member.objects.values_list('firstname')
  template = loader.get_template('template.html')
  context = {
    'mymembers': mydata,
  }
  return HttpResponse(template.render(context, request))
```
Return only the records where firstname is 'Emil'
```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def testing(request):
  mydata = Member.objects.filter(firstname='Emil').values()
  template = loader.get_template('template.html')
  context = {
    'mymembers': mydata,
  }
  return HttpResponse(template.render(context, request))
```

## QuerySet Filter
The filter() method is used to filter your search, and allows you to return only the rows that matches the search term. As we learned in the previous chapter, we can filter on field names like this:
```python
mydata = Member.objects.filter(firstname='Emil').values()
```
In SQL, the above statement would be written like this:
```sql
SELECT * FROM members WHERE firstname = 'Emil';
```
The filter() method takes the arguments as **kwargs (keyword arguments), so you can filter on more than one field by separating them by a comma.
```python
mydata = Member.objects.filter(lastname='Refsnes', id=2).values()
```
In SQL, the above statement would be written like this:
```sql
SELECT * FROM members WHERE lastname = 'Refsnes' AND id = 2;
```
To return records where firstname is Emil or firstname is Tobias (meaning: returning records that matches either query, not necessarily both) is not as easy as the AND example above. We can use multiple filter() methods, separated by a pipe | character. The results will merge into one model.
```python
mydata = Member.objects.filter(firstname='Emil').values() | Member.objects.filter(firstname='Tobias').values()
```
Another common method is to import and use Q expressions:
```python
mydata = Member.objects.filter(Q(firstname='Emil') | Q(firstname='Tobias')).values()
```
In SQL, the above statement would be written like this:
```sql
SELECT * FROM members WHERE firstname = 'Emil' OR firstname = 'Tobias';
```
Django has its own way of specifying SQL statements and WHERE clauses. To make specific where clauses in Django, use "Field lookups". Field lookups are keywords that represents specific SQL keywords. All Field lookup keywords must be specified with the fieldname, followed by two(!) underscore characters, and the keyword. In our Member model, the statement would be written like this:
```python
mydata = Member.objects.filter(firstname__startswith='L').values()
```
A list of all field look up keywords:
* contains	Contains the phrase
* icontains	Same as contains, but case-insensitive
* date	Matches a date
* day	Matches a date (day of month, 1-31) (for dates)
* endswith	Ends with
* iendswith	Same as endswidth, but case-insensitive
* exact	An exact match
* iexact	Same as exact, but case-insensitive
* in	Matches one of the values
* isnull	Matches NULL values
* gt	Greater than
* gte	Greater than, or equal to
* hour	Matches an hour (for datetimes)
* lt	Less than
* lte	Less than, or equal to
* minute	Matches a minute (for datetimes)
* month	Matches a month (for dates)
* quarter	Matches a quarter of the year (1-4) (for dates)
* range	Match between
* regex	Matches a regular expression
* iregex	Same as regex, but case-insensitive
* second	Matches a second (for datetimes)
* startswith	Starts with
* istartswith	Same as startswith, but case-insensitive
* time	Matches a time (for datetimes)
* week	Matches a week number (1-53) (for dates)
* week_day	Matches a day of week (1-7) 1 is sunday
* iso_week_day	Matches a ISO 8601 day of week (1-7) 1 is monday
* year	Matches a year (for dates)
* iso_year	Matches an ISO 8601 year (for dates)

## QuerySet Order By
To sort QuerySets, Django uses the order_by() method. Order the result alphabetically by firstname:
```python
mydata = Member.objects.all().order_by('firstname').values()
```
In SQL, the above statement would be written like this:
```sql
SELECT * FROM members ORDER BY firstname;
```

By default, the result is sorted ascending (the lowest value first), to change the direction to descending (the highest value first), use the minus sign (NOT), - in front of the field name:
```python
mydata = Member.objects.all().order_by('-firstname').values()
```
In SQL, the above statement would be written like this:
```sql
SELECT * FROM members ORDER BY firstname DESC;
```

To order by more than one field, separate the fieldnames with a comma in the order_by() method. Order the result first by lastname ascending, then descending on id:
```python
mydata = Member.objects.all().order_by('lastname', '-id').values()
```
In SQL, the above statement would be written like this:
```sql
SELECT * FROM members ORDER BY lastname ASC, id DESC;
```

# Static Files

## Add Static Files
When building web applications, you probably want to add some static files like images or css files. Start by creating a folder named static in your project, the same place where you created the templates folder. The name of the folder has to be static.

Add a CSS file in the static folder, the name is your choice, we will call it myfirst.css in this example. Open the CSS file and insert the following:
```css
body {
  background-color: lightblue;
  font-family: verdana;
}
```
Now you have a CSS file, with some CSS styling. The next step will be to include this file in a HTML template: Open the HTML file and add the following:
```html
{% load static %}
<link rel="stylesheet" href="{% static 'myfirst.css' %}">
```
```html
{% load static %}
<!DOCTYPE html>
<html>
<link rel="stylesheet" href="{% static 'myfirst.css' %}">
<body>

{% for x in fruits %}
  <h1>{{ x }}</h1>
{% endfor %}

</body>
</html>
```

Just testing? If you just want to play around, and not going to deploy your work, you can set DEBUG = True in the settings.py file, and the example above will work. Plan to deploy? If you plan to deploy your work, you should set DEBUG = False in the settings.py file. The example above will fail, because Django has no built-in solution for serving static files, but there are other ways to serve static files, you will learn how in the next chapter. When using DEBUG = False you have to specify which host name(s) are allowed to host your work. You could choose '127.0.0.1' or 'localhost' which both represents the address of your local machine. We choose '*', which means any address are allowed to host this site. This should be change into a real domain name when you deploy your project to a public server.

## Installing WhiteNoise
Django does not have a built-in solution for serving static files, at least not in production when DEBUG has to be False. We have to use a third-party solution to accomplish this. In this Tutorial we will use WhiteNoise, which is a Python library, built for serving static files.

To install WhiteNoise in your virtual environment, type the command below:
```sh
pip install whitenoise
```
To make Django aware of you wanting to run WhitNoise, you have to specify it in the MIDDLEWARE list in settings.py file:
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
].
```
There is one more action you have to perform before you can serve the static file from the example in the previous chapter. You have to collect all static files and put them into one specified folder. You will learn how in the next chapter.

## Collect Static Files
Static files in your project, like stylesheets, JavaScripts, and images, are not handled automatically by Django when DEBUG = False. When DEBUG = True, this worked fine, all we had to do was to put them in the static folder of the application. When DEBUG = False, static files have to be collected and put in a specified folder before we can use it.

To collect all necessary static files for your project, start by specifying a STATIC_ROOT property in the settings.py file. This specifies a folder where you want to collect your static files. You can call the folder whatever you like, we will call it productionfiles:
```python
STATIC_ROOT = BASE_DIR / 'productionfiles'

STATIC_URL = 'static/'
```
You could manually create this folder and collect and put all static files of your project into this folder, but Django has a command that do this for you:
```sh
python manage.py collectstatic
```
Why so many files? Well this is because of the admin user interface, that comes built-in with Django. We want to keep this feature in production, and it comes with a whole bunch of files including stylesheets, fonts, images, and JavaScripts.

Now you have collected the static files of your project, and if you have installed WhiteNoise, the example from the Add Static Files chapter will finally work. Start the server and see the result:
```sh
python manage.py runserver
```

## Add Global Static Files
We have learned how to add a static file in the application's static folder, and how to use it in the application. But what if other applications in your project wants to use the file? Then we have to create a folder on the root directory and put the file(s) there. It is not enough to create a static folder in the root directory, and Django will fix the rest. We have to tell Django where to look for these static files. Start by creating a folder on the project's root level, this folder can be called whatever you like, I will call it mystaticfiles in this tutorial. Add a CSS file in the mystaticfiles folder, the name is your choice, we will call it myglobal.css in this example. Open the CSS file and insert the following:
```css
body {
  color: violet;
}
```
You will have to tell Django to also look for static files in the mystaticfiles folder in the root directory, this is done in the settings.py file:
```python
STATIC_ROOT = BASE_DIR / 'productionfiles'

STATIC_URL = 'static/'

#Add this in your settings.py file:
STATICFILES_DIRS = [
    BASE_DIR / 'mystaticfiles'
]
```
In the STATICFILES_DIRS list, you can list all the directories where Django should look for static files. The BASE_DIR keyword represents the root directory of the project, and together with the / "mystaticfiles", it means the mystaticfiles folder in the root directory. If you have files with the same name, Django will use the first occurrence of the file. The search starts in the directories listed in STATICFILES_DIRS, using the order you have provided. Then, if the file is not found, the search continues in the static folder of each application.

Now you have a global CSS file for the entire project, which can be accessed from all your applications. To use it in a template, use the same syntax as you did for the myfirst.css file. Begin the template with the following:
```html
{% load static %}
<!DOCTYPE html>
<html>
<link rel="stylesheet" href="{% static 'myglobal.css' %}">
<body>

{% for x in fruits %}
  <h1>{{ x }}</h1>
{% endfor %}

</body>
</html>
```

Run the collectstatic command to collect the new static file:
```sh
python manage.py collectstatic
```

## Add Styles to the Project
We want to add a stylesheet to this project, and put it in the mystaticfiles folder. The name of the CSS file is your choice, we call it mystyles.css in this project. Open the CSS file and insert the following:
```css
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;600&display=swap');
body {
  margin:0;
  font: 600 18px 'Source Sans Pro', sans-serif;
  letter-spacing: 0.64px;
  color: #585d74;
}
.topnav {
  background-color:#375BDC;
  color:#ffffff;
  padding:10px;
}
.topnav a:link, .topnav a:visited {
  text-decoration: none;
  color: #ffffff; 
}
.topnav a:hover, .topnav a:active {
  text-decoration: underline;
}
.mycard {
  background-color: #f1f1f1;
  background-image: linear-gradient(to bottom, #375BDC, #4D70EF); 
  background-size: 100% 120px;
  background-repeat: no-repeat;
  margin: 40px auto;
  width: 350px;
  border-radius: 5px;
  box-shadow: 0 5px 7px -1px rgba(51, 51, 51, 0.23); 
  padding: 20px;
}
.mycard h1 {
  text-align: center;
  color:#ffffff;
  margin:20px 0 60px 0;
}
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
li {
  background-color: #ffffff;
  background-image: linear-gradient(to right, #375BDC, #4D70EF); 
  background-size: 50px 60px;
  background-repeat: no-repeat;
  cursor: pointer;
  transition: transform .25s;
  border-radius: 5px;
  box-shadow: 0 5px 7px -1px rgba(51, 51, 51, 0.23);
  padding: 15px;
  padding-left: 70px;
  margin-top: 5px;
}
li:hover {
  transform: scale(1.1);
}
a:link, a:visited {
  color: #375BDC; 
}
.main, .main h1 {
  text-align:center;
  color:#375BDC;
}
```
Now you have a css file, the next step will be to include this file in the master template. Open the master template file and add the following:
```html
{% load static %}
<!DOCTYPE html>
<html>
<head>
  <title>{% block title %}{% endblock %}</title>
  <link rel="stylesheet" href="{% static 'mystyles.css' %}">  
</head>
<body>

{% block content %}
{% endblock %}

</body>
</html>
```
In all_members.html we want to make som changes in the HTML code. The members are put in a div element, and the links become list items with onclick attributes. We also want to remove the navigation, because that is now a part of the master template.
```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club - List of all members
{% endblock %}


{% block content %}
  <div class="mycard">
    <h1>Members</h1>
    <ul>
      {% for x in mymembers %}
        <li onclick="window.location = 'details/{{ x.id }}'">{{ x.firstname }} {{ x.lastname }}</li>
      {% endfor %}
    </ul>
  </div>
{% endblock %}
```
In details.html we will put the member details in a div element, and remove the link back to members, because that is now a part of the navigation in the master template.
```html
{% extends "master.html" %}

{% block title %}
  Details about {{ mymember.firstname }} {{ mymember.lastname }}
{% endblock %}

{% block content %}
  <div class="mycard">
    <h1>{{ mymember.firstname }} {{ mymember.lastname }}</h1>
    <p>Phone {{ mymember.phone }}</p>
    <p>Member since: {{ mymember.joined_date }}</p>
  </div>
{% endblock %}  
```
In the main.html template, we will put some of the HTML code into a div element:
```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club
{% endblock %}

{% block content %}
  <div class="main">
    <h1>My Tennis Club</h1>

    <h3>Members</h3>
  
    <p>Check out all our <a href="members/">members</a></p>
  </div>
{% endblock %}
```

# Postgre SQL

## Introduction to Postgre SQL
Django comes with a SQLite database which is great for testing and debugging at the beginning of a project. However, it is not very suitable for production. Django also support these database engines:

* PostgreSQL
* MariaDB
* MySQL
* Oracle

We will take a closer look at the PostgreSQL database engine.

PostgreSQL database is an open source relational database, which should cover most demands you have when creating a database for a Django project. It has a good reputation, it is reliable, and it perform well under most circumstances. We will add a PostgreSQL database to our Django project. To be able to use PostgreSQL in Django we have to install a package called psycopg2.

Type this command in the command line to install the package. Make sure you are still inn the virtual environment:
```sh
pip install psycopg2-binary
```
The psycopg2 package is a driver that is necessary for PostgreSQL to work in Python. We also need a server where we can host the database. In this tutorial we have chosen the Amazon Web Services (AWS) platform, you will learn more about that in the next chapter.

## Create AWS Account
There are many providers out there that can host Django projects and PostgreSQL databases. In this tutorial we will use the Amazon Web Services (AWS) platform, mainly because they offer a free solution that can host both Django projects and PostgreSQL databases. All you need is an AWS account. Go to aws.amazon.com, and create an account. Once you have created an AWS account, it is time to sign in for the first time. If this is your first time you sign into your AWS account, you will be directed to the AWS Console Home page.

Once you have an AWS account, you can start creating a database. We will use a database service at AWS, called RDS. In the search field, search for "RDS", and click to start the service:

## Create Database in RDS
Inside the RDS service, create a database, either by navigating to the Database section, or just click the "Create database" button. Once you have started creating a database, you will be given some choices for the type and settings of your database. This will take a few minutes, but when it is finished, you will have a new PostgreSQL database, almost ready to run on your Django project!

## Connect to Database
To make Django able to connect to your database, you have to specify it in the DATABASES tuple in the settings.py file. Before, it looked like this:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
Now, you should change it to look like this:
```python

```
As you can see in the settings.py file, we insert postgresql instead of sqlite.

The database does not have a name, but you have to assign one in order to access the database. If no name is given, the provider accepts 'postgres' as the name of the database. Insert the username and password that you specified when you created the database.

As you can see in the settings.py file, we insert postgresql instead of sqlite, and insert the username and password that we specified when we created the database. The HOST and PORT can be found under the "Connectivity & security" section in the RDS instance. They are described as "Endpoint" and "Port".

Once we have done the changes in settings.py, we must run a migration in our virtual environment, before the changes will take place:
```sh
python manage.py migrate
```
Now, if you run the project:
```sh
python manage.py runserver
```
And view it in your browser: 127.0.0.1:8000/. You will get the home page of the project, but if you click on the "members" link, you will see that there are no members.

## Add Members
The "My Tennis Club" project has no members: 127.0.0.1:8000/. That is because we have created a brand new database, and it is empty. The old SQLite database contained 5 members, so let us dive into the admin interface and add the same 5 members. But first we have to create a new superuser.

Since we now have a new database, we have to create the superuser once again. This is done by typing this command in the command view:
```sh
python manage.py createsuperuser
```
Here you must enter: username, e-mail address, (you can just pick a fake e-mail address), and password.

Now start the server again:
```sh
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/admin in the address bar. And fill in the form with the correct username and password. When you are in the admin interface, click the "Add" button for "Members", and start inserting new members until you have a list. In the browser window, type 127.0.0.1:8000/members in the address bar. And once again you have a Tennis Club page with 5 members!

# Deploy Django

## Elastic Beanstalk (EB)

## Create requirements.txt

## Create django.config

## Create .zip File

## Deploy with EB

## Update Project
