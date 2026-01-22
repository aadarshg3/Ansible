# 📝 Python List – daily used methods 
==
## 🔹 `append()` — add one item
**🧠 Code:**
```python
vlan_list = [10, 20, 30]
vlan_list.append(40)          # add a single VLAN
print(vlan_list)
```
**📤 Output:**
```text
[10, 20, 30, 40]
```

---

## 🔹 `extend()` — add many items
**🧠 Code:**
```python
base_cmds = ["conf t", "interface gi0/1"]
extra_cmds = ["switchport mode access", "switchport access vlan 10"]
base_cmds.extend(extra_cmds)  # merge lists
print(base_cmds)
```
**📤 Output:**
```text
['conf t', 'interface gi0/1', 'switchport mode access', 'switchport access vlan 10']
```

---

## 🔹 `insert(index, item)` — insert at a position
**🧠 Code:**
```python
intf_cmds = ["interface gi0/1", "switchport mode trunk"]
intf_cmds.insert(1, "description Uplink")  # place description after interface
print(intf_cmds)
```
**📤 Output:**
```text
['interface gi0/1', 'description Uplink', 'switchport mode trunk']
```

---

## 🔹 `pop([index])` — remove & return item
**🧠 Code:**
```python
pending_devices = ["sw1", "sw2", "sw3"]
device = pending_devices.pop()        # removes last
print(device)                         # show which device was taken
print(pending_devices)
```
**📤 Output:**
```text
sw3
['sw1', 'sw2']
```

---

## 🔹 `remove(value)` — delete first match
**🧠 Code:**
```python
intf_list = ["gi0/1", "gi0/2", "gi0/2", "gi0/3"]
intf_list.remove("gi0/2")             # removes first occurrence only
print(intf_list)
```
**📤 Output:**
```text
['gi0/1', 'gi0/2', 'gi0/3']
```
> Short note: if value not present → `ValueError` (catch with `if "x" in list:`).

---

## 🔹 `clear()` — wipe the list
**🧠 Code:**
```python
session_log = ["login sw1", "push conf", "save"]
session_log.clear()                   # list becomes empty
print(session_log)
```
**📤 Output:**
```text
[]
```

---

## 🔹 `index(value[, start[, end]])` — find position
**🧠 Code:**
```python
devices = ["sw1", "sw2", "sw3", "sw2"]
print(devices.index("sw2"))           # first position of 'sw2'
```
**📤 Output:**
```text
1
```
> Short note: if not found → `ValueError` (use `"swX" in devices` first).

---

## 🔹 `count(value)` — frequency
**🧠 Code:**
```python
states = ["up", "down", "up", "errdisable", "up"]
print(states.count("up"))             # how many links are up
```
**📤 Output:**
```text
3
```

---

## 🔹 `sort(key=None, reverse=False)` — in-place sort
**🧠 Code:**
```python
# Sort VLANs numerically
vlans = [20, 1, 100, 10]
vlans.sort()
print(vlans)

# Sort interfaces by numeric part after '/'
intfs = ["gi0/10", "gi0/2", "gi0/1"]
intfs.sort(key=lambda x: int(x.split('/')[1]))  # important: numeric sort
print(intfs)

# Descending
vlans.sort(reverse=True)
print(vlans)
```
**📤 Output:**
```text
[1, 10, 20, 100]
['gi0/1', 'gi0/2', 'gi0/10']
[100, 20, 10, 1]
```

---

## 🔹 `reverse()` — reverse in-place
**🧠 Code:**
```python
recent_logs = ["log3", "log2", "log1"]
recent_logs.reverse()
print(recent_logs)                    # newest-first → oldest-first
```
**📤 Output:**
```text
['log1', 'log2', 'log3']
```

---

## 🔹 `copy()` — shallow copy
**🧠 Code:**
```python
base_acl = ["permit tcp any any eq 22", "permit icmp any any"]
acl_copy = base_acl.copy()            # safe copy before editing
acl_copy.append("deny ip any any log")
print(base_acl)                       # original intact
print(acl_copy)
```
**📤 Output:**
```text
['permit tcp any any eq 22', 'permit icmp any any']
['permit tcp any any eq 22', 'permit icmp any any', 'deny ip any any log']
```

---

## 🔹 `len()` (built-in) — size of list
**🧠 Code:**
```python
ip_list = ["10.2.3.4", "10.3.4.5", "172.16.1.1"]
print(len(ip_list))                   # number of IPs to process
```
**📤 Output:**
```text
3
```

---

## 🔹 Copy vs Reference (important)
**🧠 Code:**
```python
a = ["sw1", "sw2"]
b = a                 # reference, both point to same list
c = a.copy()          # independent copy

a.append("sw3")
print(a)              # ['sw1', 'sw2', 'sw3']
print(b)              # ['sw1', 'sw2', 'sw3']  ← changed too
print(c)              # ['sw1', 'sw2']        ← unchanged
```
**📤 Output:**
```text
['sw1', 'sw2', 'sw3']
['sw1', 'sw2', 'sw3']
['sw1', 'sw2']
```

---

## 🔹 Remove items by slice (handy)
**🧠 Code:**
```python
logs = ["L1", "L2", "L3", "L4", "L5"]
del logs[1:4]                            # drop a middle block
print(logs)
```
**📤 Output:**
```text
['L1', 'L5']
```

---

## 🔹 Build lists quickly (bonus)
> Not methods, but useful in daily work.

**🧠 Code:**
```python
# List comprehension: build config lines
vlans = [10, 20, 30]
vlan_cmds = [f"vlan {v}\n name vlan_{v}" for v in vlans]
print(vlan_cmds)

# enumerate(): get index + value
for i, dev in enumerate(["sw1", "sw2", "sw3"], start=1):
    print(i, dev)
```
**📤 Output:**
```text
['vlan 10\n name vlan_10', 'vlan 20\n name vlan_20', 'vlan 30\n name vlan_30']
1 sw1
2 sw2
3 sw3
```

---

## ✅ Quick Recap
- **Mutators**: `append`, `extend`, `insert`, `pop`, `remove`, `clear`, `sort`, `reverse`
- **Queries**: `index`, `count`, `len` (built-in)
- **Safety**: use `copy()` before edits; beware references.
- **Patterns**: slicing and list comprehensions speed up config generation.
