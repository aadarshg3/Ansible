# JSON ↔ YAML ↔ XML Conversion 
---

## 📂 Folder Structure

```bash
conversion_project/
│
├── json_to_yaml.py
├── yaml_to_json.py
├── json_to_xml.py
├── yaml_to_xml.py
├── xml_to_json.py
├── xml_to_yaml.py
├── test_data.json
├── test_data.yaml
└── test_data.xml
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

## 🔄 JSON → YAML

```python
import json, yaml

with open("test_data.json") as json_file:
    data = json.load(json_file)

with open("test_data.yaml", "w") as yaml_file:
    yaml.dump(data, yaml_file, indent=4, sort_keys=False)
```

---

## 🔁 YAML → JSON

```python
import yaml, json

with open("test_data.yaml") as yaml_file:
    data = yaml.safe_load(yaml_file)

with open("converted_back.json", "w") as json_file:
    json.dump(data, json_file, indent=4)
```

---

## ⚙️ JSON → XML (`json_to_xml.py`)

```python
import json
import xml.etree.ElementTree as ET

# Read JSON data
with open("test_data.json") as json_file:
    data = json.load(json_file)

root = ET.Element("interfaces")

for intf in data:
    intf_elem = ET.SubElement(root, "interface")
    for key, value in intf.items():
        child = ET.SubElement(intf_elem, key)
        child.text = str(value)

tree = ET.ElementTree(root)
tree.write("test_data.xml", encoding="utf-8", xml_declaration=True)
```

**Output (`test_data.xml`)**
```xml
<?xml version='1.0' encoding='utf-8'?>
<interfaces>
    <interface>
        <intf_name>GigabitEthernet0/1</intf_name>
        <desc>Connected to Server1</desc>
        <speed>1000</speed>
        <duplex>full</duplex>
    </interface>
    ...
</interfaces>
```

---

## ⚙️ YAML → XML (`yaml_to_xml.py`)

```python
import yaml
import xml.etree.ElementTree as ET

with open("test_data.yaml") as yaml_file:
    data = yaml.safe_load(yaml_file)

root = ET.Element("interfaces")
for intf in data:
    intf_elem = ET.SubElement(root, "interface")
    for key, value in intf.items():
        ET.SubElement(intf_elem, key).text = str(value)

ET.ElementTree(root).write("from_yaml.xml", encoding="utf-8", xml_declaration=True)
```

---

## 🔁 XML → JSON (`xml_to_json.py`)

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
```

**Output (`from_xml.json`)**
```json
[
    {"intf_name": "GigabitEthernet0/1", "desc": "Connected to Server1", "speed": "1000", "duplex": "full"}
]
```

---

## 🔁 XML → YAML (`xml_to_yaml.py`)

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
```

**Output (`from_xml.yaml`)**
```yaml
- intf_name: GigabitEthernet0/1
  desc: Connected to Server1
  speed: '1000'
  duplex: full
```

---

## 🧩 Network Automation Example

```python
import json, yaml, xml.etree.ElementTree as ET

# JSON API response from network controller
api_data = [
    {"intf_name": "GigabitEthernet0/1", "state": "up", "mtu": 1500},
    {"intf_name": "GigabitEthernet0/2", "state": "down", "mtu": 1500}
]

# Convert to YAML for human-readable output
yaml_output = yaml.dump(api_data, indent=4)
open("api_interfaces.yaml", "w").write(yaml_output)

# Convert to XML for NETCONF template
root = ET.Element("interfaces")
for intf in api_data:
    intf_elem = ET.SubElement(root, "interface")
    for k, v in intf.items():
        ET.SubElement(intf_elem, k).text = str(v)

ET.ElementTree(root).write("api_interfaces.xml", encoding="utf-8", xml_declaration=True)
```

---

## 🧾 Summary Table

| Source | Target | Library | Method |
|---------|---------|----------|---------|
| JSON → YAML | `yaml.dump()` | `PyYAML` | Converts dict → YAML |
| YAML → JSON | `json.dump()` | Built-in | Converts YAML → JSON |
| JSON → XML | `xml.etree.ElementTree` | Built-in | Creates XML tree |
| YAML → XML | `xml.etree.ElementTree` | Built-in | YAML → XML |
| XML → JSON | `xml.etree.ElementTree`, `json` | Built-in | XML → dict → JSON |
| XML → YAML | `xml.etree.ElementTree`, `yaml` | Built-in + PyYAML | XML → dict → YAML |

---
