# Python Cheatsheet

## Table Of Contents

- [Python Cheatsheet](#python-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Pipenv](#pipenv)
  - [Config Files](#config-files)
    - [`ini` file](#ini-file)
    - [`yaml` file](#yaml-file)
    - [`toml` file](#toml-file)
  - [Pandas](#pandas)
  - [Matplot](#matplot)
  - [Unittest](#unittest)
  - [Functions that act on iterables](#functions-that-act-on-iterables)
  - [Iterable Objects](#iterable-objects)
    - [Generators](#generators)
    - [Iterate](#iterate)
  - [Method Overload](#method-overload)
  - [Flask](#flask)
    - [URL Building](#url-building)
    - [Variable Rules](#variable-rules)
    - [Extend template](#extend-template)
    - [HTTP Methods](#http-methods)
    - [Redirects](#redirects)
    - [Error Handling](#error-handling)
    - [Custom headers in response](#custom-headers-in-response)
    - [Request Object](#request-object)
  - [Logging](#logging)
    - [Log Levels](#log-levels)
    - [Logging from multiple modules to file](#logging-from-multiple-modules-to-file)
  - [Handwrite Notes](#handwrite-notes)
    - [Vectorization](#vectorization)
    - [Difference `__repr__` and `__str__`](#difference-__repr__-and-__str__)
    - [Object to dict](#object-to-dict)

## Pipenv

- `$ pipenv install`: Install dependencies from `Pipfile` or `requirements.txt`.
- `$ pipenv uninstall --all`: Uninstall all dependencies.
- `$ pipenv graph`: Show a graph of your installed dependencies.
- `$ pipenv lock`: Generate a `Pipfile.lock` file.
- Add custom `test` command to `Pipfile`:
```toml
[scripts]
test = "python -m unittest tests/test.py"
```

## Config Files

Read more in this source: [Link][1]

Built in packages: `configparser`, `json`, `yaml`, `toml`
Third party packages: `dotenv`

### `ini` file

- Supports only 1 level of nesting.
- Everything is a string.

```ini
# example_ini.ini
[APP]
ENVIRONMENT = test
DEBUG = True
# Only accept True or False

[DATABASE]
USERNAME = xiaoxu
PASSWORD = xiaoxu
HOST = 127.0.0.1
PORT = 5432
DB = xiaoxu_database
```

### `yaml` file

- Supports multiple levels of nesting.
- Supports string, integer, double, boolean, list, dictionary, etc.

```yaml
# example_yaml.yaml
APP:
  ENVIRONMENT: test
  DEBUG: True
  # Only accept True or False

DATABASE:
  USERNAME: xiaoxu
  PASSWORD: xiaoxu
  HOST: 127.0.0.1
  PORT: 5432
  DB: xiaoxu_database
```

### `toml` file

- TOML is similar to INI, but supports more data types and has defined syntax for nested structures.
- It’s used a lot by Python package management like pip or poetry. 
- But if the config file has too many nested structures, YAML is a better choice.

```toml
# example_toml.toml
[APP]
ENVIRONMENT = "test"
DEBUG = true
# Only accept True or False

[DATABASE]
USERNAME = "xiaoxu"
PASSWORD = "xiaoxu"
HOST = "127.0.0.1"
PORT = 5432
DB = "xiaoxu_database"
```

## Pandas

- `df.replace(".", np.nan)`: [Link to document][3]
- `df.replace(r"\s*\.\s*", np.nan, regex=True)`: [Link to document][3]
- `apply()`: Apply a function on every rows
- `df.drop_duplicates()`: Return DataFrame with duplicate rows removed [Link to document][4]

## Matplot

- A beautiful plotting

```python
complex, real, imag = fourier_transform(pulse_signal, t)
fig, (ax1, ax2) = plt.subplots(2, 1)
fig.suptitle('F.T. of pulse signal')
ax1.plot(t, real)
ax1.set_title('real part')
ax2.plot(t, imag)
ax2.set_title('imag part')

for ax in fig.get_axes():
    ax.label_outer()
```

## Unittest

| Method                                    | Checks that                                               |
| ----------------------------------------- | --------------------------------------------------------- |
| `assertEqual(a, b)`                       | `a == b`                                                  |
| `assertNotEqual(a, b)`                    | `a != b`                                                  |
| `assertTrue(x)`                           | `bool(x) is True`                                         |
| `assertFalse(x)`                          | `bool(x) is False`                                        |
| `assertIs(a, b)`                          | `a is b`                                                  |
| `assertIsNot(a, b)`                       | `a is not b`                                              |
| `assertIsNone(x)`                         | `x is None`                                               |
| `assertIn(a, b)`                          | `a in b`                                                  |
| `assertIsInstance(a, b)`                  | `isinstance(a, b)`                                        |
| `assertAlmostEqual(a, b)`                 | `round(a-b, 7) == 0`                                      |
| `assertGreater(a, b)`                     | `a > b`                                                   |
| `assertGreaterEqual(a, b)`                | `a >= b`                                                  |
| `assertLess(a, b)`                        | `a < b`                                                   |
| `assertLessEqual(a, b)`                   | `a <= b`                                                  |
| `assertRegex(s, r)`                       | `r.search(s)`                                             |
| `assertMultiLineEqual(a, b)`              | `a == b`, usable when `a` and `b` are strings             |
| `assertSequenceEqual(a, b)`               | `a == b`, usable when `a` and `b` are lists, tuples, etc. |
| `assertListEqual(a, b)`                   | `a == b`, usable when `a` and `b` are lists               |
| `assertTupleEqual(a, b)`                  | `a == b`, usable when `a` and `b` are tuples              |
| `assertSetEqual(a, b)`                    | `a == b`, usable when `a` and `b` are sets                |
| `assertDictEqual(a, b)`                   | `a == b`, usable when `a` and `b` are dicts               |
| `assertRaises(exc, fun, *args, **kwargs)` | `fun(*args, **kwargs)` raises `exc`                       |

## Functions that act on iterables

- `sum`: sum the contents of an iterable.
- `sorted`: return a list of the sorted contents of an interable
- `any`: returns `True` and ends the iteration immediately if `bool(item)` was `True` for any item in the iterable.
- `all`: returns `True` only if `bool(item)` was `True` for all items in the iterable.
- `max`: return the largest value in an iterable.
- `min`: return the smallest value in an iterable.

```python
# `bool(item)` evaluates to `False` for each of these items
>>> any((0, None, [], 0))
False

# `bool(item)` evaluates to  `True` for each of these items
>>> all([1, (0, 1), True, "hi"])
True
```

## Iterable Objects

### Generators

Because `range` is a generator, the command `range(5)` will simply store the instructions needed to produce the sequence of numbers 0-4, whereas the list `[0, 1, 2, 3, 4]` stores all of these items in memory at once.

Python provides a sleek syntax for defining a simple generator in a single line of code; this expression is known as a **generator comprehension**.
  
```python
# (<expression> for <var> in <iterable> if <condition>)
# when iterated over, `even_gen` will generate 0.. 2.. 4.. ... 98
even_gen = (i for i in range(100) if i%2 == 0)
```

### Iterate

To create an object/class as an iterator you have to implement the methods `__iter__()` and `__next__()` to your object.

- The `__iter__()` method acts similar, you can do operations (initializing etc.), but must always return the iterator object itself.
- The `__next__()` method also allows you to do operations, and must return the next item in the sequence.
- To prevent the iteration from going on forever, we can use the `StopIteration` statement.
- The `[...]` indexing syntax is called a subscript, because it's equivalent to mathematical notation that uses actual subscripts; e.g. `a[1]` is Python for what mathematicians would write as a₁. So "subscriptable" means "able to be subscripted". Which, in Python terms, means it has to implement `__getitem__()`, since `a[1]` is just syntactic sugar for `a.__getitem__(1)`.

```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self

  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration

myclass = MyNumbers()
myiter = iter(myclass)

for x in myiter:
  print(x)
```

## Method Overload

An efficient way to overload functions in Python

```python
from multipledispatch import dispatch
 
# passing one parameter
@dispatch(int, int)
def product(first, second):
    result = first*second
    print(result)
 
# passing two parameters
@dispatch(int, int, int)
def product(first, second, third):
    result = first * second * third
    print(result)
 
# you can also pass data type of any value as per requirement
@dispatch(float, float, float)
def product(first, second, third):
    result = first * second * third
    print(result)
 
# calling product method with 2 arguments
product(2, 3)  # this will give output of 6
# calling product method with 3 arguments but all int
product(2, 3, 2)  # this will give output of 12
# calling product method with 3 arguments but all float
product(2.2, 3.4, 2.3)  # this will give output of 17.985999999999997
```

## Flask

### URL Building

To build a URL to a specific function, use the `url_for()` function. It accepts the name of the function as its first argument and any number of keyword arguments, each corresponding to a variable part of the URL rule. Unknown variable parts are appended to the URL as query parameters.

Use this in templates: `{{ url_for('static', filename='dist/main.js') }}`

```python
from flask import url_for

@app.route('/')
def index():
    return 'index'

@app.route('/login')
def login():
    return 'login'

@app.route('/user/<username>')
def profile(username):
    return f'{username}\'s profile'

with app.test_request_context():
    print(url_for('index'))
    print(url_for('login'))
    print(url_for('login', next='/'))
    print(url_for('profile', username='John Doe'))
```

### Variable Rules

`<converter:variable_name>`

converters: `string` -  `int` - `float` - `path` - `uuid`

### Extend template

```html
{% raw %}
{% extends "base.html" %}

{% block head %}
    <script defer type="module" src={{ url_for('static', filename='dist/main.js') }}></script>
    <link rel="stylesheet" href={{ url_for('static', filename='css/game.css') }}>
    <title>Connect-4</title>
{% endblock %}

{% block body %}
    <div id="app"></div>
{% endblock %}
{% endraw %}
```

### HTTP Methods

By default, a route only answers to GET requests. You can use the methods argument of the route() decorator to handle different 
HTTP methods.
```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```

```python
@app.get('/login')
def login_get():
    return show_the_login_form()

@app.post('/login')
def login_post():
    return do_the_login()
```

### Redirects

To redirect a user to another endpoint, use the `redirect()` function; to abort a request early with an error code, use the `abort
()` function:
```python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```

### Error Handling

By default a black and white error page is shown for each error code. If you want to customize the error page, you can use the 
`errorhandler()` decorator:
```python
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```
Note the 404 after the render_template() call. This tells Flask that the status code of that page should be 404 which means not found. By default 200 is assumed which translates to: all went well.

### Custom headers in response

You just need to wrap the return expression with make_response() and get the response object to modify it, then return it:
```python
from flask import make_response

@app.errorhandler(404)
def not_found(error):
    resp = make_response(render_template('error.html'), 404)
    resp.headers['X-Something'] = 'A value'
    return resp
```

### Request Object
```python
from flask import request
```
The current request method is available by using the method attribute. To access form data (data transmitted in a `POST` or 
`PUT` request) you can use the form attribute. Here is a full example of the two attributes mentioned above:
```python
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
    # the code below is executed if the request method
    # was GET or the credentials were invalid
    return render_template('login.html', error=error)
```
What happens if the key does not exist in the form attribute? In that case a special `KeyError` is raised.

If the data is **JSON**, the request should have the content-type header set to application/json and use `request.json[]`

To access parameters submitted in the URL (`?key=value`) you can use the args attribute:
```python
searchword = request.args.get('key', '')
```

## Logging

### Log Levels

|Level|When it’s used|
|---|---|
|`DEBUG`|Detailed information, typically of interest only when diagnosing problems.|
|`INFO`|Confirmation that things are working as expected.|
|`WARNING`|An indication that something unexpected happened, or indicative of some problem in the near future (e.g. ‘disk space low’). The software is still working as expected.|
|`ERROR`|Due to a more serious problem, the software has not been able to perform some function.|
|`CRITICAL`|A serious error, indicating that the program itself may be unable to continue running.|

### Logging from multiple modules to file

```python
# myapp.py
import logging
import mylib

def main():
    logging.basicConfig(filename='myapp.log', level=logging.INFO)
    logging.info('Started')
    mylib.do_something()
    logging.info('Finished')

if __name__ == '__main__':
    main()
```
```python
# mylib.py
import logging

def do_something():
    logging.info('Doing something')
```
If you run myapp.py, you should see this in myapp.log:
```
INFO:root:Started
INFO:root:Doing something
INFO:root:Finished
```

## Handwrite Notes

### Vectorization

Vectorization is a technique to speed up your code by replacing explicit loops with array operations.

Read more examples in this source: [Link][2]

**Using Loops**
```python
import time 
start = time.time()

# Iterating through DataFrame using iterrows
for idx, row in df.iterrows():
    # creating a new column 
    df.at[idx,'ratio'] = 100 * (row["d"] / row["c"])  
end = time.time()
print(end - start)
### 109 Seconds
```
**Using Vectorization**

```python
start = time.time()
df["ratio"] = 100 * (df["d"] / df["c"])

end = time.time()
print(end - start)
### 0.12 seconds
```

### Difference `__repr__` and `__str__`

`__repr__()` provides the official string representation of an object, aimed at the programmer. `__str__()` provides the informal string representation of an object, aimed at the user.

### Object to dict

The `vars()` built-in function can be used to convert an object into a dictionary in Python.
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

# Create an instance of Person
person = Person("Alice", 30)
# Convert the object to a dictionary using vars()
person_dict = vars(person)
# Print the resulting dictionary
print(person_dict)
```


[1]: https://towardsdatascience.com/from-novice-to-expert-how-to-write-a-configuration-file-in-python-273e171a8eb3
[2]: https://medium.com/codex/say-goodbye-to-loops-in-python-and-welcome-vectorization-e4df66615a52
[3]: https://pandas.pydata.org/docs/user_guide/missing_data.html#string-regular-expression-replacement
[4]: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html