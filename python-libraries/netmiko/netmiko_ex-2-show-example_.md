# Netmiko + Python  
## Executing *Show* Commands (Multi-Device)

---

### 📌 Purpose of This Document

This document demonstrates a **real-world Netmiko automation use case** focused on:

- Executing **show commands**
- Collecting operational data from **multiple devices**
- Producing consistent, readable output

This example is based on the official Netmiko documentation for *executing show commands*, adapted for a **3-device lab environment**.

Reference: Netmiko examples – Executing show command citeturn3file0

---

## 1️⃣ Python Environment Setup

> Same setup pattern as configuration automation to maintain consistency.

```bash
python -m venv venv

# Activate virtual environment
source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate       # Windows

# Install required library
pip install netmiko pyyaml
```

📌 **Note**  
Jinja2 is **not required** here because:
- No configuration is being generated
- No CLI structure needs templating
- Commands are executed directly and output is collected

---

## 2️⃣ Project Layout

```text
automation/
├── inventory.yaml          # Device connection details
├── main.py                 # Executes show commands
└── show_output.txt         # Collected output (audit)
```

---

## 3️⃣ Device Inventory  
### 📄 `inventory.yaml`

YAML is still useful here to **scale cleanly across devices**.

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

## 4️⃣ Why Jinja2 Is NOT Required

Jinja2 is typically used when:
- Generating configurations
- Rendering structured CLI output

In this use case:
- Commands are static (e.g., `show ip interface brief`)
- No transformation is required

➡ **Direct execution via Netmiko is sufficient and cleaner**.

---

## 5️⃣ Automation Script – Execute Show Commands  
### 📄 `main.py`

This script:
- Connects to each device
- Executes a show command
- Collects output
- Writes consolidated results to a file

```python
import yaml                              # Load inventory
from netmiko import ConnectHandler       # Netmiko SSH handler

SHOW_COMMAND = "show ip interface brief"  # Command to execute

# Load device inventory
with open("inventory.yaml") as f:
    inventory = yaml.safe_load(f)

with open("show_output.txt", "w") as outfile:
    for device in inventory["devices"]:
        print(f"Connecting to {device['hostname']}")

        connection = ConnectHandler(**device)
        output = connection.send_command(SHOW_COMMAND)
        connection.disconnect()

        # Write structured output
        outfile.write(f"===== {device['hostname']} =====\n")
        outfile.write(output)
        outfile.write("\n\n")

        print(f"Command executed on {device['hostname']}")
```

📌 **Key Netmiko Method Used**
- `send_command()` → Used for show/exec commands (read-only)

---

## 6️⃣ Intended Output  
### 📄 `show_output.txt`

```text
===== SW1 =====
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet1       10.1.1.1         YES manual up                    up
GigabitEthernet2       unassigned       YES unset  administratively down down

===== SW2 =====
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet1       10.1.2.1         YES manual up                    up
GigabitEthernet2       unassigned       YES unset  down                  down

===== SW3 =====
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet1       10.1.3.1         YES manual up                    up
GigabitEthernet2       unassigned       YES unset  down                  down
```

---

## 7️⃣ Real-Time Use Cases

- Health checks
- Compliance validation
- Pre-change verification
- Post-change validation
- Inventory collection

---

## 8️⃣ Best-Practice Suggestions

✔ Timestamp output files  
✔ Add exception handling  
✔ Use TextFSM for structured parsing  
✔ Store per-device outputs if needed  

---
