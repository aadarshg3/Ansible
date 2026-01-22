# Ansible Basic Class (1 & 2) — Install + 3 Playbooks + Inventories

---

## 1) Install Ansible

### Option A (recommended): Python venv + pip (clean + repeatable)
```bash
# Linux/macOS
python3 -m venv .venv
source .venv/bin/activate

python -m pip install --upgrade pip
pip install ansible

# verify
ansible --version
ansible-playbook --version
```

### Option B: Ubuntu/Debian (quick)
```bash
sudo apt update
sudo apt install -y ansible

ansible --version
```

## 2) Suggested folder layout
```text
ansible_basic_class_lab/
  playbooks/
    myhello.yaml
    loop.yaml
    get-sw-info.yaml
  inv/
    localhost.ini
    inventory.ini
```

---

## 3) Inventories

### 3.1 Localhost inventory (`inv/localhost.ini`)
Use for `myhello.yaml` and `loop.yaml`.
```ini
################################
# LOCALHOST
################################
[local]
localhost ansible_connection=local
```

### 3.2 Network lab inventory (`inv/inventory.ini`)
Use for `get-sw-info.yaml`.
```ini
################################
# SWITCHES
################################
[switches]
SW1 ansible_host=192.168.199.188
SW2 ansible_host=192.168.199.189
SW3 ansible_host=192.168.199.190

################################
# ROUTERS
################################
[routers]
R1 ansible_host=192.168.199.187
R2 ansible_host=192.168.199.185
R3 ansible_host=192.168.199.186

################################
# COMMON NETWORK VARIABLES
################################
[network:children]
switches
routers

[network:vars]
ansible_user=netg
ansible_password=india
ansible_network_os=cisco.ios.ios
ansible_connection=network_cli
```

---

## 4) Playbook 1 — `myhello.yaml`

### File: `playbooks/myhello.yaml`
```yaml
---
- name: Play A
  hosts: localhost
  connection: local
  # gather_facts: false # ==> default is true

  tasks:
    - name: task 1
      debug:
        msg: "Hello Ansible World"

    - name: task 2 
      debug:  
        msg: "Testing Task 2"    

- name: Play B
  hosts: localhost
  connection: local
  # gather_facts: false

  tasks:
    - name: task 10
      debug:
        msg: "Testing Task 10"

    - name: task 20 
      debug:  
        msg: "Testing Task 20"
```

### Run
```bash
ansible-playbook playbooks/myhello.yaml -i inv/localhost.ini
```

### Intended output
```text
TASK [task 1] *********************************************************
ok: [localhost] => {
  "msg": "Hello Ansible World"
}

TASK [task 2] *********************************************************
ok: [localhost] => {
  "msg": "Testing Task 2"
}

TASK [task 10] ********************************************************
ok: [localhost] => {
  "msg": "Testing Task 10"
}

TASK [task 20] ********************************************************
ok: [localhost] => {
  "msg": "Testing Task 20"
}
```

---

## 5) Playbook 2 — `loop.yaml` (vars + loops)

### What gets demonstrated
- `vars:` list in the play (`devices`)
- `loop:` (modern)
- `with_items:` (legacy but still seen in the wild)
- direct lists (multi-line)
- pythonic list style (`["FW1", "FW2", "FW3"]`)

### File: `playbooks/loop.yaml`
```yaml
---
- name: Play A
  hosts: localhost
  gather_facts: false

  vars:
    devices:
      - R1
      - R2
      - R3


  tasks:
    - name: "10 MYTASK"
      debug:
        msg: Hello world for Ansible


    - name:  "20 MY Loop TASK" 
      debug: 
        var: item
      loop: "{{ devices }}"

    - name:  "30 MY Loop TASK" 
      debug: 
        var: item
      with_items: "{{ devices }}"


    - name:  "40 MY Loop TASK with direct list" 
      debug: 
        var: item
      with_items: 
        - SW1
        - SW2
        - SW3

    - name:  "50 MY Loop TASK with loop keyword" 
      debug: 
        var: item
      loop: 
        - FW1
        - FW2
        - FW3

    - name:  "60 MY Loop TASK with loop keyword" 
      debug: 
        var: item
      loop: ["FW1", "FW2", "FW3"]
```

### Run
```bash
ansible-playbook playbooks/loop.yaml -i inv/localhost.ini
```

