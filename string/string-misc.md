# 🚀 Python String Methods: 
------------------------------------------------------------------------

## 🔠 `upper()`

**What it does:** Converts all characters to uppercase.\

``` python
cmd = "show ip route"
print(cmd.upper())
```

👉 **Output:** `SHOW IP ROUTE`

``` python
hostname = "r1-core"
print(hostname.upper())
```

👉 **Output:** `R1-CORE`

------------------------------------------------------------------------

## 🔡 `lower()`

**What it does:** Converts all characters to lowercase.\

``` python
hostname = "SWITCH-CORE"
print(hostname.lower())
```

👉 **Output:** `switch-core`

``` python
interface = "GigabitEthernet0/1"
print(interface.lower())
```

👉 **Output:** `gigabitethernet0/1`

------------------------------------------------------------------------

## ✂️ `strip()`

**What it does:** Removes leading/trailing spaces or characters.\
✅ **Where to use:** Clean device outputs before parsing.

``` python
output = "   up    "
print(output.strip())
```

👉 **Output:** `up`

``` python
ip = "###192.168.1.1###"
print(ip.strip("#"))
```

👉 **Output:** `192.168.1.1`

------------------------------------------------------------------------

## ⚙️ `replace()`

**What it does:** Replaces part of a string with another.\
✅ **Where to use:** Modify templates or logs dynamically.

``` python
config = "ip address 192.168.1.1 255.255.255.0"
print(config.replace("192.168.1.1", "10.0.0.1"))
```

👉 **Output:** `ip address 10.0.0.1 255.255.255.0`

``` python
log = "ERROR: Interface down"
print(log.replace("ERROR", "WARNING"))
```

👉 **Output:** `WARNING: Interface down`

------------------------------------------------------------------------

## 📑 `split()`

**What it does:** Splits string into a list using a separator.\   ====> String to List 


``` python
routes = "10.0.0.0,172.16.0.0,192.168.1.0"
print(routes.split(","))
```

👉 **Output:** `['10.0.0.0', '172.16.0.0', '192.168.1.0']`

``` python
intf = "GigabitEthernet0/1 description Uplink"
print(intf.split())
```

👉 **Output:** `['GigabitEthernet0/1', 'description', 'Uplink']`

------------------------------------------------------------------------

## 🔗 `join()`

**What it does:** Joins list elements into a string.\  ====> helps in creating List to string 

``` python
vlans = ["10", "20", "30"]
print(",".join(vlans))
```

👉 **Output:** `10,20,30`

``` python
cmds = ["interface Gi0/1", "switchport mode trunk", "no shut"]
print("\n".join(cmds))
```

👉 **Output:**

    interface Gi0/1
    switchport mode trunk
    no shut

------------------------------------------------------------------------

## 🔍 `startswith()`

**What it does:** Checks if string starts with given text.\

``` python
cmd = "show running-config"
print(cmd.startswith("show"))
```

👉 **Output:** `True`

``` python
hostname = "R1-Core"
print(hostname.startswith("SW"))
```

👉 **Output:** `False`

------------------------------------------------------------------------

## 🏁 `endswith()`

**What it does:** Checks if string ends with given text.\

``` python
filename = "config_backup.txt"
print(filename.endswith(".txt"))
```

👉 **Output:** `True`

``` python
image = "iosxe.bin"
print(image.endswith(".cfg"))
```

👉 **Output:** `False`

------------------------------------------------------------------------

## 🔎 `find()`

**What it does:** Finds first index of substring (-1 if not found).\
✅ **Where to use:** Locate keywords in configs/logs.

``` python
log = "Interface Gi0/1 is up"
print(log.find("Gi0/1"))
```

👉 **Output:** `11`

``` python
config = "router ospf 1"
print(config.find("bgp"))
```

👉 **Output:** `-1`

------------------------------------------------------------------------

## 🔢 `count()`

**What it does:** Counts substring occurrences.\
✅ **Where to use:** Count errors/warnings.

``` python
log = "ERROR: Link down. ERROR: CPU high."
print(log.count("ERROR"))
```

