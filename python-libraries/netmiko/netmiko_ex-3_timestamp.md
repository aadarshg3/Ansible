# Netmiko + Python  
## Executing *Show* Commands with Timestamped Output (Production Pattern)

---

### 📌 Purpose of This Document

This document extends the **Netmiko show-command automation** by adding **timestamp integration**, which is a **real-world production requirement**.

It demonstrates how to:
- Execute show commands on multiple devices
- Automatically **timestamp output files**
- Maintain historical records for audits and troubleshooting

---

## 1️⃣ Why Timestamps Matter in Automation

In real environments, timestamps are critical for:

- Change tracking
- Audit trails
- Post-incident analysis
- Compliance requirements

Without timestamps, outputs are easily overwritten and lose context.

---

## 2️⃣ Python Environment Setup

```bash
python -m venv venv

source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate       # Windows

pip install netmiko pyyaml
```

---

## 3️⃣ Project Layout

```text
automation/
├── inventory.yaml
├── main.py
└── outputs/
    └── show_output_YYYYMMDD_HHMMSS.txt
```

Each execution creates a **new timestamped file**.

---

## 4️⃣ Device Inventory  
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

## 5️⃣ Automation Script with Timestamp  
This script:
- Generates a timestamp at runtime
- Creates a uniquely named output file
- Executes show commands on all devices
- Stores results safely without overwriting old data


### 📄 `main.py`

```python
import yaml
from datetime import datetime
from pathlib import Path
from netmiko import ConnectHandler

SHOW_COMMAND = "show ip interface brief"

# Generate timestamp once per execution
timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")

# Prepare output directory
output_dir = Path("outputs")
output_dir.mkdir(exist_ok=True)

# Timestamped output file
output_file = output_dir / f"show_output_{timestamp}.txt"

# Load inventory
with open("inventory.yaml") as f:
    inventory = yaml.safe_load(f)

with open(output_file, "w") as outfile:
    for device in inventory["devices"]:
        print(f"Connecting to {device['hostname']}")

        connection = ConnectHandler(**device)
        output = connection.send_command(SHOW_COMMAND)
        connection.disconnect()

        outfile.write(f"===== {device['hostname']} =====\n")
        outfile.write(output)
        outfile.write("\n\n")

        print(f"Collected output from {device['hostname']}")

print(f"Output saved to {output_file}")
```

---

## 6️⃣ Example Generated Files

```text
outputs/
├── show_output_20250924_101530.txt
├── show_output_20250924_104212.txt
└── show_output_20250924_110845.txt
```

Each file represents a **point-in-time snapshot**.

---

## 7️⃣ Intended Output (Sample)

```text
===== SW1 =====
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet1       10.1.1.1         YES manual up                    up

===== SW2 =====
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet1       10.1.2.1         YES manual up                    up

===== SW3 =====
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet1       10.1.3.1         YES manual up                    up
```

---

## 8️⃣ Real-World Usage Scenarios

- Daily health checks (cron-based)
- Pre-change snapshots
- Post-change verification
- Incident forensics
- Compliance reporting

---

