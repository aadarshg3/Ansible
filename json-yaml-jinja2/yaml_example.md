
# YAML for Network Automation –

---

# 📂 Folder Structure

```
yaml_project/
│
├── yaml_write_example.py
├── yaml_read_example.py
├── yaml_safe_load_example.py
└── test_intf.yaml   ← This file is CREATED by yaml_write_example.py
```

---

# 🧠 Writing YAML Data (`yaml_write_example.py`)

```python
import yaml

# Network Interface Configuration Data
intf_conf_data = [
    {
        "intf_name": "GigabitEthernet0/1",
        "desc": "Connected to Server1",
        "speed": 1000,
        "duplex": "full"
    },
    {
        "intf_name": "GigabitEthernet0/2",
        "desc": "Connected to Server2",
        "speed": 1000,
        "duplex": "full"
    },
    {
        "intf_name": "GigabitEthernet0/3",
        "desc": "Connected to Server3",
        "speed": 1000,
        "duplex": "full"
    }
]

# Write YAML file
with open("test_intf.yaml", "w") as yaml_file:
    yaml.dump(intf_conf_data, yaml_file, indent=4, sort_keys=False)
```

---

# ✅ Example Output — *This is exactly what `yaml_write_example.py` writes into test_intf.yaml*

```yaml
- intf_name: GigabitEthernet0/1
  desc: Connected to Server1
  speed: 1000
  duplex: full
- intf_name: GigabitEthernet0/2
  desc: Connected to Server2
  speed: 1000
  duplex: full
- intf_name: GigabitEthernet0/3
  desc: Connected to Server3
  speed: 1000
  duplex: full
```


---

# 📥 Reading YAML Data (`yaml_read_example.py`)

```python
import yaml

with open("test_intf.yaml", "r") as yaml_file:
    yaml_data = yaml.load(yaml_file, Loader=yaml.FullLoader)

print(yaml_data)
```

---

# 🧯 Secure Reading (`yaml_safe_load_example.py`)

```python
import yaml

with open("test_intf.yaml", "r") as yaml_file:
    yaml_data = yaml.safe_load(yaml_file)

print(yaml_data)
```

---

# ==================== Short Note:

## **Why Does yaml.load() Require a Loader Parameter?**

`yaml.load()` **requires** a Loader parameter because:

1. PyYAML **5.1+** made Loaders mandatory for security.  
2. Without Loader, `yaml.load()` raises a **TypeError**.  
3. Loader determines **how YAML is interpreted**, so choosing one is important.

---

## ✔ Difference Between `yaml.load()` and `yaml.safe_load()`

### **`yaml.safe_load()`**
- No Loader parameter needed.
- Internally uses:
  ```
  yaml.load(stream, Loader=yaml.SafeLoader)
  ```
- Safe for automation usage.

### **`yaml.load()`**
- Loader must be specified:
  ```
  yaml.load(stream, Loader=yaml.FullLoader)
  ```
- Not recommended unless advanced parsing behavior is required.

---

# ⚙️ Enhanced Dump Example (Flow Style)

```python
import yaml

device_inventory = {
    "routers": [
        {"name": "R1", "ip": "10.0.0.1", "vendor": "Cisco"},
        {"name": "R2", "ip": "10.0.0.2", "vendor": "Juniper"}
    ],
    "switches": [
        {"name": "SW1", "ip": "10.0.1.1", "vendor": "Arista"}
    ]
}

with open("device_inventory.yaml", "w") as yaml_file:
    yaml.dump(device_inventory, yaml_file, default_flow_style=True)
```

---

# 🔍 Reading Multiple YAML Documents (`yaml.load_all()`)

```python
import yaml

with open("multi_device.yaml", "r") as yaml_file:
    docs = yaml.load_all(yaml_file, Loader=yaml.SafeLoader)
    for doc in docs:
        print(doc)
```

### Example YAML (`multi_device.yaml`)
```yaml
---
hostname: R1
ip: 192.168.1.1
vendor: Cisco
---
hostname: R2
ip: 192.168.1.2
vendor: Juniper
```

Output:
```python
{'hostname': 'R1', 'ip': '192.168.1.1', 'vendor': 'Cisco'}
{'hostname': 'R2', 'ip': '192.168.1.2', 'vendor': 'Juniper'}
```

---


