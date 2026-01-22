# 📝 Python List Data Type
==

## 🔹 How to Define a List

**🧠 Code:**
```python
# A list of devices
device_list = ["R1", "R2", "R3", "R4"]

# A list with mixed data types
device_info = ["switch", 10, 15.5, "24-8-2025", "cisco"]
print(device_list)
print(device_info)
```
**📤 Output:**
```text
['R1', 'R2', 'R3', 'R4']
['switch', 10, 15.5, '24-8-2025', 'cisco']
```

---

## 🔹 Accessing List Items

**🧠 Code:**
```python
print(device_info[0])     # first element
print(device_info[2])     # float value
print(device_info[-1])    # last element
print(device_info[-2])    # second last element
```
**📤 Output:**
```text
switch
15.5
cisco
24-8-2025
```

---

## 🔹 Slicing Lists

**🧠 Code:**
```python
print(device_info[0:3])   # first 3 items
print(device_info[2:5])   # from index 2 to 4
print(device_info[:-1])   # all except last
print(device_info[::-1])  # reversed list
```
**📤 Output:**
```text
['switch', 10, 15.5]
[15.5, '24-8-2025', 'cisco']
['switch', 10, 15.5, '24-8-2025']
['cisco', '24-8-2025', 15.5, 10, 'switch']
```

---

## 🔹 Creating and Updating Lists

**🧠 Code:**
```python
newlist = []                 # start empty
newlist.append("Router")     # add one item
newlist.append("Switch")
newlist.append("FW")
newlist.append("LB")
print(newlist)
```
**📤 Output:**
```text
['Router', 'Switch', 'FW', 'LB']
```

---

## 🔹 Insert at a Specific Position

**🧠 Code:**
```python
newlist = ["Router", "Switch"]
newlist.insert(1, "FW")   # insert at index 1
print(newlist)
```
**📤 Output:**
```text
['Router', 'FW', 'Switch']
```

---

## 🔹 Iterating Through a List

**🧠 Code:**
```python
device_info = ["switch", 10, 15.5, "24-8-2025", "cisco"]

for item in device_info:
    print(item)
```
**📤 Output:**
```text
switch
10
15.5
24-8-2025
cisco
```

---

## 🔹 Common Errors

**🧠 Code:**
```python
newlist.append("switch" , 1)
# ❌ TypeError: list.append() takes exactly one argument (2 given)

newlist.insert("LB")
# ❌ TypeError: insert expected 2 arguments, got 1
```
**📤 Output:**
```text
TypeError: list.append() takes exactly one argument (2 given)
TypeError: insert expected 2 arguments, got 1
```

👉 Explanation:  
- `append()` can only add **one item at a time**; passing more than one argument causes an error.  
- `insert()` requires **two arguments**: the index position and the item to insert.  
