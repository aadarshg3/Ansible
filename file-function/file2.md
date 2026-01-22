# 🧠 VLAN Configuration: file example

## 📘 File Name
`vlan_config.py`

---

## 🎯 Purpose
This Python script demonstrates **file handling techniques** — how to **write, read, and append data** to files.  
The use case focuses on automating **VLAN configuration generation**, but the key objective is to understand and visualize Python’s file methods.

---

## 🧩 Topics Covered
1. File Handling using `open()` and `with open()` methods  
2. Explanation of file access modes — **Write (`w`)**, **Read (`r`)**, **Append (`a`)**  
3. Examples of file reading methods — **read()**, **readline()**, **readlines()**  
4. Best practices for managing file resources  

---

## 🧾 Section 1: Using `open()` Method

The `open()` method explicitly opens and later closes the file using `.close()`.

### 💡 Example: Writing and Reading VLAN Configuration

```python
# Writing VLAN configurations using open()
vlan_file = open("vlan_config_data.conf", "w")  # 'w' mode overwrites existing file
for vlan_id in range(2, 11):
    vlan_file.write(f"vlan {vlan_id}\n")          # Write VLAN ID
    vlan_file.write(f"  name vlan_{vlan_id}\n")   # Write VLAN name
vlan_file.close()  # Must close file manually

# Reading using open() method
file_data = open("vlan_config_data.conf", "r")  # Open file in read mode

print("\n--- Using read() ---")
print(file_data.read())  # Reads entire content as a single string

file_data.seek(0)  # Reset cursor to file start for next operation
print("\n--- Using readline() ---")
print(file_data.readline())  # Reads one line at a time

file_data.seek(0)
print("\n--- Using readlines() ---")
for line in file_data.readlines():  # Reads all lines into a list
    print(line.strip())

file_data.close()
```

---

## 🧾 Section 2: Using `with open()` Method

The `with open()` method is **Pythonic** and automatically closes the file, even if exceptions occur.  
This approach is **cleaner and safer** than manually closing files.

### 💡 Example: Reading and Appending Data

```python
# Reading with 'with open' (no need to call close())
with open("vlan_config_data.conf", "r") as file:
    print("\n--- File content using 'with open' ---")
    for line in file:
        print(line.strip())

# Appending new VLANs
with open("vlan_config_data.conf", "a") as file:
    for vlan_id in range(11, 13):
        file.write(f"vlan {vlan_id}\n")
        file.write(f"  name vlan_{vlan_id}\n")

print("\n✅ VLANs appended successfully!")
```

---

## ⚙️ File Modes Summary

| Mode | Meaning | Description |
|------|----------|-------------|
| `'r'` | Read | Opens file for reading (default). Error if file doesn’t exist. |
| `'w'` | Write | Opens file for writing (creates new file or overwrites existing one). |
| `'a'` | Append | Opens file for appending (adds to end, doesn’t erase data). |
| `'r+'` | Read & Write | Opens file for both reading and writing. |

---

## 🧠 Difference between `open()` and `with open()`

| Aspect | `open()` | `with open()` |
|--------|-----------|----------------|
| File Closing | Must be manually closed using `.close()` | Auto-closes after block ends |
| Error Handling | Risk of resource leak if exception occurs | Safely handles exceptions |
| Code Readability | Verbose | Cleaner and recommended |

🗒️ **In short:** `with open()` is the *modern, cleaner, and safer* way to handle files.
- Always prefer `with open()` for safe, clean file operations.

---

## 📤 Final Output Example

After executing both code sections, the `vlan_config_data.conf` file will contain:

```text
vlan 2
  name vlan_2
vlan 3
  name vlan_3
vlan 4
  name vlan_4
vlan 5
  name vlan_5
vlan 6
  name vlan_6
vlan 7
  name vlan_7
vlan 8
  name vlan_8
vlan 9
  name vlan_9
vlan 10
  name vlan_10
vlan 11
  name vlan_11
vlan 12
  name vlan_12
```

---



---

🕒 *Document generated on: 2025-10-23 05:40:39*
