## Introduction to python

- Libraries like pandas and numpy are used for data analysis.
- Tensorflow and scikit are used for machine learning and for working with AI models.
- In web development, python frameworks like Django, FastAPI, and flask helps the developers to build scalable and secure backend systems.
- Python is widely used in DevOps for writing CI/CD scripts and managing infrastructure across development pipelines. It is also commonly used to build back-end services and internal APIs.
- Python can be used for automation. You can write simple scripts to help you with repetitive tasks like extracting data from spreadsheets, sending emails, and working with files on your local machine.

## Understanding variables and data types

### How Do You Declare Variables and What Are Naming Conventions to Name Variables?
- In Python, you just write the name of the variable on the left, followed by the assignment operator, and the value you want to assign the variable on the right. Here's an example of how to declare name and age variables:
```python
name = 'John Doe'
age = 25
```
- Strings are written with single or double quotes.
- When naming variables in Python, there are some important rules you should keep in mind:

  1. Variable names can only start with a letter or an underscore (_), not a number.
  2. Variable names can only contain alphanumeric characters (a-z, A-Z, 0-9) and underscores (_).
  3. Variable names are case-sensitive — age, Age, and AGE are all considered unique.
  4. Variable names cannot be one of Python's reserved keywords such as if, class, or def.
- If you break any of those rules, your Python program will throw a SyntaxError:
```python
5variable_name = 5
     ^
SyntaxError: invalid syntax
```
- some common naming conventions for variables in Python:

  1. variables names should be in lowercase, with separate words separated by an underscore. This is called snake case:
    ```python
      my_variable_name = 'freeCodeCamp'
    ```
  2. Next, you should use descriptive names for variables. For example, if you want to save a user's age as a variable, user_age is better than age or an abbreviation like ua:
    ```python
     user_age = 30
    ```
  3. Another convention is to avoid using single-letter variable names. This is very common in Python, but should be avoided because variable names with a single letter communicate no purpose or meaning:
    ```python
    x = 56 # What do you mean by x?
    ```
