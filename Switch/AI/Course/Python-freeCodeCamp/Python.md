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
- Below are the operation with integers:

  1. addition
```python
my_int_1 = 56
my_int_2 = 12

sum_ints = my_int_1 + my_int_2
print('Integer Addition:', sum_ints) # Integer Addition: 68
```
  2. subtraction
```python
my_int_1 = 56
my_int_2 = 12

# Subtraction
diff_ints = my_int_1 - my_int_2
print('Integer Subtraction:', diff_ints) # Integer Subtraction: 44
```
  3. multiplication
  ```python
my_int_1 = 12
my_int_2 = 4

# Multiplication
product_ints = my_int_1 * my_int_2
print('Integer Multiplication:', product_ints) # Integer Multiplication: 48
```
  4. division
  ```python
my_int_1 = 56
my_int_2 = 12

# Division
div_ints = my_int_1 / my_int_2
print('Integer Division:', div_ints) # Integer Division: 4.666666666666667
```
- Below are the operations with float:

  1. addition
```python
my_float_1 = 5.4
my_float_2 = 12.0

float_addition = my_float_1 + my_float_2
print('Float Addition:', float_addition) # Float Addition: 17.4
```
  2. subtraction
```python
my_float_1 = 5.4
my_float_2 = 12.0

float_subtraction = my_float_2 - my_float_1
print('Float Subtraction:', float_subtraction) # Float Subtraction: 6.6
```
  3. multiplication
```python
my_float_1 = 5.4
my_float_2 = 12.0

float_multiplication = my_float_2 * my_float_1
print('Float Multiplication:', float_multiplication) # Float Multiplication: 64.80000000000001
```
  4. division
