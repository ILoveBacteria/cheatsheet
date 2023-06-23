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