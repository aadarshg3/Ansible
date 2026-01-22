
# JSON Example in Python

This example shows how to **save and read Python data** using the `json` module.

---

## 📂 Folder Structure
```bash
# Project structure
json_project/
│
├── json_example.py      # Python code for writing JSON (creates test_data.json)
├── read_example.py      # Python code for reading JSON from test_data.json
└── test_data.json       # JSON file created after running the write script
```
> Comment: `json_example.py` writes the JSON file. `read_example.py` reads it back. Keeping separate files prevents confusion for beginners.

---

## 🧠 Python Code — Writing JSON to a File (`json_example.py`)

```python
import json

intf_conf_data = [
    {"intf_name": "gig0/1", "desc": "Connected to server1", "speed": 1000, "duplex": "full"},
    {"intf_name": "gig0/2", "desc": "Connected to server2", "speed": 1000, "duplex": "full"},
    {"intf_name": "gig0/3", "desc": "Connected to server3", "speed": 1000, "duplex": "full"}
]

# Write JSON using with open() — recommended
with open("test_data.json", "w") as f:
    json.dump(intf_conf_data, f, indent=4)
```

---

## ✨ Why use `with open()`?
Using `with open()` automatically closes the file when the block finishes — safer and cleaner than calling `close()` manually.

---

## 📥 Reading JSON Data from a File (`read_example.py`)

```python
import json

# Read the existing JSON file
with open("test_data.json", "r") as f:
    data = json.load(f)

# Now `data` is a Python list of dictionaries
print(data)
```

### 🧠 Short Explanation (for beginners)
- `json.dump()` — writes Python object to a file in JSON format.
- `json.load()` — reads JSON from a file and converts it to a Python object.
- `indent=4` — makes the file human-readable with 4-space indentation.
- Keep write and read code in separate files to learn each step independently.

---

## 📘 Example Output (content of `test_data.json`)

```json
[
    {
        "intf_name": "gig0/1",
        "desc": "Connected to server1",
        "speed": 1000,
        "duplex": "full"
    },
    {
        "intf_name": "gig0/2",
        "desc": "Connected to server2",
        "speed": 1000,
        "duplex": "full"
    },
    {
        "intf_name": "gig0/3",
        "desc": "Connected to server3",
        "speed": 1000,
        "duplex": "full"
    }
]
```

---

## ⚡ Summary

| Operation | Function | Description |
|-----------|----------|-------------|
| Write JSON file | `json.dump()` | Converts Python → JSON and writes to file |
| Read JSON file | `json.load()` | Converts JSON → Python and reads from file |
| Pretty format | `indent=4` | Adds indentation for readability |
| Use `with open()` | Recommended | Auto-closes files safely |

---

✅ Now the folder structure includes the separate read file (`read_example.py`). You can run `json_example.py` first to create `test_data.json`, then run `read_example.py` to read it back.