### Intended output (sample)
```text
[playbooks]$ ansible-playbook loop.yaml -i ../inv/inventory.ini 

PLAY [Play A] ***************************************************************************************************************************************************************

TASK [10 MYTASK] ************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Hello world for Ansible"
}

TASK [20 MY Loop TASK] ******************************************************************************************************************************************************
ok: [localhost] => (item=R1) => {
    "item": "R1"
}
ok: [localhost] => (item=R2) => {
    "item": "R2"
}
ok: [localhost] => (item=R3) => {
    "item": "R3"
}

TASK [30 MY Loop TASK] ******************************************************************************************************************************************************
ok: [localhost] => (item=R1) => {
    "item": "R1"
}
ok: [localhost] => (item=R2) => {
    "item": "R2"
}
ok: [localhost] => (item=R3) => {
    "item": "R3"
}

TASK [40 MY Loop TASK with direct list] *************************************************************************************************************************************
ok: [localhost] => (item=SW1) => {
    "item": "SW1"
}
ok: [localhost] => (item=SW2) => {
    "item": "SW2"
}
ok: [localhost] => (item=SW3) => {
    "item": "SW3"
}

TASK [50 MY Loop TASK with loop keyword] ************************************************************************************************************************************
ok: [localhost] => (item=FW1) => {
    "item": "FW1"
}
ok: [localhost] => (item=FW2) => {
    "item": "FW2"
}
ok: [localhost] => (item=FW3) => {
    "item": "FW3"
}

TASK [60 MY Loop TASK with loop keyword] ************************************************************************************************************************************
ok: [localhost] => (item=FW1) => {
    "item": "FW1"
}
ok: [localhost] => (item=FW2) => {
    "item": "FW2"
}
ok: [localhost] => (item=FW3) => {
    "item": "FW3"
}

PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

[playbooks]$ 
```

---

## 6) Playbook 3 — `get-sw-info.yaml` (inventory vars + host targeting)

### Purpose
Prints:
- `inventory_hostname` (logical inventory name like SW1)
- `ansible_host` (management IP from inventory)

### File: `playbooks/get-sw-info.yaml`
```yaml
---
- name: Get basic device info
  hosts: "{{ hostname }}" #switches # network
  gather_facts: false

  tasks:
    - name: Print inventory name (logical name)
      debug:
        msg: "Device Name: {{ inventory_hostname }}"

    - name: Print management IP
      debug:
        msg: "Management IP: {{ ansible_host }}"



# ansible-playbook get-sw-info.yaml -i ../inv/inventory.ini --limit=SW2
# ansible-playbook get-sw-info.yaml -i ../inv/inventory.ini -e hostname=SW1
```

### Run patterns (this is the spicy part)

#### A) Run for a single device (recommended)
```bash
ansible-playbook playbooks/get-sw-info.yaml -i inv/inventory.ini -e hostname=SW1
```

#### B) Run for a whole group
```bash
ansible-playbook playbooks/get-sw-info.yaml -i inv/inventory.ini -e hostname=switches
```

#### C) Group is broad, but only one node should run → use `--limit`
```bash
ansible-playbook playbooks/get-sw-info.yaml -i inv/inventory.ini -e hostname=switches --limit SW2
```

#### D) Inventory mismatch / “no hosts matched” situation
If `-e hostname=SW99` is passed but SW99 is not in `inventory.ini`, Ansible will say **no hosts matched**.
Fix = use a host/group name that exists in inventory **or** add it to inventory.

Quick sanity check:
```bash
ansible-playbook playbooks/get-sw-info.yaml -i inv/inventory.ini -e hostname=switches --list-hosts
```

### Intended output:
```text
TASK [Print inventory name (logical name)] ****************************
ok: [SW1] => {
  "msg": "Device Name: SW1"
}

TASK [Print management IP] ********************************************
ok: [SW1] => {
  "msg": "Management IP: 192.168.199.188"
}
```

---

## 7) Mini cheat-sheet:

### Extra vars
```bash
-e hostname=SW1
-e "hostname=switches"
```

### Limit targets (filters inventory selection)
```bash
--limit SW2
--limit switches
--limit "SW1,SW3"
--limit "all:!SW1"
ex: ansible-playbook get-sw-info.yaml -i ../inv/inventory.ini --limit 'switches:!SW1' -e hostname=switches
```

### Dry visibility (what hosts will run)
```bash
--list-hosts
ex: ansible 'switches:!SW1' --list-hosts -i ../inv/inventory.ini

```

---


