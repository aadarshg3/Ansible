# Ansible Inventory guide:

This guide covers **everything important about Ansible inventory**
---

## 0) What inventory is

Inventory is Ansible’s **device address book**:
- which devices exist (hosts)
- how to connect to them (IP/username/connection type)
- how to group them (switches/routers/fw/lb, prod/lab, site-blr/site-hyd)
- what common settings they share (group variables)

---

## 1) Inventory basics: formats, hosts, and groups

For static inventory, the most common formats are **INI** and **YAML**.

### INI example (network devices)
```ini
SW-CORE-01

[switches]
SW-ACC-01
SW-ACC-02

[routers]
RTR-EDGE-01
RTR-EDGE-02

[firewalls]
FW-01
FW-02

[loadbalancers]
LB-01
LB-02
```

### YAML example (same idea)
```yaml
ungrouped:
  hosts:
    SW-CORE-01:
switches:
  hosts:
    SW-ACC-01:
    SW-ACC-02:
routers:
  hosts:
    RTR-EDGE-01:
    RTR-EDGE-02:
firewalls:
  hosts:
    FW-01:
    FW-02:
loadbalancers:
  hosts:
    LB-01:
    LB-02:
```

---

## 1.1) Default groups

Ansible always creates:
- **🧷 Fixed / Special:** `all` → every host
- **🧷 Fixed / Special:** `ungrouped` → hosts not placed in any custom group
---

## 1.2) Grouping groups: parent-child relationships

Parent groups are “groups of groups”.

- In INI use **🧷 Fixed / Special:** `:children`
- In YAML use `children:`

INI example:
```ini
[switches]
SW-ACC-01
SW-ACC-02

[routers]
RTR-EDGE-01

[firewalls]
FW-01

[network_devices:children]
switches
routers
firewalls
```

Now running on `network_devices` targets everything in switches+routers+firewalls.

---

## 1.3) Adding ranges of devices

Ranges help avoid writing 50 device names manually.

### Numeric range (Switches)
```ini
[switches]
SW-ACC-[01:05]
```
Matches: `SW-ACC-01` … `SW-ACC-05`

### Stride range (every 2nd switch)
```ini
[switches]
SW-ACC-[01:09:2]
```
Matches: `SW-ACC-01, SW-ACC-03, SW-ACC-05, SW-ACC-07, SW-ACC-09`

### Alphabet range (Firewalls)
```ini
[firewalls]
FW-[a:c]
```
Matches: `FW-a, FW-b, FW-c`

---

## 2) Passing multiple inventory sources

Multiple inventory sources can be used together.

Example:
```bash
ansible-playbook site.yml -i inv/lab.ini -i inv/prod.ini
```

**Rule:** inventory sources are merged in the order provided (later definitions can override earlier ones).

---

## 3) Organizing inventory in a directory

Instead of one file, an inventory can be a directory.

Example layout:
```text
inventory/
  lab.ini
  prod.ini
  parent-groups.ini
  dynamic-plugin.yml
```

Run:
```bash
ansible-inventory -i inventory/ --list   # parses the inventory directory and then prints the complete inventory as JSON.
# Confirms inventory has no parsing errors
# Shows exactly what Ansible “sees”
# Helps debug missing hosts/vars before running a playbook
```
### Intended output
```text
[playbooks]$ ansible-inventory -i inventory/ --list
[WARNING]: Unable to parse /automation/Network-Security-Automation/my-ansible-codes/ansible_11_01_2026/playbooks/inventory as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
{
    "_meta": {
        "hostvars": {},
        "profile": "inventory_legacy"
    },
    "all": {
        "children": [
            "ungrouped"
        ]
    }
}
[playbooks]$ ansible-inventory -i ../inv/ --list
{
    "_meta": {
        "hostvars": {
            "R1": {
                "ansible_connection": "network_cli",
                "ansible_host": "192.168.199.187",
                "ansible_network_os": "cisco.ios.ios",
                "ansible_password": "india",
                "ansible_user": "netg"
            },
            "R2": {
                "ansible_connection": "network_cli",
                "ansible_host": "192.168.199.185",
                "ansible_network_os": "cisco.ios.ios",
                "ansible_password": "india",
                "ansible_user": "netg"
            },
            "R3": {
                "ansible_connection": "network_cli",
                "ansible_host": "192.168.199.186",
                "ansible_network_os": "cisco.ios.ios",
                "ansible_password": "india",
                "ansible_user": "netg"
            },
            "SW1": {
                "ansible_connection": "network_cli",
                "ansible_host": "192.168.199.188",
                "ansible_network_os": "cisco.ios.ios",
                "ansible_password": "india",
                "ansible_user": "netg"
            },
            "SW2": {
                "ansible_connection": "network_cli",
                "ansible_host": "192.168.199.189",
                "ansible_network_os": "cisco.ios.ios",
                "ansible_password": "india",
                "ansible_user": "netg"
            },
            "SW3": {
                "ansible_connection": "network_cli",
                "ansible_host": "192.168.199.190",
                "ansible_network_os": "cisco.ios.ios",
                "ansible_password": "india",
                "ansible_user": "netg"
            }
        },
        "profile": "inventory_legacy"
    },
    "all": {
        "children": [
            "ungrouped",
            "network"
        ]
    },
    "network": {
        "children": [
            "switches",
            "routers"
        ]
    },
    "routers": {
        "hosts": [
            "R1",
            "R2",
            "R3"
        ]
    },
    "switches": {
        "hosts": [
            "SW1",
            "SW2",
            "SW3"
        ]
    }
}
[playbooks]$ 

```

