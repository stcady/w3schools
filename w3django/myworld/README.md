# Python Django Tutorial

### Virtual Environment
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

### 