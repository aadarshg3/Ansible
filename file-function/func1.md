# 🔍 FUNCTION1.MD - Document-1  


## 🧠 What is a Function?
A **function** is a reusable block of code designed to perform a specific task.  
It helps **organize**, **modularize**, and **re-use** code efficiently.

> Think of it like a *machine* — inputs go in, some processing happens inside, and output comes out.

---

## 🧾 Example 1: Simple `sum()` Function (2 parameters)

```python
def sum(a, b):  # 'a' and 'b' are input parameters
    add = (a + b)  # performs addition operation
    return add     # returns the result back
```

### 🧩 Understanding the Function Call (2 parameters)

```python
x = 10
y = 20

temp_var = sum(x, y)  # calling the function
print(temp_var)
```

### 🧠 Explanation (2 parameters)
When we call `sum(x, y)` —  
- Value of **x** (10) is *copied* into **a** inside the function.  
- Value of **y** (20) is *copied* into **b** inside the function.  
- Inside the function, `a` and `b` are *local variables* — they exist only while the function runs.  
- The result (`a + b = 30`) is returned and stored in `temp_var`.  
- Note: `x` and `y` remain unchanged; only their *values* are copied, not the variables themselves.

### 🧾 Output
```text
30
```

---

## 🧾 Example 2: Multiplication — Three Parameters

```python
def mul_three(a, b, c):  # function with three parameters
    result = a * b * c   # chained multiplication
    return result
```

### 🧪 Function Call (3 parameters)

```python
res = mul_three(2, 3, 4)
print(res)
```

### 🧾 Output
```text
24
```

💡 **Note:** When the function has **three parameters**, you must pass **exactly three arguments** while calling it.

---

## ⚠️ Argument Mismatch Errors (Works the same for any number of parameters)

Whether your function has **2 parameters, 3 parameters, or 35 parameters**, the rule is the same: the **call must provide exactly the same number of arguments**. If not, Python raises a `TypeError`.

### Example — Missing Arguments (less than defined)
```python
# function with 3 parameters
result = mul_three(2, 3)   # only 2 arguments passed
```
**Error output:**
```text
TypeError: mul_three() missing 1 required positional argument: 'c'
```

### Example — Too Many Arguments (more than defined)
```python
# function with 3 parameters
result = mul_three(2, 3, 4, 5)   # 4 arguments passed instead of 3
```
**Error output:**
```text
TypeError: mul_three() takes 3 positional arguments but 4 were given
```

### Illustration with many parameters (e.g., 35)
```python
# imagine a function defined with 35 parameters:
# def many(a1, a2, ..., a35): pass

# calling with wrong count (34 or 36) will raise the same kind of TypeError:
# TypeError: many() missing 1 required positional argument: 'a35'
# OR
# TypeError: many() takes 35 positional arguments but 36 were given
```

---

## 🧾 Example 3: Multiple Returns and Operations

```python
def sub(a, b):
    output1 = a - b       # subtraction result
    output2 = a - a       # always 0
    return output1, output2  # returns multiple results as a tuple

def mul(a, b):
    output1 = a * b
    return output1

temp1 = sub(10, 20)
print(temp1)  # (-10, 0)

temp2 = sub(20, 5)
print(temp2)  # (15, 0)

temp3 = mul(10, 20)
print(temp3)  # 200
```

### 🧾 Output
```text
(-10, 0)
(15, 0)
200
```

---

## 🧾 Example 4: Classic Function Syntax — Using `def`

```python
def add(a, b):  # 'def' defines a function
    output = a + b
    return output

temp = add(10, 20)
print(temp)
```

### 🧾 Output
```text
30
```

🧩 **Note:**  
- `def` = keyword to define a function.  
- Function name (`add`) = identifier, can be anything descriptive.  

---

## 🧾 Example 5: ✅ Best Practice — Clean and Modular Functions

```python
def add(a, b):
    return a + b

def sub(a, b):
    return a - b

def mul(a, b):
    return a * b

def div(a, b):
    return a / b

# Function calls
print(add(10, 20))  # 30
print(sub(10, 20))  # -10
print(mul(10, 20))  # 200
print(div(10, 20))  # 0.5
```

### 💬 Comment
Each function has a **single clear purpose** — this makes code clean and reusable.  

---

## 🧾 Example 6: 🚀 Taking User Input Dynamically

```python
#!/usr/bin/env python  # Shebang line for Linux execution

def sum(a, b):
    return a + b

x = input("Enter value of x: ")  # user input (string)
y = input("Enter value of y: ")

temp = sum(int(x), int(y))  # convert to int before addition
print(temp)
```

### 🧾 Expected Output
```text
Enter value of x: 15
Enter value of y: 25
40
```

---

## 🧩 Summary
- Function = block of reusable code.  
- Parameters = placeholders for input values.  
- `return` sends result back.  
- Variables inside function are **local**.  
- Inputs are **copied**, not linked.  
- Always use clear, simple names and comments.  
- Matching the number of arguments is **mandatory** — else Python raises a **TypeError**.
