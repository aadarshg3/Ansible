======================================================
## 🔹 String's Method : upper | lower
======================================================
**🧠 Code:**
```python
s = "hello world"
print(s.upper())  # Convert to uppercase
```
**📤 Output:**
```text
HELLO WORLD
```
**🧠 Code:**
```python
intf_name = "GigabitEthernet0/1"

if intf_name == "GIGABITETHERNET0/1":
    print("Interface found")
else:
    print("Interface NOT found")

if intf_name.upper() == "GIGABITETHERNET0/1":
    print("Interface found")

if intf_name.lower() == "gigabitethernet0/1":
    print("Interface found")
```
**📤 Output:**
```text
Interface NOT found
Interface found
Interface found
```
======================================================
## 🔹 Indexing and Advanced Slicing
======================================================
**🧠 Code:**
```python
device = "ROUTER"

# Fetch by index
print(device[0])     # 'R'
print(device[3])     # 'T'

# Negative indexing
print(device[-1])    # 'R'
print(device[-2])    # 'E'

# Slicing
print(device[1:4])   # 'OUT'
print(device[:3])    # 'ROU'
print(device[2:])    # 'UTER'

# Step slicing
print(device[::2])   # 'RUE'
print(device[1::2])  # 'OTR'

# Reversing
print(device[::-1])      # 'RETUOR'
print(device[-1::-2])    # 'RTE'
print(device[5:1:-1])    # 'RETU'
```
======================================================
## 🔹 String Concatenation
======================================================
**🧠 Code:**
```python
vlan = "vlan"
vlan_id = "10"
result = vlan + vlan_id
print(result)

vlan_name = "Accounting"
result = vlan + " " + vlan_name
print(result)

# Common Error
vlan_id = 10
# result = vlan + vlan_id  # ❌ TypeError
result = vlan + str(vlan_id)  # ✅ Correct
print(result)
```
**📤 Output:**
```text
vlan10
vlan Accounting
vlan10
```
======================================================
## 🔹 Typecasting Examples
======================================================

## ✅ Example 1: Converting VLAN ID (str → int)
```python
vlan_id_str = "20"              # From YAML or user input
vlan_id_int = int(vlan_id_str)  # Convert to int for math/range check

if vlan_id_int > 1 and vlan_id_int < 4095:
    print("Valid VLAN ID")
```
```text
Valid VLAN ID
```

## ✅ Example 2: Device ID (int → str for hostname)
```python
device_id = 5
hostname = "Router" + str(device_id)
print(hostname)
```
```text
Router5
```

## ✅ Example 3: Convert list of IPs to string
```python
ip_list = ["192.168.1.1", "192.168.1.2"]
ip_string = ", ".join(ip_list)
print("Devices found at: " + ip_string)
```
```text
Devices found at: 192.168.1.1, 192.168.1.2
```

## ⚠️ Common Typecasting Errors
### ❌ Error 1: Non-numeric string to int
```python
vlan_id = "twenty"
vlan_number = int(vlan_id)
```
```text
ValueError: invalid literal for int() with base 10: 'twenty'
```

### ❌ Error 2: Concatenating int with str without conversion
```python
device_number = 1
hostname = "Switch" + device_number  # ❌
```
```text
TypeError: can only concatenate str (not "int") to str
```

### ✅ Fix: Use str()
```python
hostname = "Switch" + str(device_number)
```
## 🔄 Classic Example:
```python
num = 10
"vlan" + num           # ❌ Error
"vlan " + str(num)     # ✅ Works
```
```text
TypeError: can only concatenate str (not "int") to str
```

✅ Why it works:
- `str(num)` converts integer to string.
- `"vlan " + "10"` = `vlan 10`


