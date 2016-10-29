# Running a web server with Python Flask

## Links:
**[Jinja2 reference]**(http://jinja.pocoo.org/docs/dev/templates/)

**[Flask]**(http://flask.pocoo.org/docs/0.11/)

**[More Flask]**(https://exploreflask.com/en/latest/)

To install Flask, use pip:
```
sudo pip install Flask
```

This will install the necessary libraries for Flask.

To make sure it installed, open an interpreter and import Flask
```python
$>> python
    >>from flask import Flask
    >>
```

## Directory stucture
Flask apps have key folders they look for on a server.

If we have a Flask app in the file 'server.py', your directory should look like:

-/server_directory
  - server.py
  - templates/
  - static/
  - media/

The directories:

1. **templates/** - Where your HTML templates are stored (render_templates leads here)
2. **static/** - Files like .css and .js are stored here (<link> tags will be referenced here)
3. **media/** - Images stored here (static may also be used)

## Basic Flask server
Flask comes pre-built with a hello.py "Hello world" program.  Screw that, here are the basics you REALLY need:

**server.py**
```python
from flask import Flask, render_template, request, url_for

app = Flask(__name__)

@app.route("/", methods=["GET","POST"])
def index():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(host="0.0.0.0",port=80)
```
### Breaking this down:
```python
from flask import Flask, render_template, request

app = Flask(__name__)
```
This simply imports 3 packages from the flask library and initializes an app instance.

1. **Flask** - needed for creating the app (bare minimum needed to run a server)
2. **render_template** - Flask uses Jinja2 templates; to return HTML, you must render the template that's stored in the 'templates' folder
  1. Jinja2 templates are a mix of HTML and some scripting to make HTML-generation much easier
  2. The templates folder can be changed, but for all intents and purposes it's easiest to just set up the directory as shown above.
3. **request** - Often you'll need to make a GET/POST using AJAX when app developing.  This import will let you parse what was passed to the server (example to come).

```
@app.route("/", methods=["GET","POST"])
def index():
    return render_template("index.html")
```

Here we let Flask know what to do when a client hits the server (in this case, at the home address).
@app.route is a decorator that causes the function below it to run once the route is hit.

If we use localhost as our URL, more examples are:

```python
@app.route("/hackers", methods=["GET","POST"]) # Called when localhost/hackers is hit

...

@app.route("/python", methods=["GET"]) # Called when localhost/python is hit, also rejects POSTs

...

@app.route("/we/are/the/best") # Called when localhost/we/are/the/best is hit, accepts GET and POST methods

```

After our route, we define the function to be called; the name is arbitary, just **make sure you return something**.
In our example, we simply render and return a template called 'index.html' from the templates folder

Lastly:
```python
if __name__ == "__main__":
    app.run(host='0.0.0.0',port=80)
```
Here, the app is run when called.
*Note* the paramaters of app.run() are optional. The default host is localhost, while the default port is 80.  host='0.0.0.0' ensures that whatever url is used will work (a nice shorthand to the server's actual IP).

## Intro to templates
There's a great deal you can do with templates, and maybe one day I'll make a longer intro.  For now, it's concept + example

When HTML files are passed when using a Flask server, a template is first called, converted, and served.  This means...
this:
```html
{% block head %}

{% endblock %}

{% block body %}
<h1>My page</h1>
<p>Pretty darn basic</p>
{% endblock %}
``` 
is served as:
```html
<html>
<head>
</head>
<body>
<h1>My page</h1>
<p>Pretty darn basic</p>
</body>
</html>
```
<head> can be used instead of {% block head %} (and similar for body) if you're not using inheritance (that's right. Inheritance in HTML.  PRAISE BE.), however if you try extending a page, blocks are needed so the parser knows where to place things.  I'll have an inheritance example up later.

# Loops and logic

You can use loops and logic (python-based syntax)

**index.html**
```
<ul>
{% for key in dictionary %}
<li>{{ key }}</li>
{% endfor %}
</ul>
```
This prints out a list with each item being the key from a dictionary.  The dictionary is passed when the template is rendered.  Variables in Jinja2 templates are denoted by being placed within {{ }}

Serving ```index.html``` correctly:

**server.py**
```python

...

@app.route('/')
def thisNameDoesntMatterLaddyLaddyLaaaa():
    x = {'key':'value','key2':'value2'}
    return render_template('index.html',dictionary=x)

...

```

Passing variables into render_template allows them to be used in the templates.

Here's an example using **if statements**
**index.html** (template)

```
{% if {{ isTrue }} == 1 %}
<h1>It's true</h1>
{% elif {{ isTrue }} == 0 %}
<h1>It's not true</h1>
{% else %}
<h1>It's superimposed (tralse)</h1>
{% endif %}
```

if we rendered ```index.html``` with the following code:
```python
...
    return render_template('index.html',isTrue=False)
...
```

The served file would be:
**index.html** (HTML)
```
<h1>It's not true</h1>
```

## More info come?
