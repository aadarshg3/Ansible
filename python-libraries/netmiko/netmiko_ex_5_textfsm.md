# Netmiko + TextFSM
## Structured Parsing of *Show* Command Output

---

## 📌 What is TextFSM?

**TextFSM** is a Python-based template engine used to **convert unstructured CLI output** (plain text) into **structured data** (lists/dictionaries).

In simple terms:
- Raw CLI output ➜ **hard to automate**
- TextFSM ➜ **machine-readable structured data**

---

## ❓ Why Do We Use TextFSM?

Network devices return output designed for **humans**, not programs.

Problems with raw output:
- Inconsistent spacing
- Hard-coded string parsing (`split()`, regex hell)
- Breaks easily across OS versions

**TextFSM solves this by:**
- Using templates that describe the output structure
- Returning predictable data structures
- Making automation **reliable and scalable**

---
In Netmiko, TextFSM is typically used with:
- `send_command(..., use_textfsm=True)`

---

## 🎯 Use Case (This Example)

**Scenario:**
- 3 Cisco IOS devices (SW1, SW2, SW3)
- Run `show ip interface brief`
- Parse output into **structured data**
- Print or process results programmatically

No configuration rendering is required here — **only parsing**.

👉 **Jinja2 is NOT required** because:
- No configuration is being generated
- Only operational data is being collected and parsed

---

## 1️⃣ Python Environment Setup

```bash
python -m venv venv

source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate       # Windows

pip install netmiko pyyaml ntc-templates
```

📌 **Why `ntc-templates`?**  
Netmiko uses these templates internally for TextFSM parsing.

---

## 2️⃣ Project Layout

```text
automation/
├── inventory.yaml
├── main.py
└── outputs/
```

---

## 3️⃣ Device Inventory
### 📄 `inventory.yaml`

```yaml
devices:
  - device_type: cisco_ios
    host: 192.168.199.132
    username: admin
    password: admin
    hostname: SW1

  - device_type: cisco_ios
    host: 192.168.199.133
    username: admin
    password: admin
    hostname: SW2

  - device_type: cisco_ios
    host: 192.168.199.134
    username: admin
    password: admin
    hostname: SW3
```

---

## 4️⃣ Automation Script Using TextFSM
### 📄 `main.py`

This script:
- Connects to each device
- Executes a *show* command
- Parses output using **TextFSM**
- Returns structured data (list of dictionaries)

```python
import yaml
from netmiko import ConnectHandler

SHOW_COMMAND = "show ip interface brief"

# Load inventory
with open("inventory.yaml") as f:
    inventory = yaml.safe_load(f)

for device in inventory["devices"]:
    print(f"Connecting to {device['hostname']}")

    connection = ConnectHandler(**device)

    # Execute command with TextFSM parsing
    output = connection.send_command(
        SHOW_COMMAND,
        use_textfsm=True
    )

    connection.disconnect()

    print(f"Parsed output from {device['hostname']}")

    for entry in output:
        print(entry)

    print("-" * 60)
```

---

## 5️⃣ Example Parsed Output

Instead of raw CLI text, TextFSM returns:

```python
{
  'interface': 'GigabitEthernet1',
  'ip_address': '10.1.1.1',
  'status': 'up',
  'protocol': 'up'
}
```

---

## 6️⃣  When NOT to Use TextFSM

Avoid TextFSM when:
- Output format is unstable or custom
- Commands are interactive
- Native APIs (REST/gRPC) are available
---

## 🔗 References

- Netmiko TextFSM Example:  
  https://github.com/ktbyers/netmiko/blob/develop/EXAMPLES.md#using-textfsm

