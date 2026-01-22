# dict2.md — Config Gen:

---

## 1) Map data → CLI templates via dicts

**Code**

```python
intf_config_data = {
    "intf": "Gigabit0/1",
    "desc": "Connected to server-1",
    "speed": 1000,
    "duplex": "full"     
}

intf_config_syntax = {
    "intf": "interface {}",
    "desc": "description {}",
    "speed": "speed {}",
    "duplex": "duplex {}"     
}

# intf_data = "gig0/10"
print(intf_config_data["intf"])  # quick check

# intf_syntax = "interface {}"
print(intf_config_syntax["intf"])  # quick check

# config = intf_syntax.format(intf_data)

# for key, value in intf_config_data.items():
#     # print(key)
#     config = intf_config_syntax[key].format(intf_config_data[key])  # template fill
#     print(config)

for key in intf_config_data.keys():
    # print(key)
    config = intf_config_syntax[key].format(intf_config_data[key])  # template fill
    print(config)
```

**Expected Output**

```text
Gigabit0/1
interface {}
interface Gigabit0/1
description Connected to server-1
speed 1000
duplex full
```

**Notes (if error)**

- None.

---

## 2) Generate config for multiple interfaces (list of dicts)

**Code**

```python
intf_config_data_list = [{
    "intf": "Gigabit0/1",
    "desc": "Connected to server-1",
    "speed": 1000,
    "duplex": "full"     
    },
    {
    "intf": "Gigabit0/2",
    "desc": "Connected to server-2",
    "speed": 1000,
    "duplex": "full"     
    },
    {
    "intf": "Gigabit0/3",
    "desc": "Connected to server-3",
    "speed": 1000,
    "duplex": "full"     
    },
    {
    "intf": "Gigabit0/4",
    "desc": "Connected to server-4",
    "speed": 1000,
    "duplex": "full"     
    }
]

intf_config_syntax = {
    "intf": "interface {}",
    "desc": "description {}",
    "speed": "speed {}",
    "duplex": "duplex {}"     
}

for intf_config_data in intf_config_data_list:
    for key in intf_config_data.keys():
        config = intf_config_syntax[key].format(intf_config_data[key])  # template fill
        print(config)
```

**Expected Output**

```text
interface Gigabit0/1
description Connected to server-1
speed 1000
duplex full
interface Gigabit0/2
description Connected to server-2
speed 1000
duplex full
interface Gigabit0/3
description Connected to server-3
speed 1000
duplex full
interface Gigabit0/4
description Connected to server-4
speed 1000
duplex full
```

**Notes (if error)**

- None.

---

## 3) Write intended config to a file

**Code**

```python
intf_config_data_list = [{
    "intf":"Gi0/1",
    "desc":"Connected to Core switch",
    "speed":1000,
    "duplex":"100MB-full"
},
{
    "intf":"Gi0/2",
    "desc":"Connected to Primary Router",
    "speed":1000,
    "duplex":"Auto"
},
{
    "intf":"Gi0/3",
    "desc":"Connected to Seconadry Router",
    "speed":1000,
    "duplex":"1000MB-full"
},
{
    "intf":"Gi0/4",
    "desc":"Connected to Server Router",
    "speed":"1GBPS",
    "duplex":"1GBPS-full"
}]

intf_config_syntax = {
    "intf": "interface {}",
    "desc": "description {}",
    "speed": "speed {}",
    "duplex": "duplex {}"     
}

file1 = open("intended_config.txt", "w")
for intf_config_data in intf_config_data_list:
    temp = []
    for key in intf_config_data.keys():
        config = intf_config_syntax[key].format(intf_config_data[key])  # template fill
        #  print(config)
        file1.write(config + "\n")

file1.close()
```

**Expected Output (file contents of ****\`\`****)**

```text
interface Gi0/1
description Connected to Core switch
speed 1000
duplex 100MB-full
interface Gi0/2
description Connected to Primary Router
speed 1000
duplex Auto
interface Gi0/3
description Connected to Seconadry Router
speed 1000
duplex 1000MB-full
interface Gi0/4
description Connected to Server Router
speed 1GBPS
duplex 1GBPS-full
```

**Notes (if error)**

- File is **overwritten** if it already exists (mode `"w"`).
- Mixed types (e.g., `1000` vs `"1GBPS"`) are safe with `{}` formatting.

---


---

> End of dict2.md. Ready for dict3.md (merging overlays with `|`, validating schemas, defaults, and templating into blocks)?

