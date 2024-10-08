* [https://www.w3schools.com/django/index.php](https://www.w3schools.com/django/index.php)

# Python Django Tutorial

## Create Virtual Environment
It is suggested to have a dedicated virtual environment for each Django project, and one way to manage a virtual environment is venv, which is included in Python. The name of the virtual environment is your choice, in this tutorial we will call it myworld. Type the following in the command prompt, remember to navigate to where you want to create your project:
```
python -m venv myworld
```
This will set up a virtual environment, and create a folder named "myworld" with subfolders and files. Then you have to activate the environment, by typing this command:
```
source myworld/bin/activate
```
Note: You must activate the virtual environment every time you open the command prompt to work on your project. It will look something like:
```
(myworld) ... $
```

## Install Django
Django is installed using pip, with this command:
```
(myworld) ... $ python -m pip install Django
```
You can check if Django is installed by asking for its version number like this:
```
django-admin --version
```

## Django Create Project
Once you have come up with a suitable name for your Django project, like mine: my_tennis_club, navigate to where in the file system you want to store the code (in the virtual environment), I will navigate to the myworld folder, and run this command in the command prompt:
```
django-admin startproject my_tennis_club
```
Django creates a my_tennis_club folder on my computer. These are all files and folders with a specific meaning, you will learn about some of them later in this tutorial, but for now, it is more important to know that this is the location of your project, and that you can start building applications in it.

Now that you have a Django project, you can run it, and see what it looks like in a browser. Navigate to the /my_tennis_club folder and execute this command in the command prompt:
```
python manage.py runserver
```
Open a new browser window and type 127.0.0.1:8000 in the address bar.

## Django Create App
An app is a web application that has a specific meaning in your project, like a home page, a contact form, or a members database. In this tutorial we will create an app that allows us to list and register members in a database. But first, let's just create a simple Django app that displays "Hello World!".

I will name my app members. Start by navigating to the selected location where you want to store the app, in my case the my_tennis_club folder, and run the command below.
```
python manage.py startapp members
```
Django creates a folder named members in my project. These are all files and folders with a specific meaning. You will learn about most of them later in this tutorial.

## Django Views
Django views are Python functions that take http requests and return http response, like HTML documents. A web page that uses Django is full of views with different tasks and missions. Views are usually put in a file called views.py located on your app's folder. Find it and open it, and replace the content with this:
```
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```
This is a simple example on how to send a response back to the browser. But how can we execute the view? Well, we must call the view via a URL.

## Django URLs
Create a file named urls.py in the same folder as the views.py file, and type this code in it:
```
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```
The urls.py file you just created is specific for the members application. We have to do some routing in the root directory my_tennis_club as well. This may seem complicated, but for now, just follow the instructions below. There is a file called urls.py on the my_tennis_club folder, open that file and add the include module in the import statement, and also add a path() function in the urlpatterns[] list, with arguments that will route users that comes in via 127.0.0.1:8000/. Then your file will look like this:
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```
If the server is not running, navigate to the /my_tennis_club folder and execute this command in the command prompt:
```
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/members/ in the address bar.

## Django Templates
In the Django Intro page, we learned that the result should be in HTML, and it should be created in a template, so let's do that. Create a templates folder inside the members folder, and create a HTML file named myfirst.html. Open the HTML file and insert the following:
```
<!DOCTYPE html>
<html>
<body>

<h1>Hello World!</h1>
<p>Welcome to my first Django project!</p>

</body>
</html>
```
Open the views.py file and replace the members view with this:
```
from django.http import HttpResponse
from django.template import loader

def members(request):
  template = loader.get_template('myfirst.html')
  return HttpResponse(template.render())
```
To be able to work with more complicated stuff than "Hello World!", We have to tell Django that a new app is created. This is done in the settings.py file in the my_tennis_club folder. Look up the INSTALLED_APPS[] list and add the members app like this:
```
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
```
python manage.py migrate
```
Start the server by navigating to the /my_tennis_club folder and execute this command:
```
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/members/ in the address bar.

## Django Models
A Django model is a table in your database. Up until now in this tutorial, output has been static data from Python or HTML templates. Now we will see how Django allows us to work with data, without having to change or upload files in the process. In Django, data is created in objects, called Models, and is actually tables in a database.

To create a model, navigate to the models.py file in the /members/ folder. Open it, and add a Member table by creating a Member class, and describe the table fields in it:
```
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
```
When we created the Django project, we got an empty SQLite database. It was created in the my_tennis_club root folder, and has the filename db.sqlite3. By default, all Models created in the Django project will be created as tables in this database.

Now when we have described a Model in the models.py file, we must run a command to actually create the table in the database. Navigate to the /my_tennis_club/ folder and run this command:
```
python manage.py makemigrations members
```
Django creates a file describing the changes and stores the file in the /migrations/ folder. Note that Django inserts an id field for your tables, which is an auto increment number (first record gets the value 1, the second record 2 etc.), this is the default behavior of Django, you can override it by describing your own id field.

The table is not created yet, you will have to run one more command, then Django will create and execute an SQL statement, based on the content of the new file in the /migrations/ folder. Run the migrate command:
```
python manage.py migrate
```
Now you have a Member table in you database!

As a side-note: you can view the SQL statement that were executed from the migration above. All you have to do is to run this command, with the migration number:
```
python manage.py sqlmigrate members 0001
```

### Django Models Insert Data
The Members table created in the previous chapter is empty. We will use the Python interpreter (Python shell) to add some members to it. To open a Python shell, type this command:
```
python manage.py shell
```
At the bottom, after the three >>> write the following:
```
>>> from members.models import Member
>>> Member.objects.all()
```
A QuerySet is a collection of data from a database. Add a record to the table, by executing these two lines:
```
>>> member = Member(firstname='Emil', lastname='Refsnes')
>>> member.save()
>>> Member.objects.all().values()
```
You can add multiple records by making a list of Member objects, and execute .save() on each entry:
```
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
```
>>> from members.models import Member
>>> x = Member.objects.all()[4]
>>> x.firstname
>>> x.firstname = "Stalikken"
>>> x.save()
>>> Member.objects.all().values()
```

### Django Models Delete Data
To delete a record in a table, start by getting the record you want to delete and then delete it:
```
>>> from members.models import Member
>>> x = Member.objects.all()[5]
>>> x.firstname
>>> x.delete()
>>> Member.objects.all().values()
```

### Django Update Model
To add a field to a table after it is created, open the models.py file, and make your changes:
```
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField(null=True)
  joined_date = models.DateField(null=True)
```
As you can see, we want to add phone and joined_date to our Member Model. This is a change in the Model's structure, and therefor we have to make a migration to tell Django that it has to update the database:
```
python manage.py makemigrations members
```
Run the migrate command:
```
python manage.py migrate
```
We can insert data to the two new fields with the same approach as we did in the Update Data chapter. First we enter the Python Shell:
```
python manage.py shell
```
At the bottom, after the three >>> write the following (and hit [enter] for each line):
```
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
```
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
```
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
```
python manage.py runserver
```
In the browser window, type 127.0.0.1:8000/members/ in the address bar.