---

## 3.1) Managing inventory load order

Load order matters:
- Ansible loads sources in the order supplied
- In an inventory directory, files are typically processed top-down alphabetically

**Production trick:** prefix files to control order:
```text
inventory/
  01-groups.ini
  02-lab.ini
  03-prod.ini
```

---

## 4) Adding variables to inventory

Variables can be added:
- to a host (host vars)
- to a group (group vars)

In production, most teams prefer using `group_vars/` and `host_vars/` files for cleanliness.

---

## 5) Assigning a variable to one device: host variables

Host variables apply only to one device.

INI:
```ini
[switches]
SW1 ansible_host=192.168.199.188 ansible_port=22
```

YAML:
```yaml
switches:
  hosts:
    SW1:
      ansible_host: 192.168.199.188
      ansible_port: 22
```

---

## 5.1) Inventory name vs real IP (inventory “alias” idea)

Example:
```ini
SW1 ansible_host=192.168.199.188
```

- `SW1` = inventory hostname (device label) → **🧩 Custom** (can be anything, must be unique)
- **🧷 Fixed / Special:** `ansible_host` = real IP/DNS used for connection

Also useful:
- **🧷 Fixed / Special:** `inventory_hostname` = gives `SW1` inside playbooks

Playbook snippet:
```yaml
- debug:
    msg: "Device={{ inventory_hostname }} IP={{ ansible_host }}"
```

**Intended output:**
```text
ok: [SW1] => {"msg": "Device=SW1 IP=192.168.199.188"}
```

---

## 6) Defining variables in INI format (very important)

INI has a common “typing” gotcha.

### Inline on host line
Values can be interpreted as Python-like literals.

Example:
```ini
SW1 ansible_host=192.168.199.188 enabled=true tags='["core","lab"]'
```

### In `[group:vars]`
Values are treated as strings.

Example:
```ini
[switches:vars]
enabled=FALSE
```
This becomes the string `"FALSE"`, not boolean `false`.

**Best practice:** YAML inventory is safer for data types.

---

## 7) Assigning variables to many devices: group variables

Group vars apply to every device in that group.

```ini
[switches]
SW1 ansible_host=192.168.199.188
SW2 ansible_host=192.168.199.189

[switches:vars]
ansible_user=netg
ansible_password=india
ansible_connection=network_cli
ansible_network_os=cisco.ios.ios
```

All switches inherit the same user/password/connection settings.

---

## 7.1) Inheriting values: vars for groups-of-groups

Parent groups can also have vars.

Example: all network devices share creds, but firewalls use a different user.

```ini
[switches]
SW1 ansible_host=192.168.199.188

[firewalls]
FW1 ansible_host=192.168.199.250

[network_devices:children]
switches
firewalls

[network_devices:vars]
ansible_user=netg
ansible_password=india
ansible_connection=network_cli

[firewalls:vars]
ansible_user=fwadmin
```

Result:
- Switches use `netg`
- Firewalls use `fwadmin` (child overrides parent)

---

## 8) Organizing host and group variables (group_vars/ and host_vars/)

This is how production repos avoid repeating vars.

Recommended layout:
```text
inventory/
  inventory.ini
  group_vars/
    network_devices.yml
    switches.yml
    routers.yml
    firewalls.yml
    loadbalancers.yml
  host_vars/
    SW1.yml
    RTR1.yml
```

Example `group_vars/network_devices.yml`:
```yaml
ansible_user: netg
ansible_password: india
ansible_connection: network_cli
ansible_network_os: cisco.ios.ios
```

Example `host_vars/SW1.yml`:
```yaml
ansible_host: 192.168.199.188
site: blr
role: access
```

---

## 9) How variables are merged

Before a play runs, Ansible merges vars for each device.

Practical precedence idea:
- host vars override group vars
- child group vars override parent group vars
- when using multiple inventory sources, later sources can override earlier ones

---

## 9.1) Managing inventory variable load order

