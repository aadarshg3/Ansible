# 🧮 CALC.MD  


## ⚙️ Unified Function — **Calculator**

```python
def calculator(a, b):              # single function doing all 4 ops
    add_r = a + b
    sub_r = a - b
    mul_r = a * b
    div_r = a / b
    return add_r, sub_r, mul_r, div_r   # returns tuple
```

---

## 🧪 Function Call — All at Once

```python
x = 20
y = 10
result = calculator(x, y)      # get all results together
print(result)
print(type(result))
```

### 🧾 Output
```
(30, 10, 200, 2.0)
<class 'tuple'>
```


## 🧾 Dynamic User Input Example

```python
def calculator(a, b):
    return a + b, a - b, a * b, a / b

x = int(input("Enter value of x: "))
y = int(input("Enter value of y: "))

result = calculator(x, y)
print(result)
```

### 🧾 Example Output
```
Enter value of x: 12
Enter value of y: 4
(16, 8, 48, 3.0)
```

---

## ✅ Best Practice

```python
def calculator(a, b):
    return a + b, a - b, a * b, a / b

print(calculator(10, 5))    # (15, 5, 50, 2.0)
```
