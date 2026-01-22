# dict1.md — Python Dictionary


## 1) Define a dictionary & check type

**Code**
```python
intf_data = { "intf": "Gig0/1", "bandwidth": 1000, "speed": 1000, "duplex": "full"}
print(type(intf_data))
print(intf_data)
```
**Expected Output**
```text
<class 'dict'>
{'intf': 'Gig0/1', 'bandwidth': 1000, 'speed': 1000, 'duplex': 'full'}
```

---

## 2) Access values by key (direct vs safe)

**Code**
```python
print(intf_data["bandwidth"])   # direct key access
print(intf_data["duplex"])      # direct key access

# Missing key (direct access)
# print(intf_data["interface"])  # would raise KeyError

print(intf_data.get("intf"))                 # safe lookup
print(intf_data.get("interface"))            # safe lookup → None
print(intf_data.get("interface", "Value not found"))
print(intf_data.get("intf", "Value not found"))
print(intf_data.get("interface", "KEY NOT FOUND"))
```
**Expected Output**
```text
1000
full
Gig0/1
None
Value not found
Gig0/1
KEY NOT FOUND
```
**Notes (if error)**
- `intf_data["interface"]` would raise **KeyError**.

---

## 3) Iterate over a dict (keys, values, items)

**Code**
```python
for key, value in intf_data.items():
    print(key, value)

print(intf_data.items())
print(list(intf_data.items()))

# Index into the list of (key, value) tuples
print(list(intf_data.items())[0])
print(list(intf_data.items())[1])
print(list(intf_data.items())[2])
print(list(intf_data.items())[3])

print(list(intf_data.items())[0][0])  # tuple index: key
print(list(intf_data.items())[0][1])  # tuple index: value
```
**Expected Output**
```text
intf Gig0/1
bandwidth 1000
speed 1000
duplex full

dict_items([('intf', 'Gig0/1'), ('bandwidth', 1000), ('speed', 1000), ('duplex', 'full')])
[('intf', 'Gig0/1'), ('bandwidth', 1000), ('speed', 1000), ('duplex', 'full')]
('intf', 'Gig0/1')
('bandwidth', 1000)
('speed', 1000)
('duplex', 'full')
intf
Gig0/1
```
**Notes (if error)**
- Using `intf_data.item()` instead of `intf_data.items()` raises **AttributeError** (method name is plural).

---

## 4) Keys & values views

**Code**
```python
print(intf_data.keys())
print(intf_data.values())
```
**Expected Output**
```text
dict_keys(['intf', 'bandwidth', 'speed', 'duplex'])
dict_values(['Gig0/1', 1000, 1000, 'full'])
```

---

## 5) Nested dictionary: device block

**Code**
```python
data = {"devices": { "intf": "Gig0/1", "bandwidth": 1000, "speed": 1000, "duplex": "full"}}
print(data["devices"])       
print(data["devices"]["duplex"]) 
```
**Expected Output**
```text
{'intf': 'Gig0/1', 'bandwidth': 1000, 'speed': 1000, 'duplex': 'full'}
full
```

---

## 6) Complex nested structure: deep indexing

