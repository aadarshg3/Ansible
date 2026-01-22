# 📝 Python List – 2
==

## 🔹 Nested Lists and Indexing

**🧠 Code:**
```python
list1 = ['R1', 'R2', 'R3']
print(list1[1])   # second item
print(list1[0])   # first item

list2 = ['R1', '10.4', ['R2', 10.5]]
print(list2[0])
print(list2[2][0])   # access element inside nested list
```
**📤 Output:**
```text
R2
R1
R1
R2
```

---

## 🔹 List of Lists

**🧠 Code:**
```python
listoflist = [[1,2,3], [4,5,6], [7,8,0]]
print(listoflist[0])
print(listoflist[1])
print(listoflist[2])
print(listoflist[0][1])   # access element inside sublist
```
**📤 Output:**
```text
[1, 2, 3]
[4, 5, 6]
[7, 8, 0]
2
```

---

## 🔹 Length and Type Checks

**🧠 Code:**
```python
templist = ["router", 16.6]
print(templist[0])       # string
print(templist[0][1])    # character 'o'
print(len(templist))     # length of list
print(len(templist[0]))  # length of string element
```
**📤 Output:**
```text
router
o
2
6
```

❌ Errors when trying to apply `len()` or indexing on float/int:  
```python
len(templist[1])       # TypeError: object of type 'float' has no len()
templist[1][1]         # TypeError: 'float' object is not subscriptable
```

---

## 🔹 More Nested Lists

**🧠 Code:**
```python
listoflist = [[[10,20,30], [40,50,60], [70,80,90]], [4,5,6], [7,8,9]]
print(listoflist[1][1])
print(listoflist[0][1][1])
print(listoflist[2][2])
```
**📤 Output:**
```text
5
50
9
```

❌ Using invalid tuple index:  
```python
listoflist[1,2,3,4,5]
# TypeError: list indices must be integers or slices, not tuple
```

---

## 🔹 Reversing and Slicing a List

**🧠 Code:**
```python
listoflist = [1,2,3,4,5]
print(listoflist[::-1])   # reverse
print(listoflist[0])      # first element
print(listoflist[0:2])    # slicing
```
**📤 Output:**
```text
[5, 4, 3, 2, 1]
1
[1, 2]
```

---

## 🔹 Iterating Over Lists

**🧠 Code:**
```python
ip_list = ["10.2.3.4", "11.4.56.6", "12.23.45.67", "172.16.2.3"]
for item in ip_list:
    print("configuring", item)
```
**📤 Output:**
```text
configuring 10.2.3.4
configuring 11.4.56.6
configuring 12.23.45.67
configuring 172.16.2.3
```

---

## 🔹 Nested Loops for List of Lists

**🧠 Code:**
```python
listpflist = [[[1,2,3],[4,5,6],[7,8,9]], [[10,20,30],[40,50,60], [100,200,300]]]

for item in listpflist:
    for ele1 in item:
        for ele2 in ele1:
            print(ele2)
```
**📤 Output:**
```text
1
2
3
4
5
6
7
8
9
10
20
30
40
50
60
100
200
300
```

---

## 🔹 Example: Configurations

**🧠 Code:**
```python
config_set = ["conf t", "interface ge0/1", "switchport mode access", "switchport access vlan 10"]
for item in config_set:
    print(item)
```
**📤 Output:**
```text
conf t
interface ge0/1
switchport mode access
switchport access vlan 10
```

---

## 🔹 IP Address Classification

**🧠 Code:**
```python
ip1 = input("Enter your IP address: ")
ip2 = ip1.split(".")

for ele in ip2:
    if int(ele) < 0 or int(ele) > 255:
        print("Invalid IP")
        break

if int(ip2[0]) == 10:
    print("Class A Private IP Address")
elif int(ip2[0]) == 172 and int(ip2[1]) >= 16 and int(ip2[1]) <= 31:
    print("Class B Private IP Address")
elif int(ip2[0]) == 192 and int(ip2[1]) == 168:
    print("Class C Private IP Address")
else:
    print("Not a Private IP Address")
```

**📤 Output:**
```text
Enter your IP address: 10.2.3.4
Class A Private IP Address

Enter your IP address: 172.16.2.3
Class B Private IP Address

Enter your IP address: 192.168.2.3
Class C Private IP Address

Enter your IP address: 192.67.1.2
Not a Private IP Address
```
