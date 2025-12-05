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

---
## 9-10 CALLABLE CLASS ATTRIBUTES

### **1. Callable Class Attributes**
- Class attributes can be **any object**, including functions (callables)
- Functions defined inside a class become **callable attributes** of the class
- Example:
  ```python
  class Program:
      language = "Python"          # Data attribute
      
      def say_hello():             # Callable attribute
          print(f"Hello from {Program.language}")
  ```

### **2. Accessing Callable Attributes**
- **Dot notation** (recommended):
  ```python
  Program.say_hello()          # Call directly
  func = Program.say_hello     # Get the function object
  ```
- **`getattr()`** function:
  ```python
  func = getattr(Program, "say_hello")
  func()
  ```
- **Direct dictionary access** (not recommended for production):
  ```python
  func = Program.__dict__["say_hello"]
  func()
  ```

### **3. Important Distinction**
- These are **class attributes**, **NOT instance methods**
- Called directly on the class, not on instances
- Defined without `self` parameter (not bound to instances)

---

## **Tips and Precautions**

### **1. Use Dot Notation for Production Code**
```python
# ✅ RECOMMENDED (clean, readable)
Program.say_hello()
func = Program.say_hello

# ❌ AVOID in production (too verbose, error-prone)
func = Program.__dict__["say_hello"]; func()
func = getattr(Program, "say_hello"); func()
```

### **2. Remember: These Are Not Instance Methods**
- No `self` parameter required
- Cannot access instance data (no `self`)
- Can only access **class attributes**
  ```python
  class Program:
      language = "Python"
      
      # ✅ CORRECT - accesses class attribute
      def say_hello():
          print(f"Hello from {Program.language}")
      
      # ❌ WRONG - self won't be passed
      def bad_method(self):  
          print("This won't work as class attribute call")
  ```

### **3. Namespace Dictionary is for Debugging Only**
- `Program.__dict__` shows both data and callable attributes
- **Only use for debugging/inspection**, not for regular access
- Access via `__dict__` bypasses Python's internal mechanisms

### **4. Type Consistency**
- Check that what you retrieve is actually callable:
  ```python
  attr = getattr(Program, "some_attr")
  if callable(attr):
      attr()
  else:
      # Handle non-callable attribute
  ```

### **5. Attribute Name Conflicts**
- Be careful with naming - callable attributes overwrite data attributes:
  ```python
  class Program:
      language = "Python"      # Data attribute
      
      def language():          # ❌ Overwrites the data attribute!
          return "Python"
  ```

### **6. Accessing Class Attributes Inside Functions**
- Must reference the class explicitly:
  ```python
  class Program:
      language = "Python"
      
      def say_hello():
          # ✅ Explicit class reference
          print(f"Hello from {Program.language}")
          
          # ❌ Won't work - no implicit access
          # print(f"Hello from {language}")
  ```

### **7. Distinguish from Static Methods**
- These are NOT `@staticmethod` decorator methods
- These are simpler function objects attached to the class
- For actual static methods, use the decorator:
  ```python
  class Program:
      @staticmethod
      def static_method():
          return "This is a proper static method"
  ```

---

## **Quick Reference Table**

| Access Method | Use Case | Example |
|--------------|----------|---------|
| **Dot notation** | Production code | `Program.say_hello()` |
| **`getattr()`** | Dynamic attribute names | `getattr(Program, "say_" + name)()` |
| **`__dict__` access** | Debugging only | `Program.__dict__["say_hello"]()` |
| **Function retrieval** | Store for later use | `func = Program.say_hello` |

---

## **Key Takeaways**
1. **Callable attributes** are functions stored as class attributes
2. **Use dot notation** (`Class.method()`) for clean, readable code
3. **These are not instance methods** - no `self`, called on class directly
4. **`__dict__` access is for debugging** only, not production code
5. **Be explicit** when accessing class attributes inside these functions
6. **Check callability** if attribute names come from dynamic sources


---

## 11-12 CLASSES ARE CALLABLES

### **1. Classes are Callable**
- When you define a class, Python automatically makes it **callable**
- Calling a class creates and returns an **instance** of that class
- This is called **class instantiation** or **instantiating the class**
- Example:
  ```python
  class Program:
      language = "Python"
      
      def say_hello():
          print(f"Hello from {Program.language}")
  
  p = Program()  # Call the class → creates instance
  ```

### **2. Class vs Instance**
- **Class**: Template/blueprint (type `type`)
- **Instance**: Object created from class (type is the class)
- They have **separate namespaces** (`__dict__`):
  ```python
  Program.__dict__   # Contains class attributes
  p.__dict__         # Contains instance attributes (empty initially)
  ```

