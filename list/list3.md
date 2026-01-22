# 📝 Python List – Configurations
==

## 🔹 Device & Interface Configuration

**🧠 Code:**
```python
config_list_access = ["switchport mode access", "switchport access vlan 10"]
config_list_trunk = ["switchport mode trunk", "switchport trunk allowed vlan add 10"]

intf_list = ["gi0/1", "gi0/2", "gi0/3", "gi0/4"]
device_list = ["switch1", "switch2", "switch3", "switch4"]

for dev in device_list:
    print("logging to", dev)
    for intf in intf_list:
        print("interface", intf)
        if intf == "gi0/1" or intf == "gi0/2":
            for conf in config_list_trunk:
                print(conf)
        else:
            for conf in config_list_access:
                print(conf)
```
**📤 Output (sample for one device):**
```text
logging to switch1
interface gi0/1
switchport mode trunk
switchport trunk allowed vlan add 10
interface gi0/2
switchport mode trunk
switchport trunk allowed vlan add 10
interface gi0/3
switchport mode access
switchport access vlan 10
interface gi0/4
switchport mode access
switchport access vlan 10
```

---

## 🔹 Interface-wise Config

**🧠 Code:**
```python
config_list_access = ["switchport mode access", "switchport access vlan 10"]
config_list_trunk = ["switchport mode trunk", "switchport trunk allowed vlan add 10"]
intf_list = ["gi0/1", "gi0/2", "gi0/3", "gi0/4"]

for intf in intf_list:
    print("interface", intf)
    if intf == "gi0/1" or intf == "gi0/2":
        for conf in config_list_trunk:
            print(conf)
    else:
        for conf in config_list_access:
            print(conf)
```
**📤 Output:**
```text
interface gi0/1
switchport mode trunk
switchport trunk allowed vlan add 10
interface gi0/2
switchport mode trunk
switchport trunk allowed vlan add 10
interface gi0/3
switchport mode access
switchport access vlan 10
interface gi0/4
switchport mode access
switchport access vlan 10
```

---

## 🔹 Using `zip()` with Lists

**🧠 Code:**
```python
list1 = [[70,80,60], ["15-7-2024:10:30:54", "16-7-2024:10:30:54", "17-7-2024:10:30:54"]]
new_device_data = list(zip(list1[0], list1[1]))
print(new_device_data)

list2 = ["a", "b", "c"]
list3 = [1,2,3]
list4 = tuple(zip(list2, list3))
print(list4)
```
**📤 Output:**
```text
[(70, '15-7-2024:10:30:54'), (80, '16-7-2024:10:30:54'), (60, '17-7-2024:10:30:54')]
(('a', 1), ('b', 2), ('c', 3))
```

---

## 🔹 CPU Utilization – Find Max & Min

**🧠 Code:**
```python
device = ["sw1"]
cpu_utilization = [70,80,60,20,10,90,30]
cpu_utilization.sort()

for item in device:
    print("max cpu utilization value", cpu_utilization[-1])
    print("min cpu utilization value", cpu_utilization[0])
```
**📤 Output:**
```text
max cpu utilization value 90
min cpu utilization value 10
```

---

## 🔹 Interface Config with Access & Trunk

**🧠 Code:**
```python
intf_list = ["gig0/1", "gig0/2", "gig0/3"]

access_intf_config = [
    "description user port",
    "switchport mode access", 
    "switchport access vlan 10", 
    "speed 1000", "duplex full"
]

trunk_intf_config = [
    "description user port",
    "switchport mode trunk", 
    "switchport allowed vlan add 10", 
    "speed 1000", "duplex full"
]

for intf in intf_list:
    print("!")
    print("interface", intf)
    if intf == "gig0/2":
        for config in trunk_intf_config:
            print(" ", config)
    else:
        for config in access_intf_config:
            print(" ", config)
```
**📤 Output:**
```text
!
interface gig0/1
  description user port
  switchport mode access
  switchport access vlan 10
  speed 1000
  duplex full
!
interface gig0/2
  description user port
  switchport mode trunk
  switchport allowed vlan add 10
  speed 1000
  duplex full
!
interface gig0/3
  description user port
  switchport mode access
  switchport access vlan 10
  speed 1000
  duplex full
```

---

## 🔹 VLAN Configuration by Site & Device

**🧠 Code:**
```python
vlan_list = [10, 20, 30, 40]
device_list = ["sw1", "sw2", "sw3"]
site_list = ["site_A", "site_B", "site_C"]

for site in site_list:
    print(f"\nConfiguring Vlan on {site}============>")
    for device in device_list:
        print(f"\nConnecting to {device}...\n")
        for vlan in vlan_list:
            print(f"vlan {vlan}")
            print(f" name vlan_{vlan}")
```
**📤 Output:**
```text
Configuring Vlan on site_A============>

Connecting to sw1...

vlan 10
 name vlan_10
vlan 20
 name vlan_20
vlan 30
 name vlan_30
vlan 40
 name vlan_40
...
```
