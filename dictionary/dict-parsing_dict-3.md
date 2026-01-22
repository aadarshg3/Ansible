# 📘 device_info_dict.md — Accessing Device Info from Nested Dictionary


---

## 🧾 Full Device Dictionary
```python
devices = {
    "device_info1": {
        "hostname": "NDLS_R1",
        "host_ip": "10.2.3.10",
        "site_id": "NDLS",
        "model": "9300",
        "device_type": "access_switch"
    },
    "device_info2": {
        "hostname": "NDLS_R2",
        "host_ip": "10.2.3.20",
        "site_id": "NDLS",
        "model": "9300",
        "device_type": "core_switch"
    },
    "device_info3": {
        "hostname": "NDLS_R3",
        "host_ip": "10.2.3.30",
        "site_id": "NDLS",
        "model": "9300",
        "device_type": "access_switch_30"
    },
    "device_info4": {
        "hostname": "NDLS_R4",
        "host_ip": "10.2.3.40",
        "site_id": "NDLS",
        "model": "9300",
        "device_type": "core_switch"
    }
}
```

---

## ▶️ Access Specific IP
Use basic key access to retrieve a single value from a nested dictionary:

```python
print(devices["device_info3"]["host_ip"])
```

📤 **Expected Output**
```text
10.2.3.30
```

---

## 🔍 Filter Devices by IP Ending with `.30`
Use a loop to filter values based on a condition:

```python
for key, value in devices.items():
    if value["host_ip"].endswith(".30"):
        print(value["host_ip"])
```

📤 **Expected Output**
```text
10.2.3.30
```

---


