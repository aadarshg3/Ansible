# Netmiko + Python
## Executing *Show* Commands with Timezone-Aware Timestamping & Log Retention

---

## 📌 Purpose

This document demonstrates a **production-ready Netmiko automation pattern** that focuses on **safe, auditable, and controlled output collection**.

Key capabilities:
- Execute *show* commands across multiple network devices
- Generate **timezone-aware timestamps**
- Enforce a strict **retention policy (max 5 active files)**
- Automatically **archive older outputs**

---

## 1️⃣ Why Timezones & Retention Matter

In real-world network operations:

- Devices operate across **multiple regions**
- Logs must reflect **accurate and consistent time**
- Output files must be **controlled, not endlessly accumulated**


---

## 2️⃣ Python Environment Setup

```bash
python -m venv venv

source venv/bin/activate        # Linux / macOS
# venv\\Scripts\\activate       # Windows

pip install netmiko pyyaml pytz
```

📌 **Why `pytz`?**  
Used to generate **timezone-aware timestamps** instead of naive local time.

---

## 3️⃣ Project Layout

```text
automation/
├── inventory.yaml
├── main.py
├── retention.py
└── outputs/
    ├── archive/
    └── show_output_YYYYMMDD_HHMMSS_TZ.txt
```

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

## 5️⃣ Retention Logic (Decoupled)
### 📄 `retention.py`

This module enforces the retention policy:
- **Keep only the newest 5 output files**
- Archive everything older

```python
from pathlib import Path

def rotate_outputs(
    output_dir: Path,
    archive_dir: Path,
    max_active_files: int,
    pattern: str = "show_output_*.txt"
):
    """
    Keeps the newest max_active_files and archives older ones.
    """
    archive_dir.mkdir(exist_ok=True)

    files = sorted(
        output_dir.glob(pattern),
        key=lambda f: f.stat().st_mtime
    )

    if len(files) <= max_active_files:
        return

    for old_file in files[:-max_active_files]:
        old_file.rename(archive_dir / old_file.name)
```

---

## 6️⃣ Main Automation Script
### 📄 `main.py`

This script integrates:
1. Timezone-aware timestamps  
2. Output file versioning  
3. Strict retention (max **5 active files**)  

```python
import yaml
from datetime import datetime
from pathlib import Path
import pytz
from netmiko import ConnectHandler

from retention import rotate_outputs

SHOW_COMMAND = "show ip interface brief"
TIMEZONE = "UTC"                 # Example: Asia/Kolkata
MAX_ACTIVE_FILES = 5

# --------------------------------------------------
# Timezone-aware timestamp
# --------------------------------------------------
tz = pytz.timezone(TIMEZONE)
timestamp = datetime.now(tz).strftime("%Y%m%d_%H%M%S_%Z")

# --------------------------------------------------
# Output directories
# --------------------------------------------------
output_dir = Path("outputs")
archive_dir = output_dir / "archive"
output_dir.mkdir(exist_ok=True)

# --------------------------------------------------
# Enforce retention BEFORE creating new output
# --------------------------------------------------
rotate_outputs(
    output_dir=output_dir,
    archive_dir=archive_dir,
    max_active_files=MAX_ACTIVE_FILES
)

# --------------------------------------------------
# Timestamped output file
# --------------------------------------------------
output_file = output_dir / f"show_output_{timestamp}.txt"

# --------------------------------------------------
# Load inventory
# --------------------------------------------------
with open("inventory.yaml") as f:
    inventory = yaml.safe_load(f)

# --------------------------------------------------
# Execute commands
# --------------------------------------------------
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

## 7️⃣ Example File Lifecycle

```text
outputs/
├── show_output_20250924_101530_UTC.txt
├── show_output_20250924_104212_UTC.txt
├── show_output_20250924_110845_UTC.txt
├── show_output_20250924_113012_UTC.txt
├── show_output_20250924_120331_UTC.txt
└── archive/
    ├── show_output_20250923_092211_UTC.txt
    └── show_output_20250923_101045_UTC.txt
```

✔ Exactly **5 active files**  
✔ Older outputs preserved  
✔ Zero manual cleanup  

---

## 8️⃣ Intended Output (Sample)

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

