# String Manipulation & Validation!

Using String methods like: `.isalpha()`, `.isnumeric()`, `.replace()`, string immutability, `split()` and `join()` methods, and URL formatting using `.format()` and f-strings.

## 🔹 Extracting Only Alphabetic Characters
**🧠 Code:**
```python
data = "I&*n5^7d&(i^&a"
naushad = ""
for deepak in data:
    if deepak.isalpha():  # Only keep alphabetic chars
        naushad += deepak
print(naushad)
```
**📤 Output:**
```text
India
```
# 🧼 Cleaning Strings and Character Filtering

## 🔹 Extracting Letters, Spaces, and Exclamation Marks
**🧠 Code:**
```python
str_data = "I#$n54d*(i)*a &i(^s) %^G4r%^e**(a)t^&&!"

temp_str = ""
for ele in str_data:
    if ele.isalpha() or ele == " " or ele == "!":
        temp_str += ele

print(temp_str)
```
**📤 Output:**
```text
India isGreat!
```
## 🔹 Partial String Cleanup (First 11 Characters Only)
**🧠 Code:**
```python
temp_str = ""
for i in range(0, 11):
    if str_data[i].isalpha() or str_data[i] == " ":
        temp_str += str_data[i]
print(temp_str)
```
**📤 Output:**
```text
India i
```
## 🔹 Character-Level Search in Strings
**🧠 Code:**
```python
device = "Router"
for item in device:
    if item == "u":
        print(item)
```
**📤 Output:**
```text
u
```


## 🔹 Using `isalpha()`, `isnumeric()`, `isdigit()`, and `isupper()`
**🧠 Code:**
```python
var1 = "10"
var2 = "NetG"
print(var1.isalpha())    # False
print(var2.isalpha())    # True
print(var1.isnumeric())  # True
print(var1.isdigit())    # True
print(var2[0].isupper()) # True
print(var2[0].islower()) # False
```
## 🔹 Strings are Immutable vs Lists
**🧠 Code:**
```python
output = "ThisisIndia"
# output[1] = 'L'  # ❌ Error
device_list = ["R1", "R2", "R3"]
device_list[1] = "R5"  # ✅ Lists are mutable
print(device_list)
```
**📤 Output:**
```text
['R1', 'R5', 'R3']
```
## 🔹 Replacing Characters in Strings
**🧠 Code:**
```python
output = "ThisisIndia"
output = output.replace("h", "H")
output = output.replace('s', 's ', 1)
print(output)
```
**📤 Output:**
```text
This isIndia
```
## 🔹 URL Formatting
**🧠 Code:**
```python
method = "GET"
domain = "netgindia.com"
endpoint = "Contact Us"

url1 = "{} https://{}/{}".format(method, domain, endpoint)
url2 = f"{method} https://{domain}/{endpoint}"

print(url1)
print(url2)
```
**📤 Output:**
```text
GET https://netgindia.com/Contact Us
GET https://netgindia.com/Contact Us
```
## 🔹 Splitting and Joining IP Addresses
**🧠 Code:**
```python
ip = "10.2.3.4"
newip = ip.split('.')
print(newip)            # ['10', '2', '3', '4']
print(".".join(newip))  # '10.2.3.4'
print("".join(newip))   # '10234'
```