👉 **Output:** `2`

``` python
output = "VLAN 10, VLAN 20, VLAN 30"
print(output.count("VLAN"))
```

👉 **Output:** `3`

------------------------------------------------------------------------

## 🅰️ `capitalize()`

**What it does:** First character uppercase, rest lowercase.\

``` python
device = "router"
print(device.capitalize())
```

👉 **Output:** `Router`

``` python
protocol = "ospf"
print(protocol.capitalize())
```

👉 **Output:** `Ospf`

------------------------------------------------------------------------

## 📝 `title()`

**What it does:** Capitalizes first letter of every word.\
✅ **Where to use:** Format documentation labels.

``` python
text = "access point configuration"
print(text.title())
```

👉 **Output:** `Access Point Configuration`

``` python
role = "network engineer"
print(role.title())
```

👉 **Output:** `Network Engineer`

------------------------------------------------------------------------

## 🅾️ `zfill()`

**What it does:** Pads string with leading zeros.\
✅ **Where to use:** Device/Port numbering.

``` python
device = "5"
print(device.zfill(3))
```

👉 **Output:** `005`

``` python
port = "12"
print(port.zfill(4))
```

👉 **Output:** `0012`

------------------------------------------------------------------------

## 🎯 `center()`

**What it does:** Centers text with padding.\
✅ **Where to use:** Pretty output formatting.

``` python
title = "Router"
print(title.center(20, "-"))
```

👉 **Output:** `-------Router-------`

``` python
device = "SW1"
print(device.center(10, "*"))
```

👉 **Output:** `***SW1****`

------------------------------------------------------------------------

## 🔟 `isdigit()`

**What it does:** Returns True if string is digits.\
✅ **Where to use:** Validate VLAN IDs.

``` python
vlan = "100"
print(vlan.isdigit())
```

👉 **Output:** `True`

``` python
hostname = "R1"
print(hostname.isdigit())
```

👉 **Output:** `False`

------------------------------------------------------------------------

## 🔡 `isalpha()`

**What it does:** Returns True if only alphabets.\
✅ **Where to use:** Detect pure text values.

``` python
name = "Router"
print(name.isalpha())
```

👉 **Output:** `True`

``` python
hostname = "SW1"
print(hostname.isalpha())
```

👉 **Output:** `False`

------------------------------------------------------------------------

## 🔤 `isalnum()`

**What it does:** Returns True if alphanumeric.\
✅ **Where to use:** Check hostnames.

``` python
hostname = "R1Core"
print(hostname.isalnum())
```

👉 **Output:** `True`

``` python
symbol = "Gi0/1"
print(symbol.isalnum())
```

👉 **Output:** `False`

------------------------------------------------------------------------

## ␣ `isspace()`

**What it does:** Returns True if only whitespace.\
✅ **Where to use:** Detect blank outputs.

``` python
data = "   "
print(data.isspace())
```

👉 **Output:** `True`

``` python
log = "Up "
print(log.isspace())
```

👉 **Output:** `False`

------------------------------------------------------------------------

## 🛠️ `format()`

**What it does:** Inserts values into template.\

``` python
config = "interface {0}".format("GigabitEthernet0/1")
print(config)
```

👉 **Output:** `interface GigabitEthernet0/1`

``` python
ip_config = "ip address {} {}".format("192.168.1.1", "255.255.255.0")
print(ip_config)
```

👉 **Output:** `ip address 192.168.1.1 255.255.255.0`

------------------------------------------------------------------------

## 📡 `encode()`

**What it does:** Encodes string into bytes.\
✅ **Where to use:** Encode before sending over APIs/SSH.

``` python
msg = "Login Success"
print(msg.encode())
```

👉 **Output:** `b'Login Success'`

``` python
cmd = "ping 8.8.8.8"
print(cmd.encode("utf-8"))
```

👉 **Output:** `b'ping 8.8.8.8'`

------------------------------------------------------------------------

> 💡 **Tip:** Many of these methods can be chained. Example:\
> `" show run ".strip().upper()` 👉 `SHOW RUN`

------------------------------------------------------------------------
