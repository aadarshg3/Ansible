# 🧵 VLAN and User Configuration Formatting!

## 🔹 VLAN Configuration
**🧠 Code:**
```python
vlan_id = input("Enter vlan id: ")
vlan_command = "vlan {}"
vlan_conf = vlan_command.format(vlan_id)
print(vlan_conf)
```
**📤 Output Example:**
```text
Enter vlan id: 10
vlan 10
```
## 🔹 User Configuration with Different String Formatting
**🧠 Code:**
```python
usr_name = input("Enter user name: ")
pwd = input("Enter password: ")

# Using format()
user_config = "username {} password {}".format(usr_name, pwd)

# Using concatenation
# user_config = "username" + " " + usr_name + " " + "password" + " " + pwd

# Using f-string
# user_config = f"username {usr_name} password {pwd}"

print(user_config)
```
**📤 Output Example:**
```text
Enter user name: admin
Enter password: netg
username admin password netg
```

