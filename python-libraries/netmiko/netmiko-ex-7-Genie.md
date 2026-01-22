# Netmiko Example-5 — Using Genie

## 📌 What is Genie (Brief)

**Genie** is a Cisco pyATS-based parsing and modeling engine used to convert raw network CLI output into **structured, normalized Python dictionaries**.

Unlike regex-based parsing, Genie uses **pre-built, vendor-aware parsers** that deeply understand network command outputs.

---

## 🤔 Why Genie is Used

Network automation requires **accurate operational data**.

Challenges without Genie:
- Complex and inconsistent CLI output
- Manual parsing logic
- High maintenance overhead

Genie solves this by:
- Providing validated parsers
- Normalizing data across platforms
- Returning rich, hierarchical structures

---

## 📍 Where Genie is Used

Genie is commonly used in:
- State validation workflows
- Network testing (pyATS)
- Compliance automation
- Pre / post-change checks
- Advanced troubleshooting automation

---

## 🎯 Primary Use Case

**Collect detailed operational state from devices and convert it into structured data for validation and decision-making.**

Typical examples:
- Interface operational state
- Routing protocol health
- Neighbor relationships
- Platform and hardware status

---

## 🧱 Example Scenario (3 Devices)

Goal:
- Run `show ip interface brief`
- Parse output using Genie
- Collect structured data from **3 devices**

---

## 📁 Project Layout

```text
automation/
├── inventory.yaml
├── commands.yaml
├── main.py
```

> ⚠️ **Jinja2 is NOT required**  
> Reason: Genie parses **device output**, not configuration.  
> Jinja2 is only needed when **rendering configs**, not when collecting state.

---

## 📄 inventory.yaml  
Defines **WHERE** data is collected from.

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

## 📄 commands.yaml  
Defines **WHAT** command to run.

```yaml
command: show ip interface brief
```

---

## 🧠 How Genie Works Internally

1. Netmiko sends a command
2. Raw CLI output is returned
3. Genie parser is invoked
4. Output is converted into a structured dictionary

Genie parsers are **vendor- and command-specific**, ensuring high accuracy.

---

## 🐍 Automation Script  
### 📄 main.py

```python
import yaml
from netmiko import ConnectHandler

with open("inventory.yaml") as f:
    inventory = yaml.safe_load(f)

with open("commands.yaml") as f:
    command_data = yaml.safe_load(f)

command = command_data["command"]

for device in inventory["devices"]:
    print(f"Connecting to {device['hostname']}")

    connection = ConnectHandler(**device)

    output = connection.send_command(
        command,
        use_genie=True
    )

    connection.disconnect()

    print(f"Parsed output from {device['hostname']}:")
    print(output)
```

---

## 📤 Sample Parsed Output (Structured)

```python
{
  'interface': {
    'GigabitEthernet1': {
      'ip_address': '192.168.1.1',
      'status': 'up',
      'protocol': 'up'
    },
    'GigabitEthernet2': {
      'ip_address': 'unassigned',
      'status': 'administratively down',
      'protocol': 'down'
    }
  }
}
```

---

## 🔗 Official Reference

Netmiko Genie Example (Source):  
https://github.com/ktbyers/netmiko/blob/develop/EXAMPLES.md#using-genie
