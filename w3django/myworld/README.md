# Python Django Tutorial

## Virtual Environment
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

### Install Django
Django is installed using pip, with this command:
```
(myworld) ... $ python -m pip install Django
```
You can check if Django is installed by asking for its version number like this:
```
django-admin --version
```

### Django Create Project
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

### Create App
An app is a web application that has a specific meaning in your project, like a home page, a contact form, or a members database. In this tutorial we will create an app that allows us to list and register members in a database. But first, let's just create a simple Django app that displays "Hello World!".

I will name my app members. Start by navigating to the selected location where you want to store the app, in my case the my_tennis_club folder, and run the command below.
```
python manage.py startapp members
```
Django creates a folder named members in my project. These are all files and folders with a specific meaning. You will learn about most of them later in this tutorial.

### Django Views
Django views are Python functions that take http requests and return http response, like HTML documents. A web page that uses Django is full of views with different tasks and missions. Views are usually put in a file called views.py located on your app's folder. Find it and open it, and replace the content with this:
```
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```
This is a simple example on how to send a response back to the browser. But how can we execute the view? Well, we must call the view via a URL.