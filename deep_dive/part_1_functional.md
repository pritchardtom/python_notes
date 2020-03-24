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