**Code**
```python
dict_data = {
    "data1": "This is the second class for dictionary",
    "data2": ["k", "v", 10, [ {"a": "a1"}, {"b": "b22"}, {"c": "c1"}], 200, 20.3],
    "data3": {"key1": "Hello world 1",
              "key2": ["Ravi", "Prince", 10, [ {"a": "a1"}, [{"b": "b22"}, {"c": [1,2,3,4]}]], 200, 20.3],
              "key3": "Hello world 3",
    },
    "data4": [ {"a": "a1"}, {"b": {"key1": "Hello world 1",
              "key2": ["Ravi", "Prince", 10, [ {"a": "a1"}, [{"b": [ {"a": "a111"}, {"b": "b22"}, {"c": "c1"}]}, {"c": [1,2,3,4]}]], 2000, 20.3],
              "key3": "Hello world 4",
              }}, {"c": "c1"}],
}

print(dict_data["data4"])                     # outer list
print(dict_data["data4"][0])                  # first dict in data4
print(dict_data["data4"][1])                  # second dict in data4
print(dict_data["data4"][1]["b"])            # nested 'b' dict
# print(dict_data["data4"][1]["b"]["k1"])  # would raise KeyError (wrong key)
print(dict_data["data4"][1]["b"]["key1"])  
print(dict_data["data4"][1]["b"]["key2"])  
print(dict_data["data4"][1]["b"]["key2"][3])
print(dict_data["data4"][1]["b"]["key2"][3][0])
print(dict_data["data4"][1]["b"]["key2"][3][1])
print(dict_data["data4"][1]["b"]["key2"][3][1][0])
print(dict_data["data4"][1]["b"]["key2"][3][1][0]["b"]) 
print(dict_data["data4"][1]["b"]["key2"][3][1][0]["b"][0])
print(dict_data["data4"][1]["b"]["key2"][3][1][0]["b"][0]["a"])  

print(int(dict_data['data4'][1]["b"]['key2'][-1]))  # deep index: last item of key2 (20.3) → int(20.3)=20
```
**Expected Output**
```text
[{'a': 'a1'}, {'b': {'key1': 'Hello world 1', 'key2': ['Ravi', 'Prince', 10, [{'a': 'a1'}, [{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}, {'c': [1, 2, 3, 4]}]], 2000, 20.3], 'key3': 'Hello world 4'}}, {'c': 'c1'}]
{'a': 'a1'}
{'b': {'key1': 'Hello world 1', 'key2': ['Ravi', 'Prince', 10, [{'a': 'a1'}, [{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}, {'c': [1, 2, 3, 4]}]], 2000, 20.3], 'key3': 'Hello world 4'}}
{'key1': 'Hello world 1', 'key2': ['Ravi', 'Prince', 10, [{'a': 'a1'}, [{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}, {'c': [1, 2, 3, 4]}]], 2000, 20.3], 'key3': 'Hello world 4'}
Hello world 1
['Ravi', 'Prince', 10, [{'a': 'a1'}, [{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}, {'c': [1, 2, 3, 4]}]], 2000, 20.3]
[{'a': 'a1'}, [{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}, {'c': [1, 2, 3, 4]}]]
{'a': 'a1'}
[{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}, {'c': [1, 2, 3, 4]}]
{'b': [{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]}
[{'a': 'a111'}, {'b': 'b22'}, {'c': 'c1'}]
{'a': 'a111'}
a111
20
```
**Notes (if error)**
- `dict_data["data4"][1]["b"]["k1"]` would raise **KeyError** (typo; valid key is `key1`).

---

## 7) Dict with a list value (tuples list under a key)

**Code**
```python
intf_data = { "intf": "Gig0/1", 
             "bandwidth": [('intf', 'Gig0/1'), ('bandwidth', 500), ('speed', 1000), ('duplex', 'full')],
             "speed": 1000, "duplex": "full"}

print(intf_data)
print(intf_data["intf"])     
print(intf_data["bandwidth"]) 
print(intf_data.keys())
print(intf_data.values())
print(intf_data.items())
```
**Expected Output**
```text
{'intf': 'Gig0/1', 'bandwidth': [('intf', 'Gig0/1'), ('bandwidth', 500), ('speed', 1000), ('duplex', 'full')], 'speed': 1000, 'duplex': 'full'}
Gig0/1
[('intf', 'Gig0/1'), ('bandwidth', 500), ('speed', 1000), ('duplex', 'full')]
dict_keys(['intf', 'bandwidth', 'speed', 'duplex'])
dict_values(['Gig0/1', [('intf', 'Gig0/1'), ('bandwidth', 500), ('speed', 1000), ('duplex', 'full')], 1000, 'full'])
dict_items([('intf', 'Gig0/1'), ('bandwidth', [('intf', 'Gig0/1'), ('bandwidth', 500), ('speed', 1000), ('duplex', 'full')]), ('speed', 1000), ('duplex', 'full')])
```

---

## 8) Common errors from the session — short notes

**Code with errors (do not run in production):**
```python
for key, value in intf_data.item():
    print(key, value)
# AttributeError: 'dict' object has no attribute 'item'. Did you mean: 'items'?

intf_data = { ... }}
# SyntaxError: unmatched '}'

intf["intf"]
# NameError: name 'intf' is not defined

# From earlier REPL (layout issue, not dict-specific):
    print(item)
# IndentationError: unexpected indent

# Manual key miss:
# intf_data["interface"]  → KeyError: 'interface'

# Long loop accidentally interrupted:
# for key, value in intf_data.items(): ...  → KeyboardInterrupt
```
**Notes**
- Prefer `dict.get()` for optional keys.
- Watch closing braces in nested literals.
- Use the correct variable name (`intf_data`).

---

> End of dict1.md. Next up: **dict2.md** (sorting by value, dict comprehensions, merging (`|`), JSON/YAML I/O, `setdefault` patterns at scale).

