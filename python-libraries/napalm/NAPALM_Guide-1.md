# NAPALM DOC — 2 Examples

1) Multi-device: read inventory from YAML and run `get_facts()`  
2) Single-device: connect to one device and run `get_interfaces()`

---

## Folder Structure:

napalm_lab/
├─ sw.py
├─ inv_multi.yaml
├─ get-info.py
└─ intended_output.md


### File: `sw.py`

```python
import yaml
from napalm import get_network_driver

# Loading inventory information
inv = open("inv_multi.yaml", "r")
inventory = yaml.safe_load(inv)

for inv in inventory:
    driver = get_network_driver(inv["device_type"].split("_")[1])

    device = driver(
        hostname=inv["host"],
        username=inv["username"],
        password=inv["password"])
    device.open()

    data = device.get_facts()
    # data = device.get_vlans()
    # data = device.get_config()
    print(data)

    device.close()
```

### File: `inv_multi.yaml`

```yaml
- device_type: "cisco_ios"
  host: "192.168.200.151"
  hostname: "sw1"
  username: netg
  password: netg

- device_type: "cisco_ios"
  host: "192.168.200.152"
  hostname: "sw2"
  username: netg
  password: netg

- device_type: "cisco_ios"
  host: "192.168.200.153"
  hostname: "sw3"
  username: netg
  password: netg
```

### File: `get-info.py`

```python
from napalm import get_network_driver

# Define device credentials
device_info = {
    'hostname': '192.168.200.151',  # Replace with the device's IP
    'username': 'netg',             # Replace with the device's username
    'password': 'netg',             # Replace with the device's password

    # Optional: leave empty for beginner labs.
    # Example use-cases: SSH port, enable secret, global_delay_factor, etc.
    'optional_args': {}
}

# Use the Cisco IOS driver
driver = get_network_driver('ios')

# Connect to the device
device = driver(
    hostname=device_info['hostname'],
    username=device_info['username'],
    password=device_info['password'],
    optional_args=device_info['optional_args']
)

# Open a connection
device.open()

# Get interface information
interfaces = device.get_interfaces()

print(interfaces)

# Close the connection
device.close()
```

### File: `intended_output.md`

```markdown
# Intended output examples (sample shapes)

> Exact values depend on the device, OS, and NAPALM driver.

---

## Example 1: `sw.py` → `get_facts()`

**Printed type:** `dict` (printed once per device)

Example shape:

``​`json
{
  "hostname": "sw1",
  "fqdn": "sw1.lab.local",
  "vendor": "Cisco",
  "model": "IOSv",
  "os_version": "15.9(3)M",
  "serial_number": "FTX00000000",
  "uptime": 123456,
  "interface_list": ["GigabitEthernet0/0", "GigabitEthernet0/1", "Loopback0"]
}
``​`

---

## Example 2: `get-info.py` → `get_interfaces()`

**Printed type:** `dict` (interfaces → details)

Example shape:

``​`json
{
  "GigabitEthernet0/0": {
    "is_up": true,
    "is_enabled": true,
    "description": "Uplink",
    "last_flapped": -1.0,
    "mac_address": "00:11:22:33:44:55",
    "speed": 1000,
    "mtu": 1500
  }
}
``​`
```

---


## Steps


### Install

```bash
pip install napalm PyYAML
```

### Run Example 1 (multi-device facts)

```bash
python sw.py
```

### Run Example 2 (single-device interfaces)

```bash
python get-info.py
```

---


## NAPALM methods used here (2 only)

### 1) `get_facts()`
- **What it does:** Retrieves basic device information (vendor/model/OS/uptime/hostname/interface list).
- **Inputs:** none
- **Typical output type:** `dict`

### 2) `get_interfaces()`
- **What it does:** Retrieves interface operational/admin state and key attributes (speed/MTU/description/MAC/last_flapped).
- **Inputs:** none
- **Typical output type:** `dict`
