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


#### Method Two: ``
