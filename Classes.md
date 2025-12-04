## 5-6 OBJECTS AND CLASSES

### **What is an Object?**
- An **object** is a "container" that contains:
  - **Data/State** (called **attributes**)
  - **Functionality/Behaviors** (called **methods**)
- **Example**: A car has:
  - State: brand, model, year
  - Behavior: accelerate, brake, steer

### **Accessing Objects**
- Use **dot notation**:
  ```python
  my_car.brand = "Ferrari"
  my_car.accelerate()
  ```

### **Classes**
- A **class** is a **template/blueprint** for creating objects.
- In Python, **everything is an object**, including classes.
- **Terminology**:
  - **Class**: the "template" (also an object of type `type`).
  - **Instance**: object created from the class.
- **Example**:
  ```python
  class Person:
      pass

  p = Person()  # instance of the Person class
  ```

### **Creating Classes**
- Use the `class` keyword:
  ```python
  class MyClass:
      pass
  ```
- Even when empty, the class already has attributes and methods provided by Python (e.g., `__name__`).

### **Checking Types**
- `type(obj)`: returns the class of the object.
- `isinstance(obj, Class)`: checks if the object is an instance of a class.
- **Difference**:
  ```python
  type(p) is Person       # True (compares identity)
  isinstance(p, Person)   # True (also considers inheritance)
  ```

---

## **Tips and Precautions**

### 1. **Differentiate Class vs Instance**
- The **class** is the template (type `type`).
- The **instance** is the object created from the class.
- **Example**:
  ```python
  class Car:
      pass

  c = Car()
  type(Car)   # <class 'type'>
  type(c)     # <class '__main__.Car'>
  ```

### 2. **Pre-defined Attributes**
- Empty classes already have attributes like `__name__` (class name).
- Do not overwrite `__dunder__` attributes without understanding their purpose.

### 3. **Proper Use of `type()` and `isinstance()`**
- Prefer `isinstance()` for type checking, as it considers inheritance.
- `type()` is more restrictive (does not consider subclasses).

### 4. **Classes Are Callables**
- Classes are **callable** (like functions), creating instances:
  ```python
  obj = MyClass()  # Calling the class creates an instance
  ```

### 5. **Meta Classes (Mentioned Only)**
- Classes are created by **meta classes** (the "type of classes").
- The type of a class is `type`.
- **Caution**: Meta programming is advanced; avoid if not necessary.

### 6. **Objects vs Classes in Language**
- In other languages (Java), "object" = instance, "class" = template.
- In Python, **everything is an object**, but maintain the conceptual distinction.

---

## **Practical Summary**
- **Objects** = data + behaviors.
- **Classes** = templates for creating objects.
- **Instances** = objects created from classes.
- Use **`.`** to access attributes/methods.
- Use `isinstance()` for type checking.
- Remember: classes are also objects (of type `type`).

---
## 7-8 CLASS ATTRIBUTES

### **Class Attributes vs Instance Attributes**
- **Class attributes** belong to the class object itself, not to instances.
- They are defined directly inside the class body.
- Example:
  ```python
  class Program:
      language = "Python"   # class attribute
      version = 3.6         # class attribute
  ```

### **Accessing Attributes**
- **Dot notation** (common):
  ```python
  Program.language   # returns "Python"
  ```
- **`getattr()` function** (more flexible):
  ```python
  getattr(Program, "language")  # returns "Python"
  getattr(Program, "x", "N/A")  # returns "N/A" if attribute doesn't exist
  ```
  - Allows providing a default value if attribute is missing.
  - Dot notation raises `AttributeError` if attribute doesn't exist.

### **Modifying Class Attributes**
- **Dot notation**:
  ```python
  Program.version = 3.7
  ```
- **`setattr()` function**:
  ```python
  setattr(Program, "version", 3.7)
  ```

### **Dynamic Nature of Python**
- You can **add new attributes** to a class at runtime:
  ```python
  Program.x = 100  # New attribute added
  ```
- You can **delete attributes** at runtime:
  ```python
  del Program.x                # Using del keyword
  delattr(Program, "x")       # Using delattr() function
  ```
- **Note**: This works for user-defined classes, but not for most built-in types.

### **Class State Storage**
- Class attributes are stored in a **`__dict__`** attribute (a namespace dictionary).
- `__dict__` returns a **mapping proxy** (read-only view of the class namespace).
- Example:
  ```python
  Program.__dict__  # Shows all class attributes
  ```
- **Important**: Not all class properties are stored in `__dict__` (e.g., `__name__` is not in `__dict__`).

---

## **Important Tips and Precautions**

### 1. **Use Dot Notation for Simplicity**
- Prefer dot notation when you know the attribute exists:
  ```python
  value = Program.language
  ```
- Use `getattr()` only when you need a default value or dynamic attribute names.

### 2. **Dynamic Attribute Names**
- Use `getattr()/setattr()` when attribute names come from variables:
  ```python
  attr_name = "version"
  value = getattr(Program, attr_name)
  ```

### 3. **Avoid Direct `__dict__` Access**
- Don't access attributes via `__dict__` in production code:
  ```python
  # ❌ Avoid:
  value = Program.__dict__["language"]
  
  # ✅ Use instead:
  value = Program.language
  value = getattr(Program, "language")
  ```
- `__dict__` is useful for debugging but not for regular attribute access.

### 4. **Built-in Types Are Different**
- You cannot add/modify attributes on built-in types:
  ```python
  str.x = 100          # ❌ Fails
  "hello".x = 100      # ❌ Fails
  ```
- This only works with user-defined classes.

### 5. **Namespace Awareness**
- Remember that `__dict__` shows most but not all attributes.
- Some special attributes (like `__name__`, `__bases__`) are stored elsewhere.

### 6. **Mapping Proxy is Read-Only**
- You cannot modify `__dict__` directly:
  ```python
  Program.__dict__["language"] = "Java"  # ❌ Raises TypeError
  ```
- Always use `setattr()` or dot notation to modify attributes.

### 7. **Debugging with `__dict__`**
- Use `__dict__` to inspect class state during debugging:
  ```python
  print(Program.__dict__)  # See all stored attributes
  ```

---

## **Summary**
- **Class attributes** are defined in the class body and belong to the class itself.
- Use **dot notation** for simple access, **`getattr()/setattr()`** for dynamic cases.
- Python allows **runtime modification** of class attributes (add/change/delete).
- Class state is stored in `__dict__` (a read-only mapping proxy).
- **Don't rely on `__dict__` for regular attribute access**—use it only for debugging.
- Built-in types behave differently from user-defined classes regarding attribute modification.

