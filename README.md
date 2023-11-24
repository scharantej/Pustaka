 ## Problem Analysis

The problem is to design a Kannada reading website using Flask. The website should have the following features:

* A home page that displays a list of all the available books.
* A book page that displays the contents of a book.
* A search page that allows users to search for books by title or author.
* A login page that allows users to create an account and log in.
* A profile page that displays the user's information and allows them to edit their profile.

## Design

The following HTML files are needed for the application:

* `index.html`: The home page.
* `book.html`: The book page.
* `search.html`: The search page.
* `login.html`: The login page.
* `profile.html`: The profile page.

The following routes are needed for the application:

* `/`: The home page.
* `/book/<book_id>`: The book page.
* `/search`: The search page.
* `/login`: The login page.
* `/profile`: The profile page.

## HTML Files

The following is the HTML code for the home page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Kannada Reading Website</title>
</head>
<body>
  <h1>Kannada Reading Website</h1>
  <ul>
    {% for book in books %}
      <li><a href="/book/{{ book.id }}">{{ book.title }}</a></li>
    {% endfor %}
  </ul>
</body>
</html>
```

The following is the HTML code for the book page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>{{ book.title }}</title>
</head>
<body>
  <h1>{{ book.title }}</h1>
  <p>{{ book.content }}</p>
</body>
</html>
```

The following is the HTML code for the search page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Search</title>
</head>
<body>
  <h1>Search</h1>
  <form action="/search" method="GET">
    <input type="text" name="q" placeholder="Search">
    <input type="submit" value="Search">
  </form>
</body>
</html>
```

The following is the HTML code for the login page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Login</title>
</head>
<body>
  <h1>Login</h1>
  <form action="/login" method="POST">
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" value="Login">
  </form>
</body>
</html>
```

The following is the HTML code for the profile page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Profile</title>
</head>
<body>
  <h1>Profile</h1>
  <p>Username: {{ user.username }}</p>
  <p>Email: {{ user.email }}</p>
  <a href="/logout">Logout</a>
</body>
</html>
```

## Routes

The following is the code for the routes:

```python
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

books = [
  {
    "id": 1,
    "title": "The Lord of the Rings",
    "content": "The Lord of the Rings is an epic fantasy novel written by English author and scholar J. R. R. Tolkien. The story began as a sequel to Tolkien's 1937 fantasy novel The Hobbit, but eventually developed into a much larger work. Written in stages between 1937 and 1949, with much of it being written during World War II, it was originally published in three volumes in 1954 and 1955. It has since been reprinted numerous times and translated into at least 38 languages, becoming one of the best-selling novels ever written."
  },
  {
    "id": 2,
    "title": "The Hobbit",
    "content": "The Hobbit is a fantasy novel by English author J. R. R. Tolkien. It began as a children's book but eventually developed into a much larger work. Written in stages between 1930 and 1937, it was originally published in 1937 and has since been reprinted numerous times and translated into at least 38 languages, becoming one of the best-selling novels ever written."
  },
  {
    "id": 3,
    "title": "The Silmarillion",
    "content": "The Silmarillion is a collection of myths and legends of the First Age of Middle-earth, the fictional setting of J. R. R. Tolkien's legendarium. It was published posthumously in 1977, four years after Tolkien's death, and was edited by his son Christopher Tolkien."
  }
]

@app.route("/")
def home():
  return render_template("index.html", books=books)

@app.route("/book/<book_id>")
def book(book_id):
  book = next((book for book in books if book["id"] == int(book_id)), None)
  if book is None:
    return redirect(url_for("home"))
  return render_template("book.html", book=book)

@app.route("/search")
def search():
  query = request.args.get("q")
  if query is None:
    return redirect(url_for("home"))
  books = [book for book in books if query in book["title"]]
  return render_template("index.html", books=books)

@app.route("/login", methods=["GET", "POST"])
def login():
  if request.method == "GET":
    return render_template("login.html")
  else:
    username = request.form.get("username")
    password = request.form.get("password")
    if username == "admin" and password == "secret":
      return redirect(url_for("profile"))
    else:
      return redirect(url_for("login"))

@app.route("/profile")
def profile():
  return render_template("profile.html", user={"username": "admin", "email": "admin@example.com"})

if __name__ == "__main__":
  app.run(debug=True)
```