### **3. Type Checking Methods**
- **`type(obj)`**: Returns the class object the instance was created from
- **`isinstance(obj, Class)`**: Checks if object is instance of class (preferred)
- **`obj.__class__`**: Direct access to class attribute (not recommended)

---

## **Critical Tips and Precautions**

### **1. ALWAYS Use `type()` or `isinstance()` Instead of `__class__`**
```python
# ❌ DON'T DO THIS (can be manipulated)
obj.__class__

# ✅ DO THIS INSTEAD (safer, more reliable)
type(obj)               # Returns actual class
isinstance(obj, Class)  # Checks instance relationship
```

### **2. Demonstration of the Danger**
```python
class MyClass:
    __class__ = "faked"  # ❌ Malicious/accidental override

obj = MyClass()

print(obj.__class__)     # "faked" (WRONG!)
print(type(obj))         # <class '__main__.MyClass'> (CORRECT)
print(isinstance(obj, MyClass))  # True (still works)
```

### **3. `isinstance()` is Usually Better Than `type()`**
- `isinstance()` considers **inheritance** (more flexible)
- `type()` is stricter (exact type match only)
- Example:
  ```python
  class Parent: pass
  class Child(Parent): pass
  
  obj = Child()
  
  type(obj) is Parent          # False (exact match)
  isinstance(obj, Parent)      # True  (considers inheritance)
  ```

### **4. Namespace Separation is Crucial**
- **Class attributes** are in `Class.__dict__`
- **Instance attributes** are in `instance.__dict__`
- They **do NOT** share the same namespace
- Empty instance `__dict__` doesn't mean no attributes exist

### **5. Built-in Attributes on Instances**
Even with empty `__dict__`, instances have:
- `__class__`: Class reference (but don't use directly!)
- Other dunder methods from Python
- Accessible attributes from the class

### **6. Instantiation ≠ Attribute Copying**
When you create an instance:
- **NOT** all class attributes are copied to the instance
- Instance has **separate namespace**
- Instance can access class attributes via lookup chain

---

## **Common Pitfalls to Avoid**

### **1. Assuming `__class__` is Reliable**
```python
# ❌ RISKY - can be overridden
if obj.__class__ == MyClass: ...

# ✅ SAFE - Python-managed
if type(obj) is MyClass: ...
if isinstance(obj, MyClass): ...
```

### **2. Confusing Class and Instance Attributes**
```python
class Program:
    language = "Python"  # Class attribute
    
p = Program()
print(p.language)        # Works (looks up from class)
print(p.__dict__)        # {} (Empty! Attribute not in instance)
```

### **3. Forgetting Namespace Separation**
```python
class Test:
    x = 10  # Class attribute
    
obj1 = Test()
obj2 = Test()

Test.x = 20  # Changes for ALL instances
print(obj1.x)  # 20
print(obj2.x)  # 20 (both affected)
```

### **4. Overriding Special Attributes**
```python
# ❌ DON'T DO THIS unless you REALLY know what you're doing
class MyClass:
    __class__ = "string"    # Breaks type system
    __dict__ = {}           # Breaks attribute storage
    __name__ = "fake"       # Breaks class identification
```

---

## **Best Practices Summary**

1. **Use `isinstance()` for type checking** (handles inheritance)
2. **Use `type()` when you need exact type** (not considering inheritance)
3. **NEVER rely on `__class__`** for type checking (can be manipulated)
4. **Remember namespace separation**: Class ≠ Instance
5. **Empty `__dict__` doesn't mean no attributes** - check via `dir()` or `hasattr()`
6. **Don't override special dunder attributes** unless implementing advanced features

---

## **Quick Decision Guide**

| Situation | Recommended Approach | Why |
|-----------|---------------------|-----|
| Checking if object is type | `isinstance(obj, Class)` | Handles inheritance |
| Getting exact type | `type(obj)` | Direct, reliable |
| Debugging type issues | `type(obj)` and `isinstance()` | Both perspectives |
| Production type checks | `isinstance(obj, Class)` | Most flexible |
| Metaprogramming | `type(obj)` | Exact type needed |

---

## **Key Takeaways**
1. **Classes are callable** - calling them creates instances
2. **Use `isinstance()`** for most type checking (safest, most flexible)
3. **Avoid `__class__`** - it's a regular attribute that can be overridden
4. **Class and instance namespaces are separate** - understand the lookup chain
5. **Python's type system is dynamic but protected** - use the right tools (`type()`, `isinstance()`)

---

## 11-12 CLASSES ARE CALLABLES