Example:
```bash
ansible-playbook site.yml -i inv/lab.ini -i inv/prod.ini
```
If both define the same variable for the same device, the later one can win.

---

## 10) Connecting to devices: behavioral inventory parameters

These are **🧷 Fixed / Special** variable names Ansible understands for connections.

Common ones used in network automation:
- `ansible_host` (real IP/DNS)
- `ansible_user`
- `ansible_password`
- `ansible_port`
- `ansible_connection` (example: `network_cli`)
- `ansible_network_os` (example: `cisco.ios.ios`)
- `ansible_become`, `ansible_become_method`, `ansible_become_password` (for enable mode)

> These names are “special” — renaming them breaks automatic behavior.

---

## 10.1) Non-SSH connection types

SSH is common, but connections can be changed using:
- **🧷 Fixed / Special:** `ansible_connection=<plugin>`

For networking:
- `network_cli` (CLI over SSH)
- `httpapi` (REST API devices)
- `local` (run locally)

---

## 11) How to validate inventory (with intended outputs)

### 12.1 List devices in a group
```bash
ansible switches --list-hosts -i inventory.ini
```
**Intended output:**
```text
  hosts (3):
    SW1
    SW2
    SW3
```

### 11.2 Combine groups / exclude devices
```bash
ansible "switches:routers" --list-hosts -i inventory.ini
```
**Intended output:**
```text
  hosts (6):
    SW1
    SW2
    SW3
    RTR1
    RTR2
    RTR3
```

```bash
ansible "all:!SW1" --list-hosts -i inventory.ini
```
**Intended output:**
```text
  hosts (N):
    SW2
    SW3
    ... (everything except SW1)
```

### 11.3 Show the fully parsed inventory
```bash
ansible-inventory -i inventory.ini --list
```
This prints a JSON structure that includes:
- groups
- children
- `_meta.hostvars`

**Intended output (shortened):**
```json
{
  "_meta": {
    "hostvars": {
      "SW1": {
        "ansible_host": "192.168.199.188",
        "ansible_user": "netg",
        "ansible_connection": "network_cli"
      }
    }
  },
  "switches": { "hosts": ["SW1","SW2","SW3"] },
  "routers": { "hosts": ["RTR1","RTR2","RTR3"] },
  "all": { "children": ["ungrouped","switches","routers"] }
}
```

### 11.4 Show inventory graph
```bash
ansible-inventory -i inventory.ini --graph
```
**Intended output (example):**
```text
@all:
  |--@ungrouped:
  |--@switches:
  |  |--SW1
  |  |--SW2
  |  |--SW3
  |--@routers:
     |--RTR1
     |--RTR2
     |--RTR3
```

### 11.5 Show playbook host preview
```bash
ansible-playbook -i inventory.ini play.yml --list-hosts
```
**Intended output (example):**
```text
playbook: play.yml

  play #1 (switches): Play A
    hosts (3):
      SW1
      SW2
      SW3
```

---

## 12) ansible.cfg + default inventory + precedence

### 12.1 Set default inventory in ansible.cfg
```ini
[defaults]
inventory = /absolute/path/to/inventory.ini
```

### 12.2 Config precedence (which ansible.cfg is used?)
Common precedence order (first match wins):
1. `ANSIBLE_CONFIG` environment variable
2. `ansible.cfg` in current directory
3. `~/.ansible.cfg`
4. `/etc/ansible/ansible.cfg`

### 12.3 Make inventory work from any directory
Best approach:
```bash
export ANSIBLE_CONFIG=/absolute/path/to/ansible.cfg
```

---

## 13) Common “why is Ansible doing this?!” problems

### 13.1 Inventory parse fails → only localhost exists
Typical warnings:
```text
[WARNING]: Unable to parse ... as an inventory source
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only implicit localhost does not match 'all'
```

Fix checklist:
1. `ansible-inventory -i inventory.ini --list`
2. ensure INI headers are correct (`[group]`, `[group:vars]`, `[group:children]`)
3. ensure file path is correct and readable

---

## Final sample inventory (clean and production-ish)

`inventory.ini`
```ini
[switches]
SW1 ansible_host=192.168.199.188
SW2 ansible_host=192.168.199.189
SW3 ansible_host=192.168.199.190

[routers]
RTR1 ansible_host=192.168.199.187
RTR2 ansible_host=192.168.199.185
RTR3 ansible_host=192.168.199.186

[firewalls]
FW1 ansible_host=192.168.199.250

[loadbalancers]
LB1 ansible_host=192.168.199.240

[network_devices:children]
switches
routers
firewalls
loadbalancers

[network_devices:vars]
ansible_user=netg
ansible_password=india
ansible_connection=network_cli
ansible_network_os=cisco.ios.ios
```

---
