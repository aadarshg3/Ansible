# 🧵 String data type
==

## 🔹 How to define a "string"


**🧠 Code:**
```python
# Different ways to create strings
single_quoted = 'Hello'
double_quoted = "World"
multi_line = '''This is 
NetG India's 
Python Class'''
```

## 🔹 What is the data type of python? And, What is memory location? 


**🧠 Code:**
```python
box = "Apple"
print(box)             # prints the string
print(type(box))       # shows the type is str
```
**📤 Output:**
```text
Apple
<class 'str'>
```

## 🔹 Invalid Assignments (Errors)


**🧠 Code:**
```python
# These are incorrect and will throw errors
"apple" = box              # ❌ Cannot assign to literal
print(undefined_box)       # ❌ Variable not defined
```
**📤 Output:**
```text
SyntaxError: can't assign to literal
NameError: name 'undefined_box' is not defined
```

## 🔹 Valid Assignment

**🧠 Code:**
```python
box = "apple"
print(box)  # ✅ Correct assignment
```
**📤 Output:**
```text
apple
```

## 🔹 Memory Address Demo

**🧠 Code:**
```python
salt_box = "Salt"
sugar_box = "Sugar"

# Display memory addresses (ids)
print(id(salt_box))
print(id(sugar_box))
```
**📤 Output:**
```text
Value of salt_box: Salt
Memory address of salt_box: 140448083084848

Value of sugar_box: Sugar
Memory address of sugar_box: 140448083084976
```
===========
## 🔹 Hello World & Type Check

**🧠 Code:**
```python
print("Hello world")
text = "hello #$%^^sfuasdufasfadsgfguo*(*6789"
type(text)
```
**📤 Output:**
```text
Hello world
<class 'str'>
```

## 🔹 Case Sensitivity and String Comparison

**🧠 Code:**
```python
device = "Router"
print(device.upper())
print(device.lower())
intf = "Gigabit0/1"
if intf.lower() == "gigabit0/1":
    print("Interface matched")
```
**📤 Output:**
```text
ROUTER
router
Interface matched
```

## 🔹 Listing Available String Methods

**🧠 Code:**
```python
dir(str)
**📤 Output:**
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'removeprefix', 'removesuffix', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']

```

## 🔹 IP Classification using startswith()

**🧠 Code:**
```python
ip = "10.2.3.4"
if ip.startswith("10."):
    print("Class A private ip")
```
**📤 Output:**
```text
Class A private ip
```

## 🔹 String Formatting in Commands

**🧠 Code:**
```python
cmd = "ping {} source {}"
print(cmd.format("10.2.2.3", "gig0/1"))
```
**📤 Output:**
```text
ping 10.2.2.3 source gig0/1
```

## 🔹 Type Checking & Errors

**🧠 Code:**
```python
type(10)          # <class 'int'>
"10.1.1.1"        # valid string literal
```
**📤 Output:**
```text
<class 'int'>
'10.1.1.1'
```

## 🔹 Check if a String Ends with a Specific Character

**🧠 Code:**
```python
device = "cs-sw-a"
if device.endswith("a"):
    print("Found devices")
else:
    print("Device not found")
```
**📤 Output:**
```text
Found devices
```

## 🔹 Using `.startswith()` and `.lstrip()` for IP validation

**🧠 Code:**
```python
ip = "   10.2.3.4"
print(ip.startswith("10"))         # False due to leading spaces
print(ip.lstrip().startswith("10"))  # True after stripping
```
**📤 Output:**
```text
False
True
```

## 🔹 Misuse of `strip()` with Characters

**🧠 Code:**
```python
ip = "   10.2.3.4"
print(ip.strip("10"))  # Removes any '1', '0', or space from both ends
```
**📤 Output:**
```text
.2.3.4
```

## 🔹 Boolean Logic (AND / OR)

**🧠 Code:**
```python
print(True and True and True)        # True
print(True and False and True)       # False
print(False or False or True)        # True
print(False or False or False)       # False

if False or False or True:
    print("tested Okay")
```
**📤 Output:**
```text
True
False
True
False
tested Okay
```

## 🔹 Membership Check with `in` and `not in`

**🧠 Code:**
```python
device_name = "sw-cr-a"
print("cr" in device_name)  # True

print("r" in "Mridul")        # True
print("r" not in "Mridul")     # False
print("3" in "10.2.3.4")       # True
```
**📤 Output:**
```text
True
True
False
True
```

============
## 🔹 String Indexing and Slicing
============
**🧠 Code:**
```python
device = "Router"
print(device[0])  # R
print(device[-1]) # r
print(device[2:5]) # ute
print(device[::-1]) # retuoR
```
**📤 Output:**
```text
R
r
ute
retuoR
```
## 🔹 IP Address Slicing
**🧠 Code:**
```python
ip = "10.2.3.4"
print(ip[0:2])   # '10'
print(ip[3:6])   # '2.3'
print(ip[-5:-2]) # '2.3'
```
**📤 Output:**
```text
10
2.3
2.3
```

## 🔹 Positive and Negative Indexing

**🧠 Code:**
```python
box = "Sugar"
print(box[0])   # 'S'
print(box[-1])  # 'r'
```
**📤 Output:**
```text
S
r
```

## 🔹 How to Slice String? 

**🧠 Code:**
```python
python = "This_is_NetG_India"
print(python[0:4])   # 'This'
print(python[8:12])  # 'NetG'
print(python[-5:])   # 'India'
```
**📤 Output:**
```text
This
NetG
India
```

## 🔹 How can slice using step (jump) & How to get string in reverse? 

**🧠 Code:**
```python
print(python[::2])    # every second char
print(python[::-1])    # reversed string
```
**📤 Output:**
```text
Ti_sNt_nda
aidnI_GteN_si_sihT
```

## 🔹 In Python how to take help to understand the method of data structure? 

**🧠 Code:**
```python
print(dir(str))       # Lists all string methods
help(str.upper)         # Help on upper method
```

## 🌐 Examples of Format method!

## 🔹 Ex:1) Ping Command

**🧠 Code:**
```python
ping_cmd = "ping {} source {}"
print(ping_cmd.format("2.2.2.2", "1.1.1.1"))
```
**📤 Output:**
```text
ping 2.2.2.2 source 1.1.1.1
```

## 🔹 Ex:2) VLAN Command

**🧠 Code:**
```python
vlan_cmd = "vlan {}"
print(vlan_cmd.format(10))
```
**📤 Output:**
```text
vlan 10
```

## 🔹 Ex:3) SNMP Config

**🧠 Code:**
```python
snmp_cmd = "snmp-server host {} {} {}"
print(snmp_cmd.format("1.1.1.1", "public", "link-down"))
```
**📤 Output:**
```text
snmp-server host 1.1.1.1 public link-down
```

## 🔹 Ex:4) DNS Config

**🧠 Code:**
```python
dns_cmd = "ip name-server {}"
print(dns_cmd.format("8.8.8.8"))
```
**📤 Output:**
```text
ip name-server 8.8.8.8
```

## 🔹 Ex:5) User Account

**🧠 Code:**
```python
cmd1 = "username {} privilege {} password {}"
print(cmd1.format("admin", "15", "netg"))
```
**📤 Output:**
```text
username admin privilege 15 password netg
```

## 🔹 Ex:6) NTP Server

**🧠 Code:**
```python
ntp_cmd = "ntp server {}"
print(ntp_cmd.format("server 0.in.pool.ntp.org"))
```
**📤 Output:**
```text
ntp server server 0.in.pool.ntp.org
```
