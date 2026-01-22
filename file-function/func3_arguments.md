# 🧠 Python Function Arguments (func3_arguments)

This documentation contains the use of **positional**, **variable positional**, **keyword**, and **keyword variable** arguments.
---

## ⚙️ 1. Positional Arguments

### 🧩 Problem
When writing simple functions, we often know exactly how many values we need.  
For example, adding two numbers or connecting to a fixed device type and IP.

### 💡 Reason
Positional arguments enforce **order** and **count**, ensuring we pass all required parameters.

### 🧰 Solution
```python
def add(x, y):
    s = x + y
    return s

temp_var = add(20, 30)
print(temp_var)
print(type(temp_var))
```

### ✅ Output
```
50
<class 'int'>
```

### ⚙️ Common Error (Missing Argument)
```python
temp_var = add(20)
# TypeError: add() missing 1 required positional argument: 'y'
```

### ⚙️ Common Error (Extra Argument)
```python
temp_var = add(20, 30, 50)
# TypeError: add() takes 2 positional arguments but 3 were given
```

### 🌐 Netmiko-Style Example
When defining a connection with fixed fields like `device_type` and `host`:
```python
def connect_device(device_type, host):
    return f"Connecting to {host} using {device_type}"

print(connect_device('cisco_ios', '10.10.10.10'))
```
**Output:**
```
Connecting to 10.10.10.10 using cisco_ios
```
> In libraries like **Netmiko**, these are fixed required parameters — their order matters.

---

## 🧮 2. Variable Positional Arguments (`*args`)

### 🧩 Problem
You may not always know how many items you’ll receive — for instance, connecting to multiple devices or summing several metrics.

### 💡 Reason
`*args` allows your function to take **any number of positional values**.  
All values are collected into a **tuple**.

### 🧰 Solution
```python
def add(*vars):
    return vars

temp_var = add(20, 30, 400)
print(temp_var)
print(type(temp_var))
```

### ✅ Output
```
(20, 30, 400)
<class 'tuple'>
```

### ⚙️ Common Error (No Arguments)
```python
temp_var = add()
print(temp_var)
# Output: ()
# () -> Empty tuple means no data passed, no error
```

### 🌐 Netmiko-Style Example
Passing multiple device IPs dynamically:
```python
def connect_multiple(*hosts):
    for host in hosts:
        print(f"Preparing to connect: {host}")

connect_multiple('10.10.10.1', '10.10.10.2', '10.10.10.3')
```
**Output:**
```
Preparing to connect: 10.10.10.1
Preparing to connect: 10.10.10.2
Preparing to connect: 10.10.10.3
```
> This flexibility becomes useful when iterating through multiple devices in an inventory file.

---

## 🔑 3. Keyword Arguments

### 🧩 Problem
When functions have many parameters, order can become confusing.  
Example: connecting to a device using `host`, `username`, `password`, `port`, etc.

### 💡 Reason
Keyword arguments let you pass data **by name**, removing order dependency.

### 🧰 Solution
```python
def add(x, y, z):
    return x + y + z

sum_val = add(x=10, y=20, z=30)
print(sum_val)
print(type(sum_val))
```

### ✅ Output
```
60
<class 'int'>
```

### ⚙️ Common Error
```python
add(x=10, z=30)
# TypeError: add() missing 1 required positional argument: 'y'
```

### 🌐 Netmiko-Style Example
Building device details for connection:
```python
def device_info(device_type, host, username, password):
    return f"{device_type} device at {host} logged in as {username}"

print(device_info(device_type='cisco_ios', host='10.10.10.10', username='test', password='pass'))
```
**Output:**
```
cisco_ios device at 10.10.10.10 logged in as test
```
> In real network automation, keyword arguments are used to pass connection parameters clearly to libraries like Netmiko.

---

## 🧰 4. Keyword Variable Arguments (`**kwargs`)

### 🧩 Problem
Different devices require different sets of configuration details — you can’t predict all parameter names in advance.

