
# Python Modules and Packages –
### (module-package.md)

---

# 🧩 1. What Is a Python Module?

A **module** is simply a Python file (`.py`) that contains:

- Variables  
- Functions  
- Classes  
- Any Python code  

If a file ends with `.py`, congratulations — it’s a *module*.

---

## ✔ Example of a Module: `intf_data.py`

```python
sw_list = ["sw1", "sw2", "sw3"]
sites_list = ["site-A", "site-B", "site-C"]

intf_conf_data = [
    {"intf_name": "gig0/1", "desc": "Connected to server1", "speed": 1000, "duplex": "full"},
    {"intf_name": "gig0/2", "desc": "Connected to server2", "speed": 1000, "duplex": "full"},
    {"intf_name": "gig0/3", "desc": "Connected to server3", "speed": 1000, "duplex": "full"}
]

intf_syntax = {
    "intf_name": "interface {}",
    "desc": "description {}",
    "speed": "speed {}",
    "duplex": "duplex {}"
}
```

This file *by itself* is a module.

To use it:

```python
import intf_data

print(intf_data.sw_list)
print(intf_data.intf_syntax)
```

---

# 📦 2. What Is a Python Package?

A **package** is:

> A folder that contains multiple Python modules + a special `__init__.py` file.

This tells Python:
> “This folder is a package. You can import modules from inside it.”

### Example packaging:

```
netbuilder/
│
├── __init__.py
├── intf_data.py
└── builder.py
```

Now you can import modules like:

```python
from netbuilder import intf_data
```

---

# 🚀 3. Why Use Modules and Packages?

### Without Modules:
- All code in one messy file  
- Difficult to maintain  
- Impossible to scale to large automation projects

### With Modules:
- Organized  
- Clean  
- Reusable  
- Professional approach

This is how real-world automation frameworks (Ansible, Nornir, Netmiko) are structured.

---

# 🛠 4. Using Your Example as a Module

Your full example reorganized as clean modules:

---

## 📁 File 1: `intf_data.py` (Module Holding Data)

```python
sw_list = ["sw1", "sw2", "sw3"]
sites_list = ["site-A", "site-B", "site-C"]

intf_conf_data = [
    {"intf_name": "gig0/1", "desc": "Connected to server1", "speed": 1000, "duplex": "full"},
    {"intf_name": "gig0/2", "desc": "Connected to server2", "speed": 1000, "duplex": "full"},
    {"intf_name": "gig0/3", "desc": "Connected to server3", "speed": 1000, "duplex": "full"}
]

intf_syntax = {
    "intf_name": "interface {}",
    "desc": "description {}",
    "speed": "speed {}",
    "duplex": "duplex {}"
}
```

---

## 📁 File 2: `main.py` (Imports the Module)

```python
import intf_data

for site in intf_data.sites_list:
    print("
Configuring Site", site)

    for sw in intf_data.sw_list:
        print("
Configuring", sw)

        for item in intf_data.intf_conf_data:
            print(intf_data.intf_syntax["intf_name"].format(item["intf_name"]))
            print(intf_data.intf_syntax["desc"].format(item["desc"]))
            print(intf_data.intf_syntax["speed"].format(item["speed"]))
            print(intf_data.intf_syntax["duplex"].format(item["duplex"]))
```

Now your logic is clean, modular, and reusable.

---

# 📦 5. Turning Modules Into a Package

```
netbuilder/
│
├── __init__.py       # Required for package
├── intf_data.py      # Module 1
└── builder.py        # Module 2
```

Then:

```python
from netbuilder import intf_data
```

This is how professional network automation repos are structured.

---

# 🧾 Summary Table

| Concept | Meaning | Example |
|--------|---------|---------|
| **Module** | A single Python file | `intf_data.py` |
| **Package** | A folder of modules + `__init__.py` | `netbuilder/` |
| **Import** | Use one module inside another | `import intf_data` |

Modules = small building blocks  
Packages = organized collections of modules  

This makes large automation projects easy to manage.
