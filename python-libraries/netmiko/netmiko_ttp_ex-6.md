# Netmiko TTP Parsing – Exercise 6

## What this automation does

This example demonstrates how to:
- Execute a show command using Netmiko
- Capture raw CLI output
- Parse that output using **TTP (Template Text Parser)**
- Keep data, templates, and logic cleanly separated
- Produce structured, machine-readable output

This is a **read-only, safe, and industry-standard automation pattern**.

---

## 1. Environment Setup

```bash
python -m venv venv
source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate       # Windows

pip install netmiko pyyaml ttp getpass4
```

---

## 2. Project Structure (Single Directory)

```text
automation/
├── devices.yaml
├── parser.ttp
├── main.py
└── output.txt
```

---

## 3. YAML File – devices.yaml

Purpose:
- Stores device connection details
- Stores the show command
- Keeps data outside Python code

```yaml
device:
  device_type: cisco_ios
  host: 192.0.2.1
  username: admin
  show_command: show ip interface brief
```

---

## 4. TTP Template – parser.ttp

```ttp
<group name="interfaces">
{{ interface | WORD }} {{ ip | IP }} {{ ok | WORD }} {{ method | WORD }} {{ status | WORD }} {{ protocol | WORD }}
</group>
```

---

## 5. Python Code – main.py

```python
from netmiko import ConnectHandler
from getpass import getpass
from ttp import ttp
import yaml

# Securely prompt for password
password = getpass("Enter device password: ")

# Load YAML data
with open("devices.yaml") as f:
    data = yaml.safe_load(f)

device_data = data["device"]

device = {
    "device_type": device_data["device_type"],
    "host": device_data["host"],
    "username": device_data["username"],
    "password": password,
}

# Execute show command
with ConnectHandler(**device) as conn:
    raw_output = conn.send_command(device_data["show_command"])

# Parse output using TTP
parser = ttp(data=raw_output, template=open("parser.ttp").read())
parser.parse()

parsed_data = parser.result()[0][0]

# Save structured output
with open("output.txt", "w") as f:
    f.write(str(parsed_data))

print(parsed_data)
```

---

## 6. Output File – output.txt

```text
[
  {
    'interface': 'GigabitEthernet0/0',
    'ip': '192.168.1.1',
    'ok': 'YES',
    'method': 'manual',
    'status': 'up',
    'protocol': 'up'
  }
]
```

---

## 7. TTP Deep Dive (Explanation & Reference)

### What is TTP?
**TTP (Template Text Parser)** is a Python library used to convert unstructured CLI output
into structured data such as dictionaries and lists.

It sits between raw text and automation logic.

---

### Why TTP is Used
- Network devices return human-readable text
- Automation needs machine-readable data
- TTP bridges this gap with minimal effort
- Easier and faster to write than TextFSM for many use cases

---

### How the TTP Template Was Written

#### Step 1: Look at real CLI output
```text
GigabitEthernet0/0 192.168.1.1 YES manual up up
GigabitEthernet0/1 unassigned YES unset administratively down down
```

#### Step 2: Identify repeated records
- Each line represents one interface
- Repetition means a `<group>` is required

#### Step 3: Map fields left to right
```text
Interface | IP | OK | Method | Status | Protocol
```

#### Step 4: Apply filters
- `WORD` → single-word fields
- `IP` → IPv4 addresses

This produces the final template:

```ttp
<group name="interfaces">
{{ interface | WORD }} {{ ip | IP }} {{ ok | WORD }} {{ method | WORD }} {{ status | WORD }} {{ protocol | WORD }}
</group>
```

---

### Important Things to Check When Writing TTP Templates

- Always use **real device output**
- Identify repeating data correctly
- Avoid relying on column spacing
- Use appropriate filters
- Test against multiple outputs

---

### When to Use TTP
- Semi-structured CLI output
- Vendor-neutral parsing
- Quick operational automation

### When NOT to Use TTP
- Highly variable output
- Deep hierarchical parsing
- Vendor-supported parsers already exist (e.g., Genie)

---

## 8. References

- TTP Documentation: https://ttp.readthedocs.io/
- Netmiko Examples: https://github.com/ktbyers/netmiko/blob/develop/EXAMPLES.md

---