### 💡 Reason
`**kwargs` lets you accept **variable keyword arguments** (key=value pairs) and packs them into a **dictionary**.

### 🧰 Solution
```python
def add(**kvars):   
    return kvars

sum_val = add(a="ip1", b="ip2", c="ip3") 
print(sum_val)
print(type(sum_val))
```

### ✅ Output
```
{'a': 'ip1', 'b': 'ip2', 'c': 'ip3'}
<class 'dict'>
```

### 🌐 Netmiko-Style Example
Building a connection dictionary dynamically:
```python
def connect_handler(**device):
    return device

cisco_881 = {
    'device_type': 'cisco_ios',
    'host': '10.10.10.10',
    'username': 'test',
    'password': 'password',
    'port': 8022,
    'secret': 'secret'
}

print(connect_handler(**cisco_881))
```
**Output:**
```
{'device_type': 'cisco_ios', 'host': '10.10.10.10', 'username': 'test', 'password': 'password', 'port': 8022, 'secret': 'secret'}
```
> This is how **Netmiko’s** `ConnectHandler(**device)` works — it unpacks all key-value pairs dynamically.

**Error Example**
```python
connect_handler("cisco_ios")
# TypeError: connect_handler() takes 0 positional arguments but 1 was given
```
> Meaning: `**kwargs` only accepts keyword pairs, not plain positional values.

---

## 🧭 Argument Order in Python

When defining a function, argument order **must always be**:

```
def func(positional, *args, keyword_defaults=None, **kwargs):
```

✅ **Execution Order**
1. Positional arguments  
2. `*args` (variable positional)  
3. Keyword arguments (with defaults)  
4. `**kwargs` (variable keyword)

### 🧰 Example
```python
def network_add(x, *args, hostname=None, **kwargs):
    return x, args, hostname, kwargs

print(network_add(10, 20, 30, hostname="R1", location="DataCenter1"))
```

**Output**
```
(10, (20, 30), 'R1', {'location': 'DataCenter1'})
```

### 🌐 Netmiko-Style Hint
When connecting to multiple devices:
```python
def netmiko_connect(device_type, *ips, username=None, **params):
    return device_type, ips, username, params

print(netmiko_connect('cisco_ios', '10.10.10.1', '10.10.10.2', username='admin', port=22, secret='lab'))
```
**Output**
```
('cisco_ios', ('10.10.10.1', '10.10.10.2'), 'admin', {'port': 22, 'secret': 'lab'})
```
> Such combinations let you scale up your connection logic dynamically.

---

## 🧩 Common Errors Summary

| Type | Mistake | Example | Error | Meaning |
|------|----------|----------|--------|----------|
| Missing positional | Missing an argument | `add(20)` | `TypeError: missing 1 required positional argument` | Required argument not passed |
| Extra positional | Too many arguments | `add(20, 30, 50)` | `TypeError: takes 2 positional arguments but 3 were given` | Too many values |
| Wrong keyword | Invalid name | `add(x1=20, y=30)` | `TypeError: got an unexpected keyword argument 'x1'` | Invalid keyword |
| Keyword before positional | Wrong order | `add(x=20, 30)` | `SyntaxError: positional argument follows keyword argument` | Wrong sequence of arguments |

---

## 🧠 Summary Table

| Argument Type | Syntax | Data Type | Example | Output |
|----------------|--------|------------|----------|----------|
| Positional | `def add(x, y)` | Any | `add(20, 30)` | 50 |
| Variable Positional | `def add(*vars)` | tuple | `add(20, 30, 400)` | (20, 30, 400) |
| Keyword | `def add(x, y, z)` | int | `add(x=10, y=20, z=30)` | 60 |
| Keyword Variable | `def add(**kvars)` | dict | `add(a="ip1", b="ip2", c="ip3")` | {'a': 'ip1', 'b': 'ip2', 'c': 'ip3'} |
