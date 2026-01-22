# XML Handling for Network Automation

---

## 📂 Folder Structure

```bash
xml_project/
│
├── xml_write_example.py      # Script for writing XML data
├── xml_read_example.py       # Script for reading XML data
└── test_intf.xml             # XML file created after writing
```

---

## 🧠 Writing XML Data (`xml_write_example.py`)

```python
import xml.etree.ElementTree as ET

# Network Interface Configuration Data
intf_conf_data = [
    {"intf_name": "GigabitEthernet0/1", "desc": "Connected to Server1", "speed": "1000", "duplex": "full"},
    {"intf_name": "GigabitEthernet0/2", "desc": "Connected to Server2", "speed": "1000", "duplex": "full"},
    {"intf_name": "GigabitEthernet0/3", "desc": "Connected to Server3", "speed": "1000", "duplex": "full"}
]

# Root element
root = ET.Element("interfaces")

# Add interface data
for intf in intf_conf_data:
    intf_elem = ET.SubElement(root, "interface")
    for key, value in intf.items():
        child = ET.SubElement(intf_elem, key)
        child.text = str(value)

# Create XML tree and write to file
tree = ET.ElementTree(root)
tree.write("test_intf.xml", encoding="utf-8", xml_declaration=True)
```

### ✅ Key Points
- `ET.Element()` creates the XML root.  
- `ET.SubElement()` adds nested tags.  
- `tree.write()` writes data to file with UTF-8 encoding.  
- Produces a structured XML representation of network interfaces.

---

## 📄 Example Output (`test_intf.xml`)

```xml
<?xml version='1.0' encoding='utf-8'?>
<interfaces>
    <interface>
        <intf_name>GigabitEthernet0/1</intf_name>
        <desc>Connected to Server1</desc>
        <speed>1000</speed>
        <duplex>full</duplex>
    </interface>
    <interface>
        <intf_name>GigabitEthernet0/2</intf_name>
        <desc>Connected to Server2</desc>
        <speed>1000</speed>
        <duplex>full</duplex>
    </interface>
    <interface>
        <intf_name>GigabitEthernet0/3</intf_name>
        <desc>Connected to Server3</desc>
        <speed>1000</speed>
        <duplex>full</duplex>
    </interface>
</interfaces>
```

---

## 📥 Reading XML Data (`xml_read_example.py`)

```python
import xml.etree.ElementTree as ET

# Parse XML file
tree = ET.parse("test_intf.xml")
root = tree.getroot()

# Read and display interface data
for intf in root.findall("interface"):
    name = intf.find("intf_name").text
    desc = intf.find("desc").text
    speed = intf.find("speed").text
    duplex = intf.find("duplex").text
    print(f"{name}: {desc}, {speed} Mbps, {duplex} duplex")
```

### Explanation:
- `ET.parse()` reads the XML file.  
- `getroot()` accesses the root element (`<interfaces>`).  
- `find()` extracts data from specific tags.  
- Output will be human-readable summaries of interface details.

---

## ⚙️ Enhanced Version (Pretty Printing & Write + Read)

```python
import xml.dom.minidom as minidom
import xml.etree.ElementTree as ET

# Read existing XML
tree = ET.parse("test_intf.xml")
xml_str = ET.tostring(tree.getroot(), encoding="utf-8")

# Pretty-print and write to a new file
pretty_xml = minidom.parseString(xml_str).toprettyxml(indent="   ")

with open("test_intf_pretty.xml", "w") as f:
    f.write(pretty_xml)

print("Formatted XML written to test_intf_pretty.xml")
```

### Why use `minidom`?
- Makes XML **readable** with indentation.  
- Useful for configuration exports or version control.

---

## 🧩 Example: Device Inventory in XML

```python
import xml.etree.ElementTree as ET

devices = ET.Element("devices")

routers = ET.SubElement(devices, "routers")
ET.SubElement(routers, "device", name="R1", ip="10.0.0.1", vendor="Cisco")
ET.SubElement(routers, "device", name="R2", ip="10.0.0.2", vendor="Juniper")

switches = ET.SubElement(devices, "switches")
ET.SubElement(switches, "device", name="SW1", ip="10.0.1.1", vendor="Arista")

# Write to XML file
tree = ET.ElementTree(devices)
tree.write("device_inventory.xml", encoding="utf-8", xml_declaration=True)
```

**Output (device_inventory.xml):**
```xml
<?xml version='1.0' encoding='utf-8'?>
<devices>
    <routers>
        <device name="R1" ip="10.0.0.1" vendor="Cisco"/>
        <device name="R2" ip="10.0.0.2" vendor="Juniper"/>
    </routers>
    <switches>
        <device name="SW1" ip="10.0.1.1" vendor="Arista"/>
    </switches>
</devices>
```

---

## 🧾 Summary

| Operation | Function / Module | Description |
|------------|------------------|--------------|
| Write XML | `xml.etree.ElementTree` | Converts Python → XML |
| Pretty Print | `xml.dom.minidom` | Beautifies XML output |
| Read XML | `ET.parse()` | Parses and loads XML data |
| Find element | `find()` | Retrieves tag by name |
| Sub-element | `ET.SubElement()` | Adds nested structure |

---

## 🧠 Quick Recap
- XML is **hierarchical**, ideal for structured network configs.  
- Python’s built-in `xml` modules provide safe read/write.  
- Great for data interchange with APIs or NETCONF/YANG models.

---
