TOPIC COVERED:

1. Creating a file  
2. Writing to a file  
3. Reading from a file (with all major methods)  
4. Understanding the **type of data** each method returns

---

## 🧱 1. Create a File

### Example: Creating a new file named `network_devices.txt`

```python
# Create a new file or overwrite existing one
device_file = open("network_devices.txt", "w")  # 'w' = write mode

device_file.write("192.168.1.1\n")  # Add first device IP
device_file.close()

print("File created successfully.")
```

**Expected Output:**
```
File created successfully.
```

---

## 🧾 2. Write to a File

### Example 1 – Append using `open()`

```python
# Append new device IPs to the existing file
device_file = open("network_devices.txt", "a")  # 'a' = append mode (keeps existing data)
device_file.write("192.168.1.2\n")
device_file.write("192.168.1.3\n")
device_file.close()

print("Device IPs added to file.")
```

**Expected Output:**
```
Device IPs added to file.
```

---

### Example 2 – Append using `with open()`

```python
# Using 'with' automatically closes the file after writing
with open("network_devices.txt", "a") as device_file:
    device_file.write("192.168.1.4\n")
    device_file.write("192.168.1.5\n")

print("More IPs added successfully.")
```

**Expected Output:**
```
More IPs added successfully.
```

---

## 📖 3. Read from a File (All Methods)

We’ll use the same file created above and explore multiple reading methods.

---

### **Method 1 – `read()` (Reads Entire File as a Single String)**

```python
with open("network_devices.txt", "r") as device_file:
    content = device_file.read()  # Reads the whole file at once
    print(content)
    print("Type of variable:", type(content))
```

**Expected Output:**
```
192.168.1.1
192.168.1.2
192.168.1.3
192.168.1.4
192.168.1.5
Type of variable: <class 'str'>
```

👉 The `.read()` method returns a **string**.

---

### **Method 2 – `readline()` (Reads One Line at a Time)**

```python
device_file = open("network_devices.txt", "r")

first_line = device_file.readline()
second_line = device_file.readline()
device_file.close()

print("First line:", first_line.strip())
print("Second line:", second_line.strip())
print("Type of variable:", type(first_line))
```

**Expected Output:**
```
First line: 192.168.1.1
Second line: 192.168.1.2
Type of variable: <class 'str'>
```

👉 `.readline()` returns **a single line** as a **string**.

---

### **Method 3 – `readlines()` (Reads All Lines into a List)**

```python
with open("network_devices.txt", "r") as device_file:
    all_lines = device_file.readlines()  # Reads all lines as list elements
    print(all_lines)
    print("Type of variable:", type(all_lines))
```

**Expected Output:**
```
['192.168.1.1\n', '192.168.1.2\n', '192.168.1.3\n', '192.168.1.4\n', '192.168.1.5\n']
Type of variable: <class 'list'>
```

👉 `.readlines()` returns a **list of strings**, one per line.

---

### **Method 4 – Looping Through Each Line (Best for Processing)**

```python
device_file = open("network_devices.txt", "r")

for line in device_file:
    print("Connecting to device:", line.strip())  # strip() removes newline at end

device_file.close()
```

**Expected Output:**
```
Connecting to device: 192.168.1.1
Connecting to device: 192.168.1.2
Connecting to device: 192.168.1.3
Connecting to device: 192.168.1.4
Connecting to device: 192.168.1.5
```

---

### **Method 5 – Using `with open()` for Safe Reading**

```python
# 'with' ensures file closure even if an error occurs
with open("network_devices.txt", "r") as device_file:
    for device in device_file:
        print("Pinging device:", device.strip())
```

**Expected Output:**
```
Pinging device: 192.168.1.1
Pinging device: 192.168.1.2
Pinging device: 192.168.1.3
Pinging device: 192.168.1.4
Pinging device: 192.168.1.5
```

---

## 🧠 Summary of File Reading Methods

| Method | Returns | Data Type | Description |
|--------|----------|------------|-------------|
| `read()` | Entire file | `str` | Reads everything as one string |
| `readline()` | One line | `str` | Reads the next line each time it’s called |
| `readlines()` | All lines | `list` | Reads all lines into a list |
| `for line in file:` | Each line | `str` | Iterates over each line efficiently |

---

## ✅ Best Practices
- Use `with open()` — automatically closes the file after use.  
- Always check the **data type** to understand how content is returned.  
- Use `.strip()` to remove newline characters (`\n`) when reading.  
- Prefer `.readlines()` or loops for processing multiple lines.

---
