# Netmiko : example-1 (Multi-Device)

The goal is to **generate consistent interface configurations and deploy them to multiple Cisco IOS devices automatically**.
---

### 📌 Purpose of This Document

- **YAML** → Source of truth (data)
- **Jinja2** → Configuration rendering engine
- **Python** → Orchestration logic
- **Netmiko** → Device connectivity & configuration push

---

## 1️⃣ Python Environment Setup (optional if packages are not installed already)

> Recommended for clean dependency isolation.

```bash
python -m venv venv

# Activate virtual environment
source venv/bin/activate        # Linux / macOS
# venv\Scripts\activate       # Windows

# Install required libraries
pip install netmiko jinja2 pyyaml
```
---

## 2️⃣ Project Layout 

```text
automation/
├── inventory.yaml              # Device connection & access details
├── intf_config_data.yaml       # Interface configuration data (WHAT)
├── sw_config_template.j2       # Jinja2 template (HOW)
├── main.py                     # Automation logic (EXECUTION)
└── output_rendered_config.txt  # Rendered configuration (AUDIT)
```

---

## 3️⃣ Interface Configuration Data  
### 📄 `intf_config_data.yaml`

Defines **WHAT** should be configured on the devices.

```yaml
- intf: GigabitEthernet1
  desc: Uplink to Core Switch
  speed: 1000
  duplex: full

- intf: GigabitEthernet2
  desc: Connection to Server
  speed: 1000
  duplex: full

- intf: GigabitEthernet3
  desc: Uplink to Core Switch
  speed: 1000
  duplex: full
```

💡 **Design note**  
This file is device‑agnostic and reusable across environments.

---

## 4️⃣ Device Inventory  
### 📄 `inventory.yaml`

Defines **WHERE** the configuration will be applied.

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

✔ Scales to hundreds of devices  
✔ Clean separation of credentials & logic  

---

## 5️⃣ Jinja2 Configuration Template  
### 📄 `sw_config_template.j2`

This template defines **HOW the configuration should look**.

```jinja2
{% for data in data_list %}              {# Iterate over interface data #}
!
interface {{ data.intf }}               {# Interface name #}
 description {{ data.desc }}             {# Description #}
 speed {{ data.speed }}                  {# Speed #}
 duplex {{ data.duplex }}                {# Duplex mode #}
{% endfor %}
```


---

## 6️⃣ Automation Script  

This script performs **end‑to‑end automation**:
- Load YAML data
- Render configuration
- Save rendered output
- Push configuration using Netmiko
- Save device configuration


### 📄 `main.py`

This script performs **end‑to‑end automation**:
- Load YAML data
- Render configuration
- Save rendered output
- Push configuration using Netmiko
- Save device configuration

```python
import yaml                                  # YAML parsing
from jinja2 import Environment, FileSystemLoader  # Jinja2 engine
from netmiko import ConnectHandler           # Netmiko SSH handler

# Load interface configuration data
with open("intf_config_data.yaml") as f:
    interface_data = yaml.safe_load(f)

# Load device inventory
with open("inventory.yaml") as f:
    inventory = yaml.safe_load(f)

# Initialize Jinja2 environment
env = Environment(
    loader=FileSystemLoader("."),             # Templates from current directory
    trim_blocks=True,                         # Clean extra newlines
    lstrip_blocks=True                        # Clean leading spaces
)

# Load template
template = env.get_template("sw_config_template.j2")

# Render configuration
rendered_config = template.render(data_list=interface_data)

# Save rendered configuration for audit/debug
with open("output_rendered_config.txt", "w") as f:
    f.write(rendered_config)

# Prepare config for Netmiko
config_commands = rendered_config.splitlines()

# Push configuration to devices
for device in inventory["devices"]:
    print(f"Connecting to {device['hostname']}")

    connection = ConnectHandler(**device)
    connection.enable()

    connection.send_config_set(config_commands)
    connection.save_config()
    connection.disconnect()

    print(f"Configuration successfully applied on {device['hostname']}")
```

---

## 7️⃣ Rendered Configuration  
### (Same as Applied on Device)

```text
!
interface GigabitEthernet1
 description Uplink to Core Switch
 speed 1000
 duplex full
!
interface GigabitEthernet2
 description Connection to Server
 speed 1000
 duplex full
!
interface GigabitEthernet3
 description Uplink to Core Switch
 speed 1000
 duplex full
```

📌 **Important**  
The rendered output is **identical** to what Netmiko sends to the device.

---

## 8️⃣ Real‑Time Device Impact

After execution, **each switch (SW1, SW2, SW3)** will have:

- Interfaces configured consistently
- Human error eliminated
- Configuration saved to startup‑config

---


