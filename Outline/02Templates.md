# 02. Templates

The Flask Mega-Tutorial by Miguel Grinberg -  https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-ii-templates

1. Create Templates folder in app package
2. Dynamic Content
3. Conditional Statements
4. Loops
5. Template Inheritance

## Templates

"Templates help achieve this separation between presentation and business logic. In Flask, templates are written as separate files, stored in a templates folder that is inside the application package."

## Rendering

"The operation that converts a template into a complete HTML page. The `render_template()` function takes a template filename and a variable list of template arguments and returns the same template, but with all the placeholders in it replaced with actual values."

"The `render_template()` function invokes the Jinja2 template engine that comes bundled with the Flask framework. Jinja2 substitutes `{{ ... }}` blocks with the corresponding values, given by the arguments provided in the render_template() call."

### Dynamic content

"there are a couple of placeholders for the dynamic content, enclosed in `{{ ... }}` sections. These placeholders represent the parts of the page that are variable and will only be known at runtime."

```app/templates/index.html
<html>
    <head>
        <title>{{ title }} - Microblog</title>
    </head>
    <body>
        <h1>Hello, {{ user.username }}!</h1>
    </body>
</html>
```

```app/routes.py
from flask import render_template
from app import app

@app.route('/')
@app.route('/index')
def index():
    user = {'username': 'Miguel'}
    return render_template('index.html', title='Home', user=user)
```

## Conditional Statements

"templates also support control statements, given inside {% ... %} blocks."

``` app/templates/index.html
<html>
    <head>
        {% if title %}
        <title>{{ title }} - Microblog</title>
        {% else %}
        <title>Welcome to Microblog!</title>
        {% endif %}
    </head>
    <body>
        <h1>Hello, {{ user.username }}!</h1>
    </body>
</html>
```

"If the view function forgets to pass a value for the title placeholder variable, then instead of showing an empty title the template will provide a default one. You can try how this conditional works by removing the title argument in the render_template() call of the view function."

## Loops

Jinja2 offers a `for` control structure

``` app/templates/index.html
<html>
    <head>
        {% if title %}
        <title>{{ title }} - Microblog</title>
        {% else %}
        <title>Welcome to Microblog</title>
        {% endif %}
    </head>
    <body>
        <h1>Hi, {{ user.username }}!</h1>
        {% for post in posts %}
        <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
        {% endfor %}
    </body>
</html>
```

## Template Inheritance

"Jinja2 has a template inheritance feature that specifically addresses this problem. In essence, what you can do is move the parts of the page layout that are common to all templates to a base template, from which all other templates are derived."

"So what I'm going to do now is define a base template called base.html that includes a simple navigation bar and also the title logic I implemented earlier."

``` app/templates/base.html
<html>
    <head>
      {% if title %}
      <title>{{ title }} - Microblog</title>
      {% else %}
      <title>Welcome to Microblog</title>
      {% endif %}
    </head>
    <body>
        <div>Microblog: <a href="/index">Home</a></div>
        <hr>
        {% block content %}{% endblock %}
    </body>
</html>
```

"In this template I used the `block` control statement to define the place where the derived templates can insert themselves. Blocks are given a unique name, which derived templates can reference when they provide their content."

``` app/templates/index.html
{% extends "base.html" %}

{% block content %}
    <h1>Hi, {{ user.username }}!</h1>
    {% for post in posts %}
    <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
    {% endfor %}
{% endblock %}
```

"The extends statement establishes the inheritance link between the two templates, so that Jinja2 knows that when it is asked to render index.html it needs to embed it inside base.html. The two templates have matching block statements with name content, and this is how Jinja2 knows how to combine the two templates into one. "