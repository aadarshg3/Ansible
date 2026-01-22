# Netmiko Configuration Changes (3 Examples: Reference → Production → More Production)

Reference (Netmiko examples):
https://github.com/ktbyers/netmiko/blob/develop/EXAMPLES.md

---

## 0. What this automation doc covers

This doc shows **three ways to make configuration changes** with Netmiko:

1. **Reference-style (Netmiko doc)**: push a small static config list
2. **Production (medium complexity)**: YAML-driven + Jinja2-rendered config with a simple verify step
3. **Production (more complex, different approach)**: multi-device change workflow with backups + per-device rendered config files + structured results

---

## 1. Environment Setup

```bash
python -m venv venv
source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate       # Windows

pip install netmiko jinja2 pyyaml
```

---

# Example 1 — Reference-style Config Change (from Netmiko docs)

## Task (what exactly changes?)
Updates **one interface** by setting a description and enabling the interface using `send_config_set()`.

## Files required
- **data.yaml (YAML):** ❌ Not required — config lines are static in this reference-style example.
- **template.j2 (Jinja2):** ❌ Not required — templating adds no value for a tiny fixed change.
- **main.py (Python):** ✅ Required
- **output.txt (Intended Output):** ✅ Required

## Project Structure
```text
automation/
├── main.py
└── output.txt
```

## Python File — main.py
```python
from netmiko import ConnectHandler
from getpass import getpass

# getpass() hides the password while typing (no plaintext in code)
password = getpass("Enter device password: ")

device = {
    "device_type": "cisco_ios",
    "host": "192.0.2.1",
    "username": "admin",
    "password": password,
}

# Small, static config list (reference-style)
cfg = [
    "interface GigabitEthernet0/1",
    "description CONFIGURED_VIA_NETMIKO",
    "no shutdown",
]

with ConnectHandler(**device) as conn:
    result = conn.send_config_set(cfg)
    print(result)
```

## Intended Output — output.txt
```text
interface GigabitEthernet0/1
 description CONFIGURED_VIA_NETMIKO
 no shutdown
```

---

# Example 2 — Production (Medium Complexity): YAML + Jinja2 + Verify

## Task (what exactly changes?)
Configures **multiple access ports** from YAML (description + VLAN + shutdown/no shutdown), renders the config via Jinja2, pushes it, then runs a simple **post-check**.

## Project Structure
```text
automation/
├── data.yaml
├── access_ports.j2
├── main.py
├── intended_config.txt
└── output.txt
```

## Data File — data.yaml
```yaml
device:
  device_type: cisco_ios
  host: 192.0.2.1
  username: admin

access_ports:
  - name: GigabitEthernet0/2
    description: USER_PC_01
    vlan: 10
    enabled: true

  - name: GigabitEthernet0/3
    description: USER_PC_02
    vlan: 10
    enabled: true

  - name: GigabitEthernet0/4
    description: UNUSED_PORT
    vlan: 999
    enabled: false
```

## Template File — access_ports.j2
```jinja2
{% for p in access_ports %}
interface {{ p.name }}
 description {{ p.description }}
 switchport mode access
 switchport access vlan {{ p.vlan }}
{% if p.enabled %}
 no shutdown
{% else %}
 shutdown
{% endif %}
{% endfor %}
```

## Python File — main.py
```python
from netmiko import ConnectHandler
from getpass import getpass
from jinja2 import Environment, FileSystemLoader
import yaml

password = getpass("Enter device password: ")

# Load data
with open("data.yaml", "r") as f:
    data = yaml.safe_load(f)

# Render config from template
env = Environment(loader=FileSystemLoader("."), trim_blocks=True, lstrip_blocks=True)
template = env.get_template("access_ports.j2")
rendered_config = template.render(access_ports=data["access_ports"])

# Save intended config (dry-run artifact)
with open("intended_config.txt", "w") as f:
    f.write(rendered_config)

device = {
    "device_type": data["device"]["device_type"],
    "host": data["device"]["host"],
    "username": data["device"]["username"],
    "password": password,
}

with ConnectHandler(**device) as conn:
    # Push config (config mode)
    push_result = conn.send_config_set(rendered_config.splitlines())

    # Simple post-check (read-only)
    verify = conn.send_command("show interface status")

# Save outputs for audit/troubleshooting
with open("output.txt", "w") as f:
    f.write("=== PUSH RESULT ===\n")
    f.write(push_result)
    f.write("\n\n=== POST-CHECK (show interface status) ===\n")
    f.write(verify)

print(push_result)
```

## Intended Output — intended_config.txt
```text
interface GigabitEthernet0/2
 description USER_PC_01
 switchport mode access
 switchport access vlan 10
 no shutdown
interface GigabitEthernet0/3
 description USER_PC_02
 switchport mode access
 switchport access vlan 10
 no shutdown
interface GigabitEthernet0/4
 description UNUSED_PORT
 switchport mode access
 switchport access vlan 999
 shutdown
```

## Intended Output — output.txt (example snippet)
```text
=== PUSH RESULT ===
interface GigabitEthernet0/2
description USER_PC_01
...
=== POST-CHECK (show interface status) ===
Port      Name               Status       Vlan       Duplex  Speed Type
Gi0/2     USER_PC_01         connected    10         a-full  a-100 10/100/1000BaseTX
Gi0/3     USER_PC_02         connected    10         a-full  a-100 10/100/1000BaseTX
Gi0/4     UNUSED_PORT        disabled     999        auto    auto 10/100/1000BaseTX
```

---

# Example 3 — Production (More Complex, Not Similar): Multi-Device Change Workflow + Backups

## Task (what exactly changes?)
Across **multiple devices**, creates VLANs, sets trunk allowed VLANs, **backs up running-config first**, renders per-device config from YAML/Jinja2, applies changes, and writes a per-device results log.

## Project Structure
```text
automation/
├── data.yaml
├── campus_change.j2
├── main.py
├── backup_<device>_running.txt
├── rendered_<device>_config.txt
└── results.txt
```

## Data File — data.yaml
```yaml
devices:
  - name: dist-sw1
    device_type: cisco_ios
    host: 192.0.2.11
    username: admin
    trunks:
      - GigabitEthernet1/0/49
      - GigabitEthernet1/0/50

  - name: dist-sw2
    device_type: cisco_ios
    host: 192.0.2.12
    username: admin
    trunks:
      - GigabitEthernet1/0/49
      - GigabitEthernet1/0/50

vlans:
  - id: 10
    name: USERS
  - id: 20
    name: SERVERS

trunk_allowed_vlans: "10,20"
```

## Template File — campus_change.j2
```jinja2
! VLAN creation
{% for v in vlans %}
vlan {{ v.id }}
 name {{ v.name }}
{% endfor %}

! Trunk updates
{% for t in trunks %}
interface {{ t }}
 switchport mode trunk
 switchport trunk allowed vlan add {{ trunk_allowed_vlans }}
{% endfor %}
```

## Python File — main.py
```python
from netmiko import ConnectHandler
from getpass import getpass
from jinja2 import Environment, FileSystemLoader
import yaml

password = getpass("Enter device password: ")

with open("data.yaml", "r") as f:
    data = yaml.safe_load(f)

env = Environment(loader=FileSystemLoader("."), trim_blocks=True, lstrip_blocks=True)
template = env.get_template("campus_change.j2")

results_lines = []

for dev in data["devices"]:
    device = {
        "device_type": dev["device_type"],
        "host": dev["host"],
        "username": dev["username"],
        "password": password,
    }

    try:
        with ConnectHandler(**device) as conn:
            # 1) Backup running-config (production hygiene)
            running = conn.send_command("show running-config")
            backup_file = f"backup_{dev['name']}_running.txt"
            with open(backup_file, "w") as bf:
                bf.write(running)

            # 2) Render per-device config (device-specific trunks)
            rendered = template.render(
                vlans=data["vlans"],
                trunks=dev["trunks"],
                trunk_allowed_vlans=data["trunk_allowed_vlans"],
            )
            rendered_file = f"rendered_{dev['name']}_config.txt"
            with open(rendered_file, "w") as rf:
                rf.write(rendered)

            # 3) Apply config (config mode)
            push = conn.send_config_set(rendered.splitlines())

            # 4) Optional quick verify
            verify = conn.send_command("show vlan brief")

        results_lines.append(f"{dev['name']}: SUCCESS (backup={backup_file}, rendered={rendered_file})")

    except Exception as e:
        results_lines.append(f"{dev['name']}: FAILED ({type(e).__name__}: {e})")

with open("results.txt", "w") as f:
    f.write("\n".join(results_lines))

print("\n".join(results_lines))
```

## Intended Output — results.txt
```text
dist-sw1: SUCCESS (backup=backup_dist-sw1_running.txt, rendered=rendered_dist-sw1_config.txt)
dist-sw2: SUCCESS (backup=backup_dist-sw2_running.txt, rendered=rendered_dist-sw2_config.txt)
```

## Intended Output — rendered_dist-sw1_config.txt (example snippet)
```text
vlan 10
 name USERS
vlan 20
 name SERVERS
interface GigabitEthernet1/0/49
 switchport mode trunk
 switchport trunk allowed vlan add 10,20
interface GigabitEthernet1/0/50
 switchport mode trunk
 switchport trunk allowed vlan add 10,20
```

---

## Reference

https://github.com/ktbyers/netmiko/blob/develop/EXAMPLES.md
