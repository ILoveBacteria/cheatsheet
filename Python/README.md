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
  - [Handwrite Notes](#handwrite-notes)
    - [Vectorization](#vectorization)

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


[1]: https://towardsdatascience.com/from-novice-to-expert-how-to-write-a-configuration-file-in-python-273e171a8eb3
[2]: https://medium.com/codex/say-goodbye-to-loops-in-python-and-welcome-vectorization-e4df66615a52
[3]: https://pandas.pydata.org/docs/user_guide/missing_data.html#string-regular-expression-replacement
[4]: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html