```python
my_float_1 = 5.4
my_float_2 = 12.0

float_division = my_float_2 / my_float_1
print('Float Division:', float_division) # Float Division: 2.222222222222222
```
- If you add an integer and a float, the result is automatically converted to a float:
```python
my_int = 56
my_float = 5.4

sum_int_and_float = my_int + my_float

print(sum_int_and_float) # 61.4
print(type(sum_int_and_float)) # <class 'float'>
```
- If you mix integers and floats, Python will return a float as the result.
- You can also perform more complex arithmetic calculations such as getting the remainder of two numbers with the modulo operator, floor division, and exponentiation with both integers and floats.
```python
my_int_1 = 56
my_int_2 = 12

my_float_1 = 5.4
my_float_2 = 12.0

mod_ints = my_int_1 % my_int_2
mod_floats = my_float_2 % my_float_1

print('Integer Modulo:', mod_ints) # Integer Modulo: 8
print('Float Modulo:', mod_floats) # Float Modulo: 1.1999999999999993
```
- Floor division divides two numbers and returns the greatest integer less than or equal to the result. This is done with the double forward slash operator (//):
```python
my_int_1 = 56
my_int_2 = 12

my_float_1 = 5.4
my_float_2 = 12.0

floor_div_ints = my_int_1 // my_int_2
floor_div_floats = my_float_2 // my_float_1

print('Integer Floor Division:', floor_div_ints) # Integer Floor Division: 4
print('Float Floor Division:', floor_div_floats) # Float Floor Division: 2.0
```
- Exponentiation raises a number to the power of another, and is done with the double asterisk operator (**):
```python
my_int_1 = 56
my_int_2 = 12

my_float_1 = 5.4
my_float_2 = 12.0

exp_ints = my_int_1 ** my_int_2
exp_floats = my_float_1 ** my_float_2

print('Integer Exponentiation:', exp_ints) # Integer Exponentiation: 951166013805414055936
print('Float Exponentiation:',  exp_floats) # Float Exponentiation: 614787626.1765089
```
- Python also provides built-in functions for converting either numeric data or strings into integers or floats.
- The float() function returns a floating-point number constructed from the given number:
```python
my_int_1 = 56
my_float_1 = float(my_int_1)

print(my_float_1)  # 56.0
print(type(my_float_1))  # <class 'float'>
```
- The int() function returns an integer constructed from the given number:
```python
my_float = 12.92563
my_int = int(my_float)

print(my_int)  # 12
print(type(my_int))  # <class 'int'>
```
- Also, you can use the same built-in functions to convert a string into either a float or integer:
```python
my_str_int = '45'
my_str_float = '7.8'

converted_int = int(my_str_int)
converted_float = float(my_str_float)

print(converted_int, type(converted_int))  # 45 <class 'int'>
print(converted_float, type(converted_float))  # 7.8 <class 'float'>
```
-  Here are some other methods Python provides for working with integers and floats.

   1. round(): Rounds a number to the specified number of decimal places. By default this function rounds to the nearest integer, and returns a whole number with no decimal places:
```python
my_int_1 = 4.798
my_int_2 = 4.253

rounded_int_1 = round(my_int_1)
rounded_int_2 = round(my_int_2, 1)

print(rounded_int_1) # 5
print(rounded_int_2) # 4.3
```
   2. abs(): returns the absolute value of a number,
   ```python
 num = -15

absolute_value = abs(num)
print(absolute_value) # 15
 ```
   3. pow(): raises a number to the power of another or performs modular exponentiation.
   ```python
result_1 = pow(2, 3)  # Equivalent to 2 ** 3
print(result_1)  # 8

result_2 = pow(2, 3, 5)  # (2 ** 3) % 5
print(result_2)  # 3
``` 
### How Do Augmented Assignments Work?
- Augmented assignment combines a binary operation with an assignment in one step. It takes a variable, applies an operation to it with another value, and stores the result back into the same variable.
- addition
```python
my_var = 10
my_var += 5

print(my_var) # 15
```
- subtraction
```python
count = 14
count -= 3

print(count) # 11
```
- multiplication
```python
product = 65
product *= 7

print(product) # 455

greet = 'Hello'
greet *= 3

print(greet) # HelloHelloHello
```
-  division
```python
price = 100
price /= 4

print(price) # 25.0
```
-  floor division
```python
total_pages = 23
total_pages //= 5

print(total_pages) # 4
```
-  module assignment
```python
bits = 35
bits %= 2

print(bits) # 1
```
-  exponentiation assignment
```python
power = 2
power **= 3

print(power) # 8
```
-  Throws TypeError
```python
reet = 'Hello'
greet -= ' World'

print(greet) # TypeError: unsupported operand type(s) for -=: 'str' and 'str'


greet = 'Hello'
greet /= 'World'

print(greet) # TypeError: unsupported operand type(s) for /=: 'str' and 'str' 
```
-  If you're wondering if increment and decrement operators (++ and  --) work in Python, they don't. That's because Python deliberately avoids C-style increment and decrement shortcuts in order to keep the language clear and explicit.
- Instead of x++, you can simply write x += 1, which makes it obvious that you're incrementing the value of x by 1.
- Writing ++x in Python just applies the unary plus twice, and does not increment anything:
```python
my_var = 5

print(+my_var)   # 5
print(++my_var)  # 5
print(+++my_var) # 5

my_var += 1

print(my_var) # 6
```

## Boolean and Conditionals

### How Do Conditional Statements and Logical Operators Work?
- Below are the comparison operators in Python:

  <img width="898" height="514" alt="image" src="https://github.com/user-attachments/assets/71f3fff8-15b2-470d-91ab-c6d5f62f708e" />

- example:
```python
print(3 > 4) # False
print(3 < 4) # True
print(3 == 4) # False
print(4 == 4) # True
print(3 != 4) # True
print(3 >= 4) # False
print(3 <= 4) # True
```
- In Python, the most basic conditional is the if statement. Here's the basic syntax:
```python
if condition:
    pass # Code to execute if condition is True
```
  - if statements start with the if keyword.
  - condition is an expression that evaluates to True or False, followed by a colon (:).
  - The body of the if statement constitutes a code block, which is a group of statements that belong together. In Python, the level of indentation is what defines a code block.
- In the example above, the body of the if statement contains a pass statement. When a pass statement is executed, nothing happens. This is a special keyword that can be used as a placeholder for future code and it is useful when empty code blocks are not allowed.
- While other programming languages use characters like curly braces to define code blocks, and just use indentation for readability, in Python, code blocks are determined by indentation.
- The following code would raise an IndentationError, which is Python's way to signal that indentation is required at a certain point of the code:
```python
age = 18

if age >= 18:
print('You are an adult') # IndentationError: expected an indented block after 'if' statement on line 3
```
- Though you can use any number spaces (as long as you are consistent) to determine each level of indentation, the Python style guide recommends using four spaces.
- syntax for if-else:
```python
if condition:
   pass # Code to execute if condition is True
else:
   pass # Code to execute if condition is False
```
- There might be situations in which you want to account for multiple conditions. To do that, Python lets you extend your if statement with the elif (else if) keyword.
```python
if condition1:
   pass # Code to execute if condition1 is True
elif condition2:
   pass # Code to execute if condition1 is False and condition2 is True
else:
   pass # Code to execute if all conditions are False
```
- Note that you can use as many elif clauses as you want.

### What Are Truthy and Falsy Values, and How Do Boolean Operators and Short-Circuiting Work?
- If you are working with more complex conditional statements, then you can use Python’s and, or, and not operators.
- In Python, every value has an inherent boolean value, or a built-in sense of whether it should be treated as True or False in a logical context. Many values are considered truthy, that is, they evaluate to True in a logical context. Others are falsy, meaning they evaluate to False.
- Here are a few falsy values:
  - None
  - False
  - Integer 0
  - Float 0.0
  - Empty strings ""
- Other values like non-zero numbers, and non-empty strings are truthy.
- If you want to check whether a value is truthy or falsy, you can use the built-in bool() function. It explicitly converts a value to its boolean equivalent and returns True for truthy values and False for falsy values.
```python
print(bool(False)) # False
print(bool(0))  # False
print(bool('')) # False

print(bool(True)) # True
print(bool(1)) # True
print(bool('Hello')) # True
```
- There are three Boolean operators in Python: and, or, and not.
- The and operator takes two operands and returns the first operand if it is falsy, otherwise, it returns the second operand. Both operands must be truthy for an expression to result in a truthy value.
```python
is_citizen = True
age = 25

print(is_citizen and age) # 25
```
- In the above example, the number 25 is printed to the terminal because the and operator will evaluate the second operand if the first operand is True. The and operator is known as a short-circuit operator. Short-circuiting means Python checks values from left to right and stops as soon as it determines the final result.
- Now let's take a look at the or operator. This operator returns the first operand if it is truthy, otherwise, it returns the second operand. An or expression results in a truthy value if at least one operand is truthy. The or operator is also known as a short-circuit operator. Here is an example:
```python
age = 19
is_employed = False

print(age or is_employed) # 19
```
- The last operator we will look at is the not operator which takes a single operand and inverts its boolean value. It converts truthy values to False and falsy values to True. Unlike the previous operators we looked at, not always returns True or False.
```python
print(not '') # True, because empty string is falsy
print(not 'Hello') # False, because non-empty string is truthy
print(not 0) # True, because 0 is falsy
print(not 1) # False, because 1 is truthy
print(not False) # True, because False is falsy
print(not True) # False, because True is truthy
```

## Understanding Functions and scope:

### How Do Functions Work in Python?

- Functions are reusable pieces of code that run when you call them. Many programming languages come with built-in functions that make it easier to get started.
- Some of the built-in functions are print(), input() (which lets you prompt the user for input) etc.
```python
name = input('What is your name?') # User types "Kolade" and presses Enter  
print('Hello', name) # Output: Hello Kolade
```
- On the other hand, int() converts a number, boolean, and a numeric string into an integer:
```python
print(int(3.14)) # 3
print(int('42')) # 42
print(int(True)) # 1
print(int(False)) # 0 
```
- You can also write your own custom functions. To do that, you use the def keyword, followed by the name you want to give your function, a pair of parentheses, and a colon. Then on a new line, you write the code your function should run. The code the function runs is also referred to as the function's body.
```python
def hello():
    print('Hello World')
```
- To run the function, you need to call it with its name followed by a pair of parentheses:
```python
hello() # Hello World
```
- Python relies on indentation to determine which groups of statements belong together. These groups of statements are called code blocks.
```python
def calculate_sum(a, b):
    print(a + b)
```
- Here, a and b are called parameters. Think of parameters as placeholder variables that act as "slots" for the values you pass into functions when you call them.
- If you call the function without the correct number of arguments, you'll get a TypeError:
```python
# TypeError: calculate_sum() missing 2 required positional arguments: 'a' and 'b'
```
- Functions also use a special return keyword to exit the function and return a value. If you don't explicitly use return, Python will return None by default.
```python
def calculate_sum(a, b):
    print(a + b)

my_sum = calculate_sum(3, 1) # 4
print(my_sum) # None
```
- Now, calculate_sum returns the sum of a and b, which gets stored in my_sum.
```python
def calculate_sum(a, b):
    return a + b

my_sum = calculate_sum(3, 1)
print(my_sum) # 4
```

### What Is Scope in Python and How Does It Work?
- In Python, scope determines the point at which you can access a variable. It's what controls the lifetime of a variable and how it is resolved in different parts of the code.
- To correctly determine scope, Python follows the LEGB rule, which stands for the following:
  - Local scope (L): Variables defined in functions or classes.
  - Enclosing scope (E): Variables defined in enclosing or nested functions.
  - Global scope (G): Variables defined at the top level of the module or file.
  - Built-in scope (B): Reserved names in Python for predefined functions, modules, keywords, and objects.

- Python uses the LEGB rule to resolve the scope of the variables in your program. We'll dive into each of these rules so you get a better understanding of the process.
-  Local scope means that a variable declared inside a function or class can only be accessed within that function or class.
```python
def my_func():
    my_var = 10 # Locally scoped to my_func
    print(my_var)

my_func() # 10

print(my_var) # NameError: name 'my_var' is not defined
```
- Enclosing scope means that a function that's nested inside another function can access the variables of the function it's nested within.
-  However, note that outer functions cannot access variables defined within any nested functions:
```python
def outer_func():
    msg = 'Hello there!'
    print(res)

    def inner_func():
        res = 'How are you?'
        print(msg)

    inner_func()

outer_func() # NameError: name 'res' is not defined
```
-  One solution is to initialize res as an empty string in the enclosing scope, which is within outer_func. Then within inner_func, make res a non-local variable with the nonlocal keyword:
```python
def outer_func():
    msg = 'Hello there!'
    res = ""  # Declare res in the enclosing scope

    def inner_func():
        nonlocal res  # Allow modification of an enclosing variable
        res = 'How are you?'
        print(msg)  # Accessing msg from outer_func()

    inner_func()
    print(res)  # Now res is accessible and modified

outer_func()

# Output:
# Hello there!
# How are you?
```
- Global scope refers to variables that are declared outside any functions or classes which can be accessed from anywhere in the program.
```python
my_var = 100

def show_var():
    print(my_var)

show_var() # 100
print(my_var) # 100
```
- And if you want to make a locally scoped variable defined inside a function globally accessible, you can use the global keyword:
```python
my_var_1 = 7

def show_vars():
    global my_var_2
    my_var_2 = 10
    print(my_var_1)
    print(my_var_2)

show_vars() # 7 10

# my_var_2 is now a global variable and can be accessed anywhere in the program
print(my_var_2) # 10
```
- You can also use the global keyword to modify a global variable:
```python
my_var = 10  # A global variable

def change_var():
    global my_var  # Allows modification of a global variable
    my_var = 20

change_var()

print(my_var)  # my_var is now modified globally to 20
```
- Finally, built-in scope refers to all of Python's built-in functions, modules, and keywords, and are available anywhere in your program:
```python
print(str(45)) # '45'
print(type(3.14)) # <class 'float'>
print(isinstance(3, str)) # False
```
-
