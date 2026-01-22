# JSON ↔ YAML ↔ XML Conversion for Network Automation

---

## 📂 Folder Structure

```bash
conversion_project/
│
├── json_to_yaml.py      # Convert JSON → YAML
├── yaml_to_json.py      # Convert YAML → JSON
├── json_to_xml.py       # Convert JSON → XML
├── yaml_to_xml.py       # Convert YAML → XML
├── xml_to_json.py       # Convert XML → JSON
├── xml_to_yaml.py       # Convert XML → YAML
├── test_data.json       # Sample input JSON file
├── test_data.yaml       # YAML output file
└── test_data.xml        # XML output file
```

---

# 📘 File Contents

Below are the **actual contents** of all Python scripts and data files used in this conversion workflow.

---

## 🧩 `test_data.json`

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

## 🧾 `test_data.yaml`

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

## 🧱 `json_to_yaml.py`

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

---

## 🔁 `yaml_to_json.py`

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

---

## ⚙️ `json_to_xml.py`

```python
import json
import xml.etree.ElementTree as ET

# Read JSON data
with open("test_data.json", "r") as json_file:
    data = json.load(json_file)

root = ET.Element("interfaces")

for intf in data:
    intf_elem = ET.SubElement(root, "interface")
    for key, value in intf.items():
        child = ET.SubElement(intf_elem, key)
        child.text = str(value)

tree = ET.ElementTree(root)
tree.write("test_data.xml", encoding="utf-8", xml_declaration=True)

print("✅ Converted JSON → XML successfully!")
```

---

## ⚙️ `yaml_to_xml.py`

```python
import yaml
import xml.etree.ElementTree as ET

# Read YAML data
with open("test_data.yaml", "r") as yaml_file:
    data = yaml.safe_load(yaml_file)

root = ET.Element("interfaces")
for intf in data:
    intf_elem = ET.SubElement(root, "interface")
    for key, value in intf.items():
        child = ET.SubElement(intf_elem, key)
        child.text = str(value)

tree = ET.ElementTree(root)
tree.write("from_yaml.xml", encoding="utf-8", xml_declaration=True)

print("✅ Converted YAML → XML successfully!")
```

---

## 🔁 `xml_to_json.py`

```python
import xml.etree.ElementTree as ET
import json

tree = ET.parse("test_data.xml")
root = tree.getroot()

data = []
for intf in root.findall("interface"):
    entry = {child.tag: child.text for child in intf}
    data.append(entry)

with open("from_xml.json", "w") as json_file:
    json.dump(data, json_file, indent=4)

print("✅ Converted XML → JSON successfully!")
```

---

## 🔁 `xml_to_yaml.py`

```python
import xml.etree.ElementTree as ET
import yaml

tree = ET.parse("test_data.xml")
root = tree.getroot()

data = []
for intf in root.findall("interface"):
    entry = {child.tag: child.text for child in intf}
    data.append(entry)

with open("from_xml.yaml", "w") as yaml_file:
    yaml.dump(data, yaml_file, indent=4, sort_keys=False)

print("✅ Converted XML → YAML successfully!")
```

---

# 🧠 Explanation and Use Cases

These scripts enable seamless interchange between **JSON**, **YAML**, and **XML**, ensuring compatibility across automation workflows and network configuration management systems.

---

## 🧩 Why Multiple Formats?

| Format | Primary Use Case | Notes |
|---------|------------------|-------|
| **JSON** | APIs, REST communication | Machine-readable, compact |
| **YAML** | Ansible, Nornir configs | Human-readable, supports comments |
| **XML** | NETCONF/YANG, vendor device configs | Hierarchical, structured |

---

## ⚡ Workflow Summary

| Conversion | Input | Output |
|-------------|--------|---------|
| JSON → YAML | `test_data.json` | `test_data.yaml` |
| YAML → JSON | `test_data.yaml` | `converted_back.json` |
| JSON → XML | `test_data.json` | `test_data.xml` |
| YAML → XML | `test_data.yaml` | `from_yaml.xml` |
| XML → JSON | `test_data.xml` | `from_xml.json` |
| XML → YAML | `test_data.xml` | `from_xml.yaml` |

---

## 🧩 End-to-End Network Automation Example

```python
import json, yaml, xml.etree.ElementTree as ET

# Sample data from a controller API
interfaces = [
    {"intf_name": "GigabitEthernet0/1", "state": "up", "mtu": 1500},
    {"intf_name": "GigabitEthernet0/2", "state": "down", "mtu": 1500}
]

# Convert to YAML for playbook usage
yaml_output = yaml.dump(interfaces, indent=4, sort_keys=False)
open("interfaces.yaml", "w").write(yaml_output)

# Convert to XML for NETCONF-compatible configs
root = ET.Element("interfaces")
for intf in interfaces:
    intf_elem = ET.SubElement(root, "interface")
    for key, value in intf.items():
        ET.SubElement(intf_elem, key).text = str(value)

ET.ElementTree(root).write("interfaces.xml", encoding="utf-8", xml_declaration=True)
print("✅ Generated YAML and XML from JSON controller data!")
```

---

## 🧾 Summary Table

| Source | Target | Library Used | Method |
|---------|---------|---------------|--------|
| JSON → YAML | PyYAML | `yaml.dump()` |
| YAML → JSON | json | `json.dump()` |
| JSON → XML | xml.etree.ElementTree | Build tree manually |
| YAML → XML | xml.etree.ElementTree | Convert parsed YAML |
| XML → JSON | xml.etree + json | Parse XML to dict |
| XML → YAML | xml.etree + yaml | Parse XML to dict, dump to YAML |

---

## 🧠 Key Takeaways

- Maintain **data consistency** across automation layers.  
- Convert **API data (JSON)** → **templates (YAML)** → **device configs (XML)** seamlessly.  
- Ideal for use with **Tufin SecureTrack**, **Ansible**, **NETCONF**, and **custom scripts**.

---
