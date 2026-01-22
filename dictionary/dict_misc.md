# dict_methods_misc.md — Dictionary Methods
---

## 1) `get`
**What it does:** Safely fetches a value. Returns `None` or default if key doesn’t exist.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
print(intf.get("desc"))
print(intf.get("speed"))
print(intf.get("speed", 1000))
```
**Expected Output**
```text
Uplink
None
1000
```

---

## 2) `setdefault`
**What it does:** Inserts key with default if missing.

**Code**
```python
intf = {"name": "Gi0/1"}
intf.setdefault("desc", "To-Core")
print(intf)
```
**Expected Output**
```text
{'name': 'Gi0/1', 'desc': 'To-Core'}
```

---

## 3) `update`
**What it does:** Adds or updates values from another dict.

**Code**
```python
intf = {"name": "Gi0/1"}
intf.update({"desc": "Uplink", "speed": 1000})
print(intf)
```
**Expected Output**
```text
{'name': 'Gi0/1', 'desc': 'Uplink', 'speed': 1000}
```

---

## 4) `keys`
**What it does:** Lists all the field names.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
print(list(intf.keys()))
```
**Expected Output**
```text
['name', 'desc']
```

---

## 5) `values`
**What it does:** Lists all values.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
print(list(intf.values()))
```
**Expected Output**
```text
['Gi0/1', 'Uplink']
```

---

## 6) `items`
**What it does:** Returns (key, value) pairs.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
for k, v in intf.items():
    print(k, v)
```
**Expected Output**
```text
name Gi0/1
desc Uplink
```

---

## 7) `pop`
**What it does:** Removes key and returns value.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
intf.pop("desc")
print(intf)
```
**Expected Output**
```text
{'name': 'Gi0/1'}
```

---

## 8) `popitem`
**What it does:** Removes the last inserted item.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
intf.popitem()
print(intf)
```
**Expected Output**
```text
{'name': 'Gi0/1'}
```

---

## 9) `clear`
**What it does:** Deletes everything inside the dict.

**Code**
```python
intf = {"name": "Gi0/1", "desc": "Uplink"}
intf.clear()
print(intf)
```
**Expected Output**
```text
{}
```

---

## 10) `copy`
**What it does:** Creates a shallow copy.

**Code**
```python
intf = {"name": "Gi0/1"}
backup = intf.copy()
print(backup)
```
**Expected Output**
```text
{'name': 'Gi0/1'}
```

---

## 11) `fromkeys`
**What it does:** Creates dict from list of keys with a default value.

**Code**
```python
ports = ["Gi0/1", "Gi0/2"]
shutdown_state = dict.fromkeys(ports, "shutdown")
print(shutdown_state)
```
**Expected Output**
```text
{'Gi0/1': 'shutdown', 'Gi0/2': 'shutdown'}
```

---

## 12) Merge (`|`) — Python 3.9+
**What it does:** Combines two dicts into a new one.

**Code**
```python
defaults = {"speed": 1000}
override = {"desc": "Uplink"}
merged = defaults | override
print(merged)
{'speed': 1000, 'desc': 'Uplink'}
>>> 
