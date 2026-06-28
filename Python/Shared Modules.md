# Creating and Installing a Simple Python Module You Can Import from Any Folder

This tutorial shows how to:

* Create a simple Python module
* Package it properly
* Install it into your Python environment
* Import it from **any directory** on your computer
* Update the module during development

This works on:

* Windows
* Linux
* macOS

---

# What is a Python Module?

A Python module is simply a `.py` file that contains reusable code.

Example:

```python
mathutils.py
```

```python
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b
```

You can import it like this:

```python
import mathutils

print(mathutils.add(5, 3))
```

The problem is that this only works if `mathutils.py` is in the same directory or somewhere on Python's search path.

A better solution is to install the module.

---

# Step 1 – Create a Project Folder

Create a new folder.

```
MyModule/
```

Inside it create the following structure.

```
MyModule/
│
├── mymodule/
│   ├── __init__.py
│   └── mathutils.py
│
├── pyproject.toml
└── README.md
```

---

# Step 2 – Create the Module

Create

```
mymodule/mathutils.py
```

```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    return a / b
```

---

# Step 3 – Create `__init__.py`

This tells Python that this folder is a package.

```
mymodule/__init__.py
```

```python
from .mathutils import *
```

Now users can simply write

```python
import mymodule

print(mymodule.add(10, 5))
```

instead of

```python
from mymodule.mathutils import add
```

---

# Step 4 – Create README.md

```
# MyModule

Simple math utilities.
```

---

# Step 5 – Create pyproject.toml

This is the modern way to package Python projects.

```toml
[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"

[project]
name = "mymodule"
version = "1.0.0"
description = "Example reusable Python module"
readme = "README.md"
authors = [
    {name = "Your Name"}
]
requires-python = ">=3.9"

[tool.setuptools]
packages = ["mymodule"]
```

---

# Step 6 – Install the Module

Open a terminal.

Navigate to your project.

```
cd MyModule
```

Install it.

```bash
pip install .
```

Output

```
Successfully installed mymodule-1.0.0
```

---

# Step 7 – Test It

Go somewhere completely different.

Example

```
Desktop/
```

Create

```python
test.py
```

```python
import mymodule

print(mymodule.add(2, 3))
print(mymodule.multiply(8, 7))
```

Run it.

```bash
python test.py
```

Output

```
5
56
```

Notice that **nothing from the module exists in this directory**.

Python found it because it was installed.

---

# How Python Finds Installed Modules

You can see where Python searches.

```python
import sys

for path in sys.path:
    print(path)
```

One of these directories is your Python installation's

```
site-packages
```

When you installed your package, Python copied it there.

---

# Where Installed Packages Live

On Windows

```
C:\Users\<username>\AppData\Local\Programs\Python\Python313\Lib\site-packages
```

or

```
C:\Python313\Lib\site-packages
```

Linux

```
~/.local/lib/python3.13/site-packages
```

or

```
/usr/lib/python3.13/site-packages
```

---

# Development Mode

If you're still editing the module, don't reinstall it after every change.

Instead use

```bash
pip install -e .
```

The `-e` means **editable**.

Now your source folder is linked directly into Python.

Edit

```python
mymodule/mathutils.py
```

Save.

Run your program.

No reinstall is required.

---

# Uninstall

```bash
pip uninstall mymodule
```

---

# Adding More Files

```
mymodule/

    __init__.py

    mathutils.py

    geometry.py

    statistics.py
```

Example

```
geometry.py
```

```python
import math

def area_circle(radius):
    return math.pi * radius ** 2
```

Update `__init__.py`

```python
from .mathutils import *
from .geometry import *
```

Now

```python
import mymodule

print(mymodule.area_circle(10))
```

---

# Organizing a Larger Module

```
mymodule/

    __init__.py

    mathutils.py

    geometry.py

    statistics.py

    strings.py

    files.py

    network.py
```

This keeps related functionality grouped into separate files while presenting a simple interface through `__init__.py`.

---

# Installing in a Virtual Environment

If you're using a virtual environment, activate it before installing:

**Windows (Command Prompt):**

```cmd
venv\Scripts\activate
```

**Windows (PowerShell):**

```powershell
venv\Scripts\Activate.ps1
```

**Linux/macOS:**

```bash
source venv/bin/activate
```

Then install your module:

```bash
pip install -e .
```

The module will only be available while that virtual environment is active, which helps keep project dependencies isolated.

---

# Best Practices

* Give your package a unique, lowercase name to avoid conflicts with existing packages.
* Use `pyproject.toml` instead of the older `setup.py` for new projects.
* Prefer `pip install -e .` during development so changes are immediately available.
* Add type hints and docstrings to public functions as your module grows.
* Keep related functionality in separate files and re-export the public API from `__init__.py`.
* Consider adding automated tests (for example, using `pytest`) before publishing your package.

---

# Summary

You learned how to:

1. Create a Python package with a proper directory structure.
2. Add reusable functions to your module.
3. Configure packaging with `pyproject.toml`.
4. Install the package so it can be imported from any directory.
5. Use editable installs (`pip install -e .`) for efficient development.
6. Expand your package by organizing code into multiple modules.

This workflow is the same foundation used by many popular Python libraries and is the recommended approach for creating reusable code that can be shared across multiple projects or distributed to other users.
