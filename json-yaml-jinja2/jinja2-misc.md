
# Jinja2 Miscellaneous –


# 🌍 1. What is Jinja2?
# ⭐ **Jinja2 — A Template Engine**

A *template* means:

> “A file with blanks that will be filled automatically.”

Example:

```
interface {{ intf }}
description {{ desc }}
```

The parts inside `{{ ... }}` are **blanks**.  
Jinja2 fills these blanks using data (YAML/JSON/Python variables).

---

# 🌟 2. Why is it called *Jinja2*?

There *was* a Jinja version 1.  
It was limited.

Jinja *version 2* (Jinja2):

✔ Faster  
✔ More powerful  
✔ Supports loops, conditions, filters  
✔ Can handle complex automation  
✔ Works everywhere (Python, Ansible, Nornir, SaltStack)

Yes, newer versions exist now, but the automation world still calls it “Jinja2” because:

- Most tools are built around Jinja2 syntax  
- All documentation, books, examples refer to Jinja2  
- It became the “standard name” in the industry  

So don’t get confused — *Jinja2* is the famous one.

---

# 📦 3. The Three Building Blocks of Jinja2 Templates

Every `.j2` file in the world is made of just **three things**:

## 3.1 Variables → `{{ ... }}`
They print values.

Example:

```
hostname {{ device_name }}
```

If data contains:

```
device_name: R1
```

Output becomes:

```
hostname R1
```

---

## 3.2 Logic / Control → `{% ... %}`
These tell the template **how to behave**, similar to instructions:

- loops  
- conditions  
- blocks  
- macros  

Example loop:

```
{% for intf in interfaces %}
interface {{ intf }}
{% endfor %}
```

---

## 3.3 Comments → `{# ... #}`
These are ignored.

```
{# this is a comment #}
```

They don’t go into the final config.

---

# 🔁 4. Loops in Jinja2 (The Heart of Automation)

Loops repeat parts of the config automatically.

Imagine you have:

```
GigabitEthernet0/1
GigabitEthernet0/2
GigabitEthernet0/3
```

Instead of repeating the config, write one loop:

### Template:
```
{% for i in interfaces %}
interface {{ i }}
 description access port
!
{% endfor %}
```

### Data:
```yaml
interfaces:
  - GigabitEthernet0/1
  - GigabitEthernet0/2
  - GigabitEthernet0/3
```

### Output:
```
interface GigabitEthernet0/1
 description access port
!
interface GigabitEthernet0/2
 description access port
!
interface GigabitEthernet0/3
 description access port
!
```

Automating repetitive config… *done*.

---

# 🧽 5. What Does the Hyphen `-%}` Mean?

Jinja2 loop tags like:

```
{% for item in list %}
```

Sometimes add **extra blank lines**.

Network configs hate messy spacing.

So we use the **hyphen**:

- `{% for ... -%}` → removes space *before* the line  
- `{%- endfor %}` → removes space *after* the line  

Example with hyphen:

```
{% for i in interfaces -%}
interface {{ i }}
{%- endfor %}
```

Result:  
✔ clean  
✔ compact  
✔ zero extra blank lines  

---

# 📘 6. What is YAML? (Beginner Explanation)

YAML is a **human‑friendly way to store data**.

It's easier than JSON for humans to read.

Example YAML:

```yaml
loopbacks:
  - id: 0
    ip: 1.1.1.1
    mask: 255.255.255.255
```

Means in English:

- There is a list called `loopbacks`  
- Each item has:
  - `id`
  - `ip`
  - `mask`

Jinja2 uses this data to fill templates.

---

# 🧠 7. How Python Loads YAML + Jinja2 (Explained Super Simply)

This code:

```python
import yaml
from jinja2 import Environment, FileSystemLoader

data = yaml.safe_load(open("data/loopbacks.yaml"))
env = Environment(loader=FileSystemLoader("templates"))
template = env.get_template("loopbacks.j2")
```

Let’s break it down step‑by‑step:

---

## 🔹 Step 1 — `import yaml`

Python doesn’t understand YAML by itself.  
This library helps Python *read YAML files*.

---

## 🔹 Step 2 — Import Jinja2 Tools

```
from jinja2 import Environment, FileSystemLoader
```

- **Environment** → controls Jinja2 engine  
- **FileSystemLoader** → tells Jinja2 where to find template files  

---

## 🔹 Step 3 — Load the YAML Data

```
data = yaml.safe_load(open("data/loopbacks.yaml"))
```

This does:

1. Open the YAML file  
2. Convert YAML → Python objects  

Now `data` becomes a Python dictionary like:

```python
{'loopbacks': [{'id': 0, 'ip': '1.1.1.1'}]}
```

---

## 🔹 Step 4 — Create Jinja2 Environment

```
env = Environment(loader=FileSystemLoader("templates"))
```

Means:
> “Look inside the *templates/* folder for `.j2` files.”

---

## 🔹 Step 5 — Load the Template File

```
template = env.get_template("loopbacks.j2")
```

This loads the `.j2` template so we can fill it with the data.

---

# 🛠 8. Full Example Bringing Everything Together

## YAML → loopbacks.yaml
```yaml
loopbacks:
  - id: 0
    ip: 1.1.1.1
    mask: 255.255.255.255
  - id: 1
    ip: 2.2.2.2
    mask: 255.255.255.255
```

---

## Template → loopbacks.j2
```
{% for lo in loopbacks -%}
interface Loopback{{ lo.id }}
 ip address {{ lo.ip }} {{ lo.mask }}
!
{% endfor %}
```

---

## Python → generate.py
```python
import yaml
from jinja2 import Environment, FileSystemLoader

data = yaml.safe_load(open("data/loopbacks.yaml"))
env = Environment(loader=FileSystemLoader("templates"))
template = env.get_template("loopbacks.j2")

print(template.render(**data))
```

---

## Final Output:
```
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface Loopback1
 ip address 2.2.2.2 255.255.255.255
!
```

---