- In Python, comments start with a pound symbol (#), and the language ignores everything after the # symbol on that line:
```python
# This is a single-line comment
```
- Multi-line comments can be created by using consecutive single-line comments:
```python
# This is a
# multi-line
# comment
```
### How Does the Print Function Work?
- In Python, you can use the print function to print data to the terminal.
```python
print('Hello world!') # Hello world!
```
-  You can also use the print function to show multiple values, or arguments, at once by separating them with commas. For example:
```python
print('My favorite colors are', 'blue', 'green', 'red')

# Output: My favorite colors are blue green red
```
- Python automatically adds a space between each item when you separate them with commas. This is helpful when you want to print several pieces of information together.

### What Are Common Data Types in Python and How Do You Get the Type of a Variable?
-  A data type describes the kind of value a variable holds. For example, a number, a piece of text, or a list of items.
-  Python is a dynamically-typed language like JavaScript, meaning you don't need to explicitly declare types for variables. The language knows what data type a variable is based on what you assign to it.
```python
name = 'John Doe' # Python knows this is a string
age = 25 # Python knows this is an integer
```
-  The dynamic-typing nature of Python makes coding really fast and more flexible, but it can lead to unexpected bugs because type errors are detected only when a program runs, not when the program compiles.
-  Since Python determines data types while your program is running, type-related mistakes are only discovered at that moment. When a program runs, Python executes your code line by line. If it reaches a line where a certain object is expected to behave in a way it's not able to, Python will stop and show an error.
-  The important idea is simply:

   1. In Python type errors can reveal themselves during execution, when the program is actually running and using your code.
   2. Compiled languages catch type errors during the compile step, before the program is allowed to run.
-  Because of this, you might not learn about a type mistake in Python until the program reaches that specific line of code while running.
-  Here are the most common data types you'll use in Python:

   1. Integer: A whole number without decimals, for example, 10 or -5.
  ```python
  my_integer_var = 10
print('Integer:', my_integer_var) # Integer: 10
  ```
   2. Float: A number with a decimal point, like 4.41 or -0.4.
   ```python
my_float_var = 4.50
print('Float:', my_float_var) # Float: 4.5
   ```
   3. String: A sequence of characters enclosed in single or double quotation marks like
   ```python
my_string_var = 'hello'
print('String:', my_string_var) # String: hello
   ```
   4. Boolean: A true or false type, written as True or False.
  ```python
my_boolean_var = True
print('Boolean:', my_boolean_var) # Boolean: True
   ```
   5. Set: An unordered collection of unique elements, like {0.5, 4, 'apple'}.
   ```python
my_set_var = {7, 'hello', 8.5}
print('Set:', my_set_var) # Set: {7, 'hello', 8.5}
   ``` 
   6. Dictionary: A collection of key-value pairs enclosed in curly braces, like {'name': 'John Doe', 'age': 28}.
  ```python
my_dictionary_var = {'name': 'Alice', 'age': 25}
print('Dictionary:', my_dictionary_var) # Dictionary: {'name': 'Alice', 'age': 25}
   ```
   7. Tuple: An immutable ordered collection, enclosed in parentheses, like ('apple', 4.5, 7).
   ```python
my_tuple_var = (7, 'hello', 8.5)
print('Tuple:', my_tuple_var) # Tuple: (7, 'hello', 8.5)
   ``` 
   8. Range: A sequence of numbers, often used in loops, for example, range(5).
   ```python
my_range_var = range(5)
print('Range:', my_range_var) # Range: range(0, 5)
   ``` 
   9. List: An ordered collection of elements that supports different data types.
   ```python
my_list = [22, 'Hello world', 3.14, True]
print(my_list) # [22, 'Hello world', 3.14, True]
   ``` 
   10. None: A special value that represents the absence of a value.
 ```python
my_none_var = None
print('None:', my_none_var) # None: None
   ```
-  To get the data type of a variable, you can use the type() function:
```python
my_integer_var = 10
print(type(my_integer_var))  # <class 'int'>

my_float_var = 4.50
print(type(my_float_var))  # <class 'float'>

my_string_var = 'hello'
print(type(my_string_var))  # <class 'str'>

my_boolean_var = True
print(type(my_boolean_var))  # <class 'bool'>

my_set_var = {7, 'hello', 8.5}
print(type(my_set_var))  # <class 'set'>

my_dictionary_var = {'name': 'Alice', 'age': 25}
print(type(my_dictionary_var))  # <class 'dict'>

my_tuple_var = (7, 'hello', 8.5)
print(type(my_tuple_var))  # <class 'tuple'>

my_range_var = range(5)
print(type(my_range_var))  # <class 'range'>

my_list = [22, 'Hello world', 3.14, True]
print(type(my_list)) # <class 'list'>

my_none_var = None
print(type(my_none_var))  # <class 'NoneType'>
```
-  The built-in isinstance() function lets you check if a variable matches a specific data type. It takes in an object and the type you want to check it against, then returns a boolean. Here are some examples:
```python
isinstance('Hello world', str) # True
isinstance(True, bool) # True
isinstance(42, int) # True
isinstance('John Doe', int) # False
```

## Introduction to strings

### What Are Strings and What Is String Immutability?
- A string is a sequence of characters surrounded by either single or double quotation marks.
```python
my_str_1 = 'Hello'
my_str_2 = "World"
```
- If you need a multi-line string, you can use triple double quotes or single quotes:
```python
my_str_3 = """Multiline
string"""
my_str_4 = '''Another
multiline
string'''
```
- If your string contains either single or double quotation marks, then you have two options:

  1. Use the opposite kind of quotes. That is, if your string contains single quotes, use double quotes to wrap the string, and vice versa:
  ```python
  msg = "It's a sunny day"
   quote = 'She said, "Hello World!"'
  ```
  2. Escape the single or double quotation mark in the string with a backslash (\). With this method, you can use either single or double quotation marks to wrap the string itself:
  ```python
  msg = 'It\'s a sunny day'
  quote = "She said, \"Hello!\""
  ```
- Sometimes, you may need to check if a string contains one or more characters. For that, Python provides the in operator, which returns a boolean that specifies whether the character or characters exist in the string or not.
```python
my_str = 'Hello world'

print('Hello' in my_str)  # True
print('hey' in my_str)    # False
print('hi' in my_str)    # False
print('e' in my_str)  # True
print('f' in my_str)  # False
```
- To get the length of a string, you can use the built-in len() function.
```python
my_str = 'Hello world'
print(len(my_str))  # 11
```
- Each character in a string has a position called an index. The index is zero-based. o access a character by its index, you use square brackets ([]) with the index of the character you want to access inside.
```python
my_str = "Hello world"

print(my_str[0])  # H
print(my_str[6])  # w
```
- Negative indexing is also allowed, so you can get the last character of any string with -1, the second-to-last character with -2, and so on:
```python
my_str = 'Hello world'
print(my_str[-1])  # d
print(my_str[-2]) # l
```
- In python, all data gets treated as objects, and some objects are immutable while others are mutable.
- Immutable data types can't be modified or altered once they're declared. You can point their variables at something new, which is called reassignment, but you can't change the original object itself by adding, removing, or replacing any of its elements.
- Strings are immutable data types in Python. This means that you can reassign a different string to a variable:
```python
greeting = 'hi'
greeting = 'hello'
print(greeting) # hello
```
- But direct modification of a string isn't allowed:
```python
greeting = 'hi'
greeting[0] = 'H' # TypeError: 'str' object does not support item assignment
```
- Examples of other immutable data types in Python are integer, float, boolean, tuple, and range.

### What Are String Concatenation and String Interpolation?

- In Python, you can combine multiple strings together with the plus (+) operator. This process is called string concatenation.
```python
my_str_1 = 'Hello'
my_str_2 = "World"

str_plus_str = my_str_1 + ' ' + my_str_2
print(str_plus_str) # Hello World
```
- But note that this only works with strings. If you try to concatenate a string with a number, you'll get a TypeError:
```python
name = 'John Doe'
age = 26

name_and_age = name + age
print(name_and_age) # TypeError: can only concatenate str (not "int") to str
```
- This happens because Python does not automatically convert other data types like integers into strings when you concatenate them. Python requires all elements to be strings before it can concatenate them. To fix that, you can convert the number into a string with the built-in str() function, which returns the string representation of the given object without modifying the original object:
```python
name = 'John Doe'
age = 26

name_and_age = name + str(age)
print(name_and_age) # John Doe26
```
- You can also use the augmented assignment operator for concatenation. This is represented by a plus and equals sign (+=), and performs both concatenation and assignment in one step. Here's it in action:
```python
name = 'John Doe'
age = 26

name_and_age = name  # Start with the name
name_and_age += str(age)  # Append the age as string

print(name_and_age)  # John Doe26
```
- The process of inserting variables and expressions into a string is called string interpolation. Python has a category of string called f-strings (short for formatted string literals), which allows you to handle interpolation with a compact and readable syntax.
- F-strings start with f (either lowercase or uppercase) before the quotes, and allow you to embed variables or expressions inside replacement fields indicated by curly braces ({}). 
```python
name = 'John Doe'
age = 26
name_and_age = f'My name is {name} and I am {age} years old'
print(name_and_age) # My name is John Doe and I am 26 years old

num1 = 5
num2 = 10
print(f'The sum of {num1} and {num2} is {num1 + num2}') # The sum of 5 and 10 is 15
```
- Note how you don't need to convert non-string types with the str() function. In the example above, the value of the age, num1, and num2 variables is converted under the hood into a string during the interpolation process.

### What Is String Slicing and How Does It Work?
- String slicing lets you extract a portion of a string or work with only a specific part of it.
```python
string[start:stop]
```
- If you want to extract characters from a certain index to another, you just separate the start and stop indices with a colon:
```python
my_str = 'Hello world'
print(my_str[1:4]) # ell
```
- Note that the stop index is non-inclusive, so [1:4] just extracted the characters from index 1, and up to, but not including, the character at index 4.
- You can also omit the start and stop indices, and Python will default to 0 or the end of the string, respectively. For example, here's what happens if you omit the start index:
```python
my_str = 'Hello world'
print(my_str[:7])  # Hello w
```
- And here's what happens if you omit the stop index:
```python
my_str = 'Hello world'
print(my_str[8:])  # rld
```
- Note that slicing a string does not modify the original string:
- You can also omit both the start and stop indices, which will extract the whole string:
```python
my_str = 'Hello world'
print(my_str[:])  # Hello world
```
- Apart from the start and stop indices, there's also an optional step parameter, which is used to specify the increment between each index in the slice.
- Here's the syntax for that:
```python
string[start:stop:step]
```
- In the example below, the slicing starts at index 0, stops before 11, and extracts every second character:
```python
my_str = 'Hello world'
print(my_str[0:11:2])  # Hlowrd
```
- A helpful trick you can do with the step parameter is to reverse a string by setting step to -1, and leaving start and stop blank:
```python
my_str = 'Hello world'
print(my_str[::-1]) # dlrow olleH
```

### What Are Some Common String Methods?
- Python provides a number of built-in methods:

  1. upper(): Returns a new string with all characters converted to uppercase.
 ```python
my_str = 'hello world'

uppercase_my_str = my_str.upper()
print(uppercase_my_str)  # HELLO WORLD
 ```
  2. lower(): Returns a new string with all characters converted to lowercase.
```python
my_str = 'Hello World'

lowercase_my_str = my_str.lower()
print(lowercase_my_str)  # hello world
```
  3. strip(): Returns a new string with the specified leading and trailing characters removed. If no argument is passed it removes leading and trailing whitespace.
```python
my_str = '  hello world  '

trimmed_my_str = my_str.strip()
print(trimmed_my_str)  # "hello world"
```
  4. replace(old, new): Returns a new string with all occurrences of old replaced by new.
```python
my_str = 'hello world'

replaced_my_str = my_str.replace('hello', 'hi')
print(replaced_my_str)  # hi world
```
  5. split(separator): Splits a string on a specified separator into a list of strings. If no separator is specified, it splits on whitespace.
```python
my_str = 'hello world'

split_words = my_str.split()
print(split_words)  # ['hello', 'world']
```
  6. join(iterable): Joins elements of an iterable into a string with a separator.
```python
my_list = ['hello', 'world']

joined_my_str = ' '.join(my_list)
print(joined_my_str)  # hello world
```
  7. startswith(prefix): Returns a boolean indicating if a string starts with the specified prefix.
```python
my_str = 'hello world'

starts_with_hello = my_str.startswith('hello')
print(starts_with_hello)  # True
```
  8. endswith(suffix): Returns a boolean indicating if a string ends with the specified suffix.
```python
my_str = 'hello world'

ends_with_world = my_str.endswith('world')
print(ends_with_world)  # True
```
  9. find(substring): Returns the index of the first occurrence of substring, or -1 if it doesn't find one.
```python
my_str = 'hello world'

world_index = my_str.find('world')
print(world_index)  # 6
```
  10. count(substring): Returns the number of times a substring appears in a string.
```python
my_str = 'hello world'

o_count = my_str.count('o')
print(o_count)  # 2
```
  11. capitalize(): Returns a new string with the first character capitalized and the other characters lowercased.
```python
my_str = 'hello world'

capitalized_my_str = my_str.capitalize()
print(capitalized_my_str)  # Hello world
```
  12. isupper(): Returns True if all letters in the string are uppercase and False if not.
```python
my_str = 'hello world'

is_all_upper = my_str.isupper()
print(is_all_upper)  # False
```
  13. islower(): Returns True if all letters in the string are lowercase and False if not.
```python
my_str = 'hello world'

is_all_lower = my_str.islower()
print(is_all_lower)  # True
```
  14. title(): Returns a new string with the first letter of each word capitalized.
   ```python
my_str = 'hello world'

title_case_my_str = my_str.title()
print(title_case_my_str)  # Hello World
```
## Numbers and Mathematical Operations

### How Do You Work With Integers and Floating Point Numbers?

- Integers and floats are the primary numeric data types in Python.
- Integers are whole numbers without decimal points, either positive or negative:
- 
