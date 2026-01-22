# JSON ↔ YAML Conversion
---

## 📂 Folder Structure

```bash
conversion_project/
│
├── json_to_yaml.py     # Convert JSON → YAML
├── yaml_to_json.py     # Convert YAML → JSON
├── test_data.json      # Input JSON file
├── test_data.yaml      # Output YAML file (after conversion)
└── converted_back.json # JSON file recreated from YAML
```

---

## 🧱 Common Data (Network Interface Configuration)

```python
intf_conf_data = [
    {"intf_name": "GigabitEthernet0/1", "desc": "Connected to Server1", "speed": 1000, "duplex": "full"},
    {"intf_name": "GigabitEthernet0/2", "desc": "Connected to Server2", "speed": 1000, "duplex": "full"},
    {"intf_name": "GigabitEthernet0/3", "desc": "Connected to Server3", "speed": 1000, "duplex": "full"}
]
```

---

## 🔄 JSON → YAML Conversion (`json_to_yaml.py`)

```python
import json
import yaml

# Step 1: Read from JSON file
with open("test_data.json", "r") as json_file:
    json_data = json.load(json_file)

# Step 2: Convert JSON → YAML
with open("test_data.yaml", "w") as yaml_file:
    yaml.dump(json_data, yaml_file, indent=4, sort_keys=False)

print("✅ Converted JSON → YAML successfully!")
```

### Example Output (`test_data.yaml`)
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

## 🔁 YAML → JSON Conversion (`yaml_to_json.py`)

```python
import yaml
import json

# Step 1: Read from YAML file
with open("test_data.yaml", "r") as yaml_file:
    yaml_data = yaml.safe_load(yaml_file)

# Step 2: Convert YAML → JSON
with open("converted_back.json", "w") as json_file:
    json.dump(yaml_data, json_file, indent=4)

print("✅ Converted YAML → JSON successfully!")
```

### Example Output (`converted_back.json`)
```json
[
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
```

---

## 🧠 Why Convert Between JSON and YAML?

| Format | Pros | Common Use |
|---------|------|-------------|
| **JSON** | Compact, supported by APIs and REST calls | Northbound APIs, automation tools |
| **YAML** | Human-readable, supports comments | Configuration templates, Ansible, Netmiko |
| **Both** | Interchangeable | Network configuration management |

---

## ⚡ Quick Tips

- Always use **`yaml.safe_load()`** when reading YAML from external sources.  
- Add **`indent=4`** for readable JSON/YAML output.  
- Conversion preserves data structure — ideal for automation pipelines.

---

## 🧩 Example Use Case in Network Automation

Imagine a script that receives JSON data from a REST API (northbound controller) and converts it into YAML templates for device configurations.

```python
import requests, yaml

# Fetch JSON data from controller
response = requests.get("https://controller/api/interfaces")
json_data = response.json()

# Convert to YAML for easy templating
yaml_output = yaml.dump(json_data, indent=4, sort_keys=False)

with open("interface_template.yaml", "w") as f:
    f.write(yaml_output)
```

This enables seamless flow between **API-driven JSON data** and **YAML-based config management tools** like Ansible or Nornir.

---
