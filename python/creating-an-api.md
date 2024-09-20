---
title: "Creating ab API"
sidebar:
  order: 16
---

## Intro to Bottle

The **bottle** package is a fast and simple micro-framework for small web applications.

It offers request dispatching (Routes) with URL parameter support, templates, a built-in HTTP Server and adapters for many third party WSGI/HTTP-server and template engines.

```bash
pip install bottle
```

```py
from bottle import run, route

@route("/")
def root():
    return "Hello, Ninjas!"

if __name__ == "__main__":
    run(host="localhost", port=8080, debug=True)
```

## More Routes & reloader

```py
# ...

@route("/api/ninjas")
def get_ninjas():
    ninja_data = {
        "name": "Yoshi",
        "belt_color": "Black",
        "special_move": "Shadow Strike",
    }

    message = f'{ninja_data['name']} is a {ninja_data['belt_color']} belt ninja.'
    return message


if __name__ == "__main__":
    run(host="localhost", port=8080, debug=True, reloader=True)
```

## JSON Responses

```py
from bottle import run, route, response

# ...

@route("/api/ninjas")
def get_ninjas():
    ninjas = [
        {
            "name": "Yoshi",
            "belt_color": "Black",
            "special_move": "Shadow Strike",
        },
        {
            "name": "Hattori",
            "belt_color": "Green",
            "special_move": "Tornado Blast",
        },
        {
            "name": "Momochi",
            "belt_color": "Blue",
            "special_move": "Rain Leap",
        },
    ]

    response.content_type = "application/json"
    return {"data": ninjas}

# ...
```

## HTML Templates

_views/index.tpl_

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Ninja Town</title>
  </head>

  <body>
    <h1>Welcome to Ninja Town</h1>

    <ul>
      % for ninja in ninjas:
      <li>
        <div>{{ ninja["name"] }} - {{ ninja["belt_color"] }} belt</div>
        <div>Special move - {{ ninja["special_move"] }}</div>
      </li>
      % end
    </ul>
  </body>
</html>
```

```py
# ...

@route("/")
def root():
    return template("index", ninjas=ninjas)

@route("/api/ninjas")
def get_ninjas():
    response.content_type = "application/json"
    return {"data": ninjas}

# ...
```

## Static Files

_static/styles.css_

```css
@import url("https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,300..900;1,300..900&display=swap");

body {
  font-family: Rubik;
  background-color: #f0f0f0;
  padding: 20px;
}
h1 {
  color: #227cd6;
  text-align: center;
}
p {
  color: #555;
  text-align: center;
}
ul {
  list-style-type: none;
  padding: 0;
  width: 480px;
  margin: 30px auto;
}
li {
  color: #fff;
  background-color: #227cd6;
  margin: 10px 0;
  padding: 10px;
  border-radius: 4px;
}
```

_views/index.tpl_

```html
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="/public/styles.css" />
    <title>Ninja Town</title>
  </head>

  <!-- ... -->
</html>
```

```py
from bottle import run, route, response, template, static_file

# ...

@route("/public/<filename:path>")
def send_static(filename):
    return static_file(filename, root="static")


# ...
```

## POST Requests

```py
from bottle import run, route, response, template, static_file, request

# ...

@route("/api/ninjas")
def get_ninjas():

    response.content_type = "application/json"
    return {"data": ninjas}


@route("/api/ninjas", method="POST")
def add_ninja():
    new_ninja = request.json

    if isinstance(new_ninja, dict):
        ninjas.append(new_ninja)

    response.content_type = "application/json"

    return {"message": "Added a new ninja", "data": new_ninja}

# ...
```
