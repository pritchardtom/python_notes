# Python Notes

## Table of Contents

- Strings
  - General Strings
  - Escape Characters
  - Concatenation
  - Indices
  - String Formatting

################################################################################

## Strings

- Strings in Python can be declared using single, double, or triple quotes:

```
msg = 'Welcome'
msg = "Welcome"
msg = """Welcome"""
```

- What's important is consistency.  Pick one method and stick to it, for style purposes.
- What is we want to quote within a quote:

```
msg = "And he said: "Hello there!""    # produces an error
msg = "And he said: 'Hello there!'"    # works.
msg = 'And he said: "Hello there!"'    # works.
```

### String Escape Characters/Sequences

- These are common across programming languages.
- What if you wanted to declare a variable `msg` and format it's output to look like this:
        ```
        Welcome to the system.
        Please remember to logout when leaving.
        ```

- Escape Sequences come to the rescue:
  - `msg = "Welcome to the system. \nPlease remember to logout when leaving."`

- Could also use `\'` or `\"` to print relevant punctuation marks.
- List of Python Escape Sequences: https://docs.python.org/3/reference/lexical_analysis.html


### String Concatenation

- Add two strings together.
- Very simple example:

```
first_name = "Bob"
surname = "Bobbit"
name = first_name + surname
name = first_name + ' ' + second_name
```

- Concatenation only works on strings:
  - Trying to concatenate an integer with a string would fail. `"The date is " + 2019`
  - However: `"The date is " + "2019"` works as 2019 has been declared as a string.

- `+=` is a little short cut.  If we look at the name example again:
  - `first_name = "Bob"`
  - `first_name += "Bobbit"`


### String and Indices/Indexes

- Each string is essentially an array of characters, like `C`, which Python is written in.
- So the string `"Hello"` is an array of characters, with index 0 = `'H'` ...
  - `name = Bob`
  - `name[0]` --> `'B'`
  - `name[-1]` -> '`b`'



################################################################################
