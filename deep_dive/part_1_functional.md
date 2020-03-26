# Python Deep Dive - Part I (Functional)

## Variables and Memory

### Variables are Memory References

- When we store data in memory, it's added to the Heap.
- Storing and retrieving objects to/from the Heap is handled by the **Python Memory Manager**.
- Variable names simply reference the object's memory address in the Heap.
  - Similar to a pointer/alias.
- We can find the memory address referenced by a variable by using the `id()` function.

#### The `id()` Function

```python
age = 42
print(id(age))       # 4497455520  (base-10)
print(hex(id(age)))  # 0x10c11b9a0 (hexadecimal/base-16)

```

- Remember: `age` does not really `= 42`.
- `age` is simply a reference to an integer object located at the memory address `id(age)`.
  - In this example it was `4497455520`.


### Reference Counting

- When we declare `age = 42`, what happens?
- Python creates an object of type integer (with a value of 42) at some address in memory.
- `age` simply references/points to that object's memory address.
- We can keep track of these objects in memory using `id().`
- And we can keep track of how many variables, if any, are pointing to the same object in memory.
- This is called **Reference Counting**.
- The **Python Memory Manager** handles all this for us.
- But how can we track these, if we wanted to?

#### Method One: `sys.getrefcount()`

- This should return a result of `1`, as we only have one `name` object in memory:

```python
import sys

name = "Bob Boblaw"
print(sys.getrefcount(name))    # output: 2

```

- Okay, we got `2` instead of `1`.  Why?
- Because the `sys.getrefcount()` function takes `name` as an argument.
- This means it creates an extra reference to `name`, hence the answer `2.`
- Let's try another example, to check this behaviour.

```python
import sys

name = "Bob Boblaw"
print(sys.getrefcount(name))    # output: 2
print(sys.getrefcount(name)-1)  # output: 1

default_name = name
print(sys.getrefcount(name))    # output: 3
print(sys.getrefcount(name)-1)  # output: 2

# just to prove they point to same thing:
print(id(name))                 # 4501509168
print(id(default_name))         # 4501509168

```

- It's trivial to subtract one to obtain the real Reference Count of a variable.
- However, there's a way which does this for us, and is therefore preferable to `sys.getrefcount()`


#### Method Two: Using `ctypes`

```python
import ctypes

name = "Bob Boblaw"
print(ctypes.c_long.from_address(id(name)).value) # output: 1

default_name = name
print(ctypes.c_long.from_address(id(name)).value) # output: 2

default_name = None
print(ctypes.c_long.from_address(id(name)).value) # output: 1

```

- But why does this `ctypes` function give the correct Reference Count for `name`, when `sys.getrefcount()` failed?
- Because the `ctypes` function passes the memory address, and not the reference itself, only a temporary pointer to `name` is used.
- This pointer is created via the `id(name)` value passed to the `ctypes` function.
- By the time it is received by the `ctypes` function, the temporary reference/pointer is gone.
- This is why we get the correct values, without having to subtract `1`.
- Recall that `sys.getrefcount()` passed the actual *reference* (i.e., the variable `name`), which created this additional count.


### Garbage Collection

- As documented, Python keeps track of all references to objects in memory.
- When the **reference count** of an object reaches zero, the Python Memory Manager destroys object/reclaims memory.
- However, sometimes this doesn't work.
  - Particularly when dealing with **circular references**, which reference counting cannot detect.

#### Circular References Example (with Garbage Collection)

- Let's create a simple circular reference example:

```python
import ctypes

class A(object):
        def __init__(self):
                self.b = B(self)  # pass instance of A to B
                print(f"A: self: {hex(id(self))}, b: {hex(id(self.b))}")


class B(object):
        def __init__(self, a):
                self.a = a
                print(f"B: self: {hex(id(self))}, a: {hex(id(self.a))}")


circ_ref = A()

```

- Printed output of this will look similar to:

```
B: self: 0x103a253c8, a: 0x103a25278
A: self: 0x103a25278, b: 0x103a253c8
```

- We can confirm all this via:

```python
print(hex(id(circ_ref)))      # output: 0x103a25278
print(hex(id(circ_ref.b)))    # output: 0x103a253c8
print(hex(id(circ_ref.b.a)))  # output: 0x103a25278
```

- And check the reference count of these:

```python

print(ctypes.c_long.from_address(id(circ_ref)).value)     # output: 2
print(ctypes.c_long.from_address(id(circ_ref.b)).value)   # output: 1
print(ctypes.c_long.from_address(id(circ_ref.b.a)).value) # output: 2

```

- Now, let's save the memory addresses of each into separate variables:

```python
a_id = id(circ_ref)
b_id = id(circ_ref.b)
```

- How does Python handle this:

```
circ_ref = None
```

- If we search in

def object_by_id(object_id):
    for obj in gc.get_objects():
        if id(obj) == object_id:
            return "Object exists"
    return "Not found"


### Variable Reassignment

- How does Python handle variable reassignment?

```python

age = 42
print(hex(id(age))) # output: 0x10096cea0

age = 42 + 1
print(hex(id(age))) # output: 0x10096cec0

```

- The memory address of `age` is different each time.  Why?
- We do not change the value inside of the object `age` during reassignment.
- Instead Python creates a new object, and the reference of `age` will be updated to point to this new object in memory.
- What about if two objects have the same value?

```python

balance = 100
deposit = 100

# these two point to same object in memory:
print(hex(id(balance))) # output: 0x10096d5e0
print(hex(id(deposit))) # output: 0x10096d5e0

# we alter deposit, it points to new object at different memory address:
deposit = 0
print(hex(id(deposit))) # output: 0x10096c960

# what if we reassign it back to 100?
deposit = 100
print(hex(id(deposit))) # output: 0x10096d5e0

# it points back to the same memory address as balance.

```

- This behaviour will be explored more in different section.


#
#
#
#
#
