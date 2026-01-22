# Jinja2 for Network Automation 


---

# ✅ EXAMPLE 1 — Interface Config (YAML + Jinja2)

## 📁 Folder Structure
```
exa1/
├── data/
│   └── config-data.yaml
├── templates/
│   └── intf_config.j2
├── scripts/
│   └── generate_intf_config.py
└── output/
```

## 📄 Data File — config-data.yaml
```yaml
---
- intf: ge0/10
  desc: connected to user port 10
  speed: 1000
  duplex: full
- intf: ge0/11
  desc: connected to user port 11
  speed: 1000
  duplex: half
```

## 📝 Jinja2 Template — intf_config.j2
```
{% for d in data_list -%}            # Loop through each interface entry
interface {{ d.intf }}               # Insert interface name
description {{ d.desc }}             # Insert description
speed {{ d.speed }}                  # Insert speed
duplex {{ d.duplex }}                # Insert duplex mode
!
{% endfor %}
```

## 🐍 Python Script — generate_intf_config.py
```python
import yaml
from jinja2 import Environment, FileSystemLoader

# Load YAML data
data_list = yaml.safe_load(open("data/config-data.yaml"))  # Converts YAML → list of dicts

# Load templates folder
env = Environment(loader=FileSystemLoader("templates"))     # Tells Jinja2 where templates are stored

template = env.get_template("intf_config.j2")               # Load the template file

# Render template
print(template.render(data_list=data_list))                 # Pass YAML list to template
```

## 🎯 Intended Output
```
interface ge0/10
description connected to user port 10
speed 1000
duplex full
!
interface ge0/11
description connected to user port 11
speed 1000
duplex half
!
```

---

# ✅ EXAMPLE 2 — OSPF Configuration

## 📁 Folder Structure
```
exa2/
├── data/
│   └── ospf-data.yaml
├── templates/
│   └── ospf.j2
├── scripts/
│   └── generate_ospf.py
└── output/
```

## 📄 Data File — ospf-data.yaml
```yaml
router_id: 1.1.1.1
process_id: 10
networks:
  - { network: 10.0.0.0, wildcard: 0.0.0.255, area: 0 }
  - { network: 192.168.1.0, wildcard: 0.0.0.255, area: 1 }
```

## 📝 Jinja2 Template — ospf.j2
```
router ospf {{ process_id }}           # Insert OSPF process ID
 router-id {{ router_id }}             # Insert router ID
{% for n in networks -%}               # Loop through each network
 network {{ n.network }} {{ n.wildcard }} area {{ n.area }}
{% endfor %}
```

## 🐍 Python Script — generate_ospf.py
```python
import yaml
from jinja2 import Environment, FileSystemLoader

# Load YAML data
data = yaml.safe_load(open("data/ospf-data.yaml"))      # YAML → dictionary

# Initialize Jinja2 environment
env = Environment(loader=FileSystemLoader("templates"))  # Load templates directory

template = env.get_template("ospf.j2")                   # Load template

# Render template with dictionary keys unpacked
print(template.render(**data))                           # Equivalent to router_id=data["router_id"], etc.
```

## 🎯 Intended Output
```
router ospf 10
 router-id 1.1.1.1
 network 10.0.0.0 0.0.0.255 area 0
 network 192.168.1.0 0.0.0.255 area 1
```

---

# ✅ EXAMPLE 3 — BGP Configuration

## 📁 Folder Structure
```
exa3/
├── data/
│   └── bgp-data.yaml
├── templates/
│   └── bgp.j2
├── scripts/
│   └── generate_bgp.py
└── output/
```

## 📄 Data File — bgp-data.yaml
```yaml
asn: 65001
router_id: 2.2.2.2
neighbors:
  - ip: 10.10.10.1
    remote_as: 65002
  - ip: 10.10.20.1
    remote_as: 65003
```

## 📝 Jinja2 Template — bgp.j2
```
router bgp {{ asn }}                     # Insert BGP ASN
 bgp router-id {{ router_id }}           # Insert router ID
{% for nbr in neighbors -%}              # Loop through neighbors
 neighbor {{ nbr.ip }} remote-as {{ nbr.remote_as }}
{% endfor %}
```

## 🐍 Python Script — generate_bgp.py
```python
import yaml
from jinja2 import Environment, FileSystemLoader

data = yaml.safe_load(open("data/bgp-data.yaml"))        # YAML → dictionary

env = Environment(loader=FileSystemLoader("templates"))   # Load template directory
template = env.get_template("bgp.j2")                     # Load template

print(template.render(**data))                            # Render BGP config
```

## 🎯 Intended Output
```
router bgp 65001
 bgp router-id 2.2.2.2
 neighbor 10.10.10.1 remote-as 65002
 neighbor 10.10.20.1 remote-as 65003
```

---

# ✅ EXAMPLE 4 — VLAN Configuration

## 📁 Folder Structure
```
exa4/
├── data/
│   └── vlan-data.yaml
├── templates/
│   └── vlan.j2
├── scripts/
│   └── generate_vlan.py
└── output/
```

## 📄 Data File — vlan-data.yaml
```yaml
vlans:
  - id: 10
    name: SALES
  - id: 20
    name: ENGINEERING
  - id: 30
    name: HR
```

## 📝 Jinja2 Template — vlan.j2
```
{% for v in vlans -%}              # Loop through VLAN list
vlan {{ v.id }}                    # Insert VLAN ID
 name {{ v.name }}                 # Insert VLAN name
!
{% endfor %}
```

## 🐍 Python Script — generate_vlan.py
```python
import yaml
from jinja2 import Environment, FileSystemLoader

data = yaml.safe_load(open("data/vlan-data.yaml"))       # Load VLAN list

env = Environment(loader=FileSystemLoader("templates"))
template = env.get_template("vlan.j2")

print(template.render(**data))
```

## 🎯 Intended Output
```
vlan 10
 name SALES
!
vlan 20
 name ENGINEERING
!
vlan 30
 name HR
!
```

---

# ✅ EXAMPLE 5 — Loopback Interface Generation

## 📁 Folder Structure
```
exa5/
├── data/
│   └── loopbacks.yaml
├── templates/
│   └── loopbacks.j2
├── scripts/
│   └── generate_loopbacks.py
└── output/
```

## 📄 Data File — loopbacks.yaml
```yaml
loopbacks:
  - id: 0
    ip: 1.1.1.1
    mask: 255.255.255.255
  - id: 1
    ip: 2.2.2.2
    mask: 255.255.255.255
```

## 📝 Jinja2 Template — loopbacks.j2
```
{% for lo in loopbacks -%}          # Loop through loopback list
interface Loopback{{ lo.id }}       # Insert interface name
 ip address {{ lo.ip }} {{ lo.mask }} # Insert IP + mask
!
{% endfor %}
```

## 🐍 Python Script — generate_loopbacks.py
```python
import yaml
from jinja2 import Environment, FileSystemLoader

data = yaml.safe_load(open("data/loopbacks.yaml"))    # YAML → dict

env = Environment(loader=FileSystemLoader("templates"))
template = env.get_template("loopbacks.j2")

print(template.render(**data))                        # Generate config
```

## 🎯 Intended Output
```
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface Loopback1
 ip address 2.2.2.2 255.255.255.255
!
```


