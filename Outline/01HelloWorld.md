# 01. Hello World

The Flask Mega-Tutorial by Miguel Grinberg -  https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world

1. Create Virtual Environment
2. Install Flask
3. Create "Hello, World" Flask application
4. Creating environment variables

## Create Virtual Environment

```shell
virtualenv venv -p /usr/bin/python3
source venv/bin/activate
```

### Freeze installed packages

```shell
pip freeze > requirements.txt
```

### Install packages from a requirements.txt

```shell
pip install -r requirements.txt
```

## Install Flask

```shell
pip install flask
```

## Create "Hello, World" Flask application

"The application will exist in a package. In Python, a sub-directory that includes a __init__.py file is considered a package, and can be imported. When you import a package, the __init__.py executes and defines what symbols the package exposes to the outside world."

### Create package app

The `app` package will host the application.It is defined by the app directory and the __init__.py script

```app/__init__.py
from flask import Flask

app = Flask(__name__)

from app import routes
```

- creates the application object as an instance of class Flask imported from the flask package

#### Multiple `app` entities

"One aspect that may seem confusing at first is that there are two entities named app. The app package is defined by the app directory and the __init__.py script, and is referenced in the from app import routes statement. The app variable is defined as an instance of class Flask in the __init__.py script, which makes it a member of the app package."

#### Circular Imports

"Another peculiarity is that the routes module is imported at the bottom and not at the top of the script as it is always done. The bottom import is a workaround to circular imports, a common problem with Flask applications. You are going to see that the routes module needs to import the app variable defined in this script, so putting one of the reciprocal imports at the bottom avoids the error that results from the mutual references between these two files."

### Create app/routes.py

"The routes are the different URLs that the application implements. In Flask, handlers for the application routes are written as Python functions, called view functions. View functions are mapped to one or more route URLs so that Flask knows what logic to execute when a client requests a given URL."

```app.routes.py
from app import app

@app.route('/')
@app.route('/index')
def index():
    return "Hello, World!"
```

#### View Function and Decorators

`def index()` is a view function

`@app.route` is a decorator. It modifies the function that follows it. It creates an association between the URL given as an argument and function.

"In this example there are two decorators, which associate the URLs / and /index to this function. This means that when a web browser requests either of these two URLs, Flask is going to invoke this function and pass the return value of it back to the browser as a response."

### Create the main application module

A python script at the top-level that defines the Flask application instance.
main.py

```main.py
from app import app
```

"The Flask application instance is called app and is a member of the app package. The from app import app statement imports the app variable that is a member of the app package."

##