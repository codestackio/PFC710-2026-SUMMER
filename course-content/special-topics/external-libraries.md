# Special Topic: External Libraries

[← Back to Course Index](../table-of-contents.md)

## 1. External Libraries

### What Are External Libraries?

Python's standard library is powerful, but sometimes you need extra functionality.

**External libraries** (also called **third-party libraries**) extend Python's capabilities in specialized areas. These libraries can be installed and managed to simplify your development process.

**Examples:**

- `requests` — HTTP requests
- `numpy` — numerical computations
- `matplotlib` — plotting graphs
- `pandas` — data analysis
- And thousands more!

### Why Use External Libraries?

- **Save time** by reusing code written by others
- **Access advanced functionality** not in the Python standard library
- **Avoid reinventing the wheel** — many problems are already solved with existing libraries
- **Improve code readability and maintainability**

**Example:** Instead of writing HTTP requests from scratch, use `requests` to handle complex networking tasks.

### Finding Libraries on PyPI

Python packages are hosted in the **Python Package Index (PyPI)** at [https://pypi.org](https://pypi.org).

Search by keyword or by the task you need (for example, "http client" or "plotting"). PyPI is the official repository that **pip** downloads from.

**Key term:**

- **Dependency** — a library your project relies on (often installed automatically with the package you asked for)

---

## 2. Managing Packages with pip

**pip** is the default tool for installing and managing external libraries in Python. It downloads packages from PyPI into your current Python environment.

### Listing Installed Packages

In PyCharm's **Terminal** (button on the bottom left of the screen), run:

```bash
pip list
```

This lists the packages installed in your current environment.

### Installing a Package

```bash
pip install requests
```

This downloads and installs the `requests` library and any dependencies it requires.

> **Note:** In PyCharm, you can also use the **Python Packages** tool window (bottom left) to browse installed packages and install new ones.

### Updating and Uninstalling

**Update** a package to its latest version:

```bash
pip install --upgrade <package_name>
```

**Uninstall** a package you no longer need:

```bash
pip uninstall <package_name>
```

---

## 3. Tracking Dependencies with `requirements.txt`

Do not ask teammates to guess which packages to install. Record your dependencies in a plain-text file called `requirements.txt`:

```text
requests==2.31.0
rich>=13.0
```

**Generate it** from the packages currently installed in your environment:

```bash
pip freeze > requirements.txt
```

**Install everything listed in it** (for example, when someone clones your repo):

```bash
pip install -r requirements.txt
```

This single file makes a Python project **reproducible** — anyone with the file and the same Python version can recreate your environment in seconds.

---

## 4. The `requests` Library

The **`requests`** package is a popular library for sending HTTP requests. It hides low-level networking behind a simple API so you can talk to web services and APIs using standard HTTP methods (GET, POST, PUT, DELETE, and others).

If you have not already installed it:

```bash
pip install requests
```

**HTTP background** (optional reading):

- [HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [HTTP Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

### A Simple GET Request

```python
import requests

# Send a GET request
response = requests.get('https://jsonplaceholder.typicode.com/posts')

# Check if the request was successful
if response.status_code == 200:
    # Print the response content
    print(response.json())  # Returns the response as JSON
else:
    print("Failed to retrieve data: " + str(response.status_code))
```

This example:

1. Imports the `requests` library
2. Sends a GET request to a test API endpoint
3. Checks the status code (`200` means success)
4. Prints the JSON response if successful

---

[← Back to Course Index](../table-of-contents.md)
