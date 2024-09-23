# Scriptal: Text Processing Language

### Table of Contents

1. [ Introduction. ](#introduction)
2. [ Syntax. ](#syntax)
3. [ Commands. ](#commands)
   1. [ Parameters. ](#parameters)
   2. [ Multiple Parameters. ](#multiple-parameters)
   3. [ Connectors. ](#connectors)

4. [ Internal Commands. ](#internal-commands)
5. [ Compound Commands. ](#compound-commands)
6. [ Comments. ](#comments)
7. [ Variables. ](#variables)
8. [ Data Types. ](#data-types)
   1. [ Getting Data Types. ](#getting-data-types)
   2. [ Strings. ](#strings)
   3. [ Numbers. ](#numbers)
   4. [ Arrays. ](#arrays)
   5. [ Booleans. ](#booleans)
   6. [ Patterns. ](#patterns)
9. [ Connector AS. ](#connector-as)
10. [ Operators. ](#operators)
    1. [ Arithmetic. ](#arithmetic-operators)
    2. [ Comparison. ](#comparison-operators)
    3. [ Logical. ](#logical-operators)
    4. [ Membership. ](#membership-operators)
    5. [ Data Combination. ](#data-combination)
    6. [ Special Operators vs Connectors. ](#special-operators-vs-connectors)
11. [ Conditions. ](#conditions)
12. [ Keywords. ](#keywords)
13. [ Data Comparison. ](#data-comparison)
14. [ Text Processing. ](#text-processing)
15. [ Events. ](#events)
16. [ Environment Variables. ](#environment-variables)
17. [ Loops. ](#loops)
18. [ Defining Commands. ](#defining-commands)
19. [ Error Handling. ](#error-handling)
20. [ Modules. ](#modules)
21. [ Libraries. ](#libraries)
22. [ Conventions. ](#conventions)
23. [ Additional Resources. ](#additional-resources)


<br>

## Introduction

Scriptal is a lightweight language specifically designed for text processing, allowing users to modify the structure of text as needed to streamline processes.

It was created to facilitate learning programming languages, based on English structure and using more common words, such as `ROUND_UP` instead of `CEIL`.

### Scriptal Syntax Compared to Other Programming Languages

Scriptal was designed to be easy to read and has some similarities to the English language. Here are some of the differences and similarities between Scriptal and other programming languages:

1. **New Lines Instead of Semicolons**

    Scriptal uses new lines to complete a command, unlike other programming languages that typically use semicolons or parentheses. This feature makes the code cleaner and more readable.

2. **Indentation to Define Scope**

    Scriptal relies on indentation using whitespace to define the scope of code blocks like loops, functions, and conditions. Other programming languages often use curly braces {} for this purpose.

<br>

## Syntax

Using Scriptal is very simple once you understand its rules and how its syntax works. Here are some of the most important rules:

### Command Sequences

A Scriptal file is a sequence of commands or instructions. It is not possible to perform actions such as mathematical operations or variable assignments freely; you must always use a specific command for these tasks. Therefore, each line must start with a command.

### One Line: One Command

As a convention, it is recommended to use one command per line. If you need to use multiple commands, separate them into different lines. This keeps the code clearer and easier to understand. Although in some cases you will need to use internal commands, always try to use one command per line when possible.

<br>

## Commands

A "command" is an instruction that is written at the beginning of the line and performs a specific action in the context of text processing. Each command is designed to be clear and direct, using uppercase syntax to indicate the operation to be executed. Commands can modify, analyze, transform, and manipulate text, as well as perform logical and flow control operations.

### Parameters

In Scriptal, commands can receive additional information known as parameters. Unlike other languages, parentheses are not used to pass parameters; they are written directly after the command.

```bash
# Pseudocode!
COMMAND PARAMETER
```

<br>

For example, the `PRINT` command displays a message on the console. It needs a parameter to function:

```bash
PRINT "I have written something for you"
```

<br>

In Scriptal, it is not possible to use multiple commands independently on the same line. For example, the following syntax is incorrect:

```bash
PRINT "Hello" PRINT "World"     # ERROR! You are actually passing 3 parameters.
```

<br>

Instead, you should write each command on a separate line:

```bash
PRINT "Hello"
PRINT "World"
```

<br>

### Multiple Parameters

Unlike other programming languages, Scriptal does not use commas to separate parameters. Instead, parameters are separated by data types or instances. Each string, number, mathematical operation, array, etc., is considered a separate instance.

```bash
# Pseudocode!
COMMAND "Parameter #1" "Parameter #2" "Parameter #3"
```
<br>

Spaces between parameters do not always indicate separation. For example, in mathematical operations, spaces may be present and the set will still be considered a single parameter:

```bash
# Pseudocode!
COMMAND "Parameter #1" 32.4564 + 10.2103 "Parameter #3"
```

**Parameters Obtained:**

* "Parameter #1"
* 42.6667
* "Parameter #3 "

> [!NOTE]  
> To avoid confusion, connectors are used.

<br>

### Connectors

To avoid confusion and improve code readability, commands in Scriptal use keywords called **connectors**. These keywords, always in uppercase, separate parameters from each other and are also considered parameters. Some connectors are mandatory in certain commands.

<br>

**List of Connectors**:

`AND`, `AS`, `AT`, `BETWEEN`, `BY`, `FROM`, `IN`, `IS`, `INTO`, `OR`, `TO`, `WITH`.

<br>

For example, to split a text, we can use the `SPLIT` command, which requires three parameters: the text, the `BY` connector, and the separator.

```bash
SPLIT "Hello World from Scriptal" BY " " # Result: ["Hello", "World", "from", "Scriptal"]
```

In this case, `BY` is a mandatory connector that also counts as a parameter. The connector ensures that the code is clearer and easier to read, clearly specifying the relationship between the different parameters of the command.

<br>

## Internal Commands

In some situations, it is necessary to use one command within another. These commands are known as internal commands, as they provide information to the main command. For an internal command to be valid, it must return a value that the external command can use. This value cannot be `NULL`.

To use an internal command, you must call it from another command and close its opening with a semicolon (;).

<br>

**Example of Internal Command**

If you want to convert a text to uppercase and then print it, you can use `UPPERCASE` as an internal command within `PRINT`:

```bash
# Pseudocode!
PRINT UPPERCASE "lower text";
```

<br>

In this case, `UPPERCASE` is an internal command that returns a value which `PRINT` uses to display the text in uppercase.

<br>

**IMPORTANT**

We recommend not using more than one internal command per line. This helps keep the code clear and avoids confusion in command execution.

However, if you need to use more than one internal command on a line, you should encapsulate the internal commands within parentheses so that Scriptal interprets them correctly.

```bash
PRINT MATCH (PATTERN "\d+";) IN $text;
PRINT (UPPERCASE (REVERSE "Hello World";);)
```

Again, **AVOID** codes like that.

<br>

> [!NOTE]  
> As an exception, only the `RETURN` command can use internal commands that return `NULL`.

## Compound Commands

In Scriptal, compound commands allow nesting code within them to handle control structures such as conditions, loops, and definitions. These commands determine when and how the nested code will be executed.

### Code Blocks and Indentation

Indentation is crucial in Scriptal for defining code blocks. Unlike other languages where indentation is mainly used to enhance readability, in Scriptal it is used to structure the code and define the hierarchy of the blocks.

**Example of Pseudocode**
```bash
PARENT
    CHILD
        GRANDCHILD
```

Indentation must be a multiple of 4 spaces.

<br>

If a code block within a compound command is empty, an indentation error will occur. To avoid this issue, use the `SKIP` command, which functions similarly to `pass` in Python.


```bash
WHEN TRUE
    SKIP
```

<br>

## Comments

Comments are used to explain the code, make it more readable, or simply to annotate sections of the code. Comments start with a `#` and are ignored by Scriptal during execution.


```bash
# This is a comment
PRINT "Hello World"
```

<br>

Comments can also be placed at the end of a line, and Scriptal will ignore the rest of the line:

```bash
PRINT "Hello World"     # This is a comment
```

<br>

### Multi-line Comments

For multi-line comments, enclose them between ``###``:

```coffeescript
###
    Yes!
    I am a multi-line
    comment
###
```

<br>


## Variables

In Scriptal, variables are containers for storing data values. Each variable must be preceded by a `$` sign, similar to how it's done in PHP. For example: `$name`.

A variable can have a short name (like x or y) or a more descriptive name (like age, car_name, total_volume). The rules for variable names in Scriptal are:

- A variable name must start with a letter or the underscore (_) character.
- A variable name cannot start with a number.
- A variable name can only contain alphanumeric characters and underscores (A-Z, 0-9, and _).
- Variable names are case-sensitive (age, Age, and AGE are three different variables).

<br>


### Declare or Modify Variables

To declare or modify a variable, the `SET` command is used. Unlike other programming languages, in Scriptal, you can only perform these tasks using commands.

**Syntax**
```
SET {variable} AS {value}
```

**Examples**

```bash
SET $name AS "Martin"
SET $age AS 22

PRINT $age      # 22
PRINT $name     # Martin
```

<br>

It's not necessary to declare variables with any specific type, and they can even change type after they have been set.

```bash
SET $x AS 4
SET $x AS "Mike"
```

<br>

### Variable Scope

In Scriptal, the scope of a variable determines where it can be accessed within the code. There are two main types of scope:

- **Local Scope**: When a variable is declared within a code block or function, its scope is limited to that block or function. These variables are not available outside of their specific context.

    ```bash
    WHEN 34
        SET $name AS "John" # Declare a variable within a block

    PRINT $name # Variable not defined
    ```
<br>


- **Global Scope**: If a variable is declared outside of any code block or function, within an event, or if no specific scope is established, the variable has global scope and can be accessed from anywhere in the code. To declare a global variable from within a block or function, the `GLOBAL_SET` command is used.

    ```bash
    WHEN 34
        GLOBAL_SET $name AS "John" # Declare a global variable
    
    PRINT $name # John
    ```

<br>

## Data Types

Variables can store different types of data. Each data type has distinct characteristics and behaviors.

Scriptal includes the following built-in data types:

- **`STRING`**: For storing text.
- **`NUMBER`**: For storing numeric values.
- **`ARRAY`**: For storing sequences of elements.
- **`MAP`**: For storing key-value pairs.
- **`BOOL`**: For boolean values (true or false).
- **`NULL`**: For representing the absence of a value.
- **`PATTERN`**: For text patterns or regular expressions.

<br>

### Getting Data Types

You can obtain the data type of any object using the `TYPE` command.

```bash
SET $x AS 32
PRINT TYPE $x;          # Will return: "NUMBER"
PRINT TYPE "Hello";     # Will return: "STRING"
```

<br>

### Strings

Text strings are enclosed in either single (`'`) or double (`"`) quotes. Both types of quotes are considered equivalent, and you can use whichever you prefer.

`"hello"` is the same as `'hello'`

```bash
PRINT "I'm a string"
PRINT 'Single quotes are also used'
```

<br>

#### Special Characters
Within strings, you can include special characters like `\n` for new lines or `\t` for tabs.

**Example**

```bash
PRINT "Line #1\nLine #2 \u0052 \tTEXT"
```

<br>

#### Raw Strings
To use a string in its raw form, meaning without interpreting special characters, use the `!` sign before the string. This is useful when you want to prevent escape sequences and other special characters from being processed.

**Example**

```bash
PRINT !"Line #1\nLine #2 \u0052 \tTEXT"
```

In this example, the string will be printed exactly as it is written, including the special characters in their literal form.

<br>

#### Conversion to String

You can convert other data types to strings using the `STRING` command:

```bash
PRINT STRING 256;   # "256"
PRINT STRING TRUE;   # "TRUE"
```

<br>


### Numbers

Numbers can be either integers or floats. Both types of numbers are handled in the same way.

**Example**

```bash
PRINT 18272616
PRINT 1289.232
```

<br>

#### Scientific Notation

You can use scientific notation to represent very large or very small numbers with `e` o `E`.

**Example**

```bash
PRINT 1.23e4		# This is the same as: 1.23 * 10^4
PRINT 4.56E-3		# This is the same as: 4.56 * 10^-3
```

<br>

#### Infinity and NAN

Scriptal recognizes the values of `INFINITY` and `NAN` (Not a Number). You can use them directly in your calculations.

**Example**

```bash
PRINT 1e308 * 10               # Positive infinity
PRINT -1e308 * 10              # Negative infinity
PRINT INFINITY - INFINITY      # NAN
PRINT -(INFINITY)              # Negative infinity
```

<br>

#### Conversion to Numbers

You can convert other data types to numbers using the `NUMBER` and `FLOAT` commands. These conversions are useful when you need to perform mathematical operations or process numerical data.

<br>

**Conversion to Integer**

The `NUMBER` command converts a value to an integer. If the value cannot be converted, an error will be thrown.

```bash
PRINT NUMBER "56";  # 56
PRINT NUMBER 123;  # 123
PRINT NUMBER 10.5;  # 10
```

<br>

**Conversion to Float**

The `FLOAT` command converts a value to a float. If the value cannot be converted, an error will be thrown.

```bash
PRINT FLOAT "56.25";  # 56.25
PRINT NUMBER 10.5;  # 10.5
PRINT NUMBER "123";  # 123.0
```

<br>

### Arrays

Arrays are used to store multiple elements in a single variable. You can easily create and manipulate arrays using brackets to define them.

#### Creating an Array

Arrays are created using brackets (`[` and `]`).

**Example**

```bash
SET $list AS ["Apple", "Pear", "Banana"]
```

<br>

#### Array Elements

- **Ordered**: Array elements have a specific order.
- **Modifiable**: You can change array elements after creating them.
- **Duplicates Allowed**: Arrays can contain duplicate values.

<br>

#### Accessing Elements

Array elements are indexed, starting from 1. You can access elements using a dot (`.`) followed by the index after the variable name.

```bash
SET $fruits AS ["Grape", "Apple", "Strawberry"]
PRINT $fruits.1 # Grape
```

<br>

You can also use the `GET` command to obtain an index dynamically:
```bash
SET $fruits AS ["Grape", "Apple", "Strawberry"]
PRINT GET 2 IN $fruits; # Apple
```

<br>

> [!IMPORTANT]  
> In Scriptal, all indices start from 1, not from 0. It is important to keep this in mind when accessing array elements.

<br>

#### Modifying Array Elements

You can modify a specific element of an array using its index.

**Example**

```bash
SET $numbers AS [10, 20, 30]
SET $numbers.2 AS 25
PRINT $numbers.2  # 25
```

<br>

You can also modify an element using the `PUT` command:
```bash
SET $numbers AS [10, 20, 30]
PUT "Hello!" AT 3 IN $numbers  # [10, 20, "Hello!"]
```

<br>

#### Adding and Removing Elements

Scriptal also allows you to add and remove elements from an array using specific commands.

**Adding an Element:**

You can use commands like `APPEND`, `PUT`, or `INSERT` for this purpose:

```bash
APPEND "Orange" TO $fruits
PUT "Orange" IN $fruits
```

<br>

**Removing an Element:**

```bash
DELETE "Orange" IN $fruits    # Remove element by value
REMOVE 1 IN $fruits           # Remove element by index
```

<br>

#### ARRAY Structure

There is another way to create arrays, using the `ARRAY_SET` command and the `ELEM` command:

```bash
ARRAY_SET $fruits
    ELEM "Grape"
    ELEM "Apple"
    ELEM "Strawberry"

PRINT $fruits
```

<br>

Check the complete list of array commands [here](Commands_EN.md#arrays).

<br>


### Maps

Maps are used to store key-value pairs, allowing for an ordered and modifiable data structure without duplicates. Maps are a fundamental tool for organizing and managing information efficiently.

<br>

#### Creating a Map

You can initialize a map with default values using the `{}` structure:

```bash
SET $my_map AS {"name": "Martin", "age": 22}
```

<br>

#### Accessing Elements

To access the values of a map, use the map variable followed by the key name, separated by a dot:

```bash
PRINT $my_map.nombre     # Martin
```
<br>

If you need to access the value of a key using a dynamic name, you can use the `MAP_KEY_GET` or `GET` command:

```bash
PRINT MAP_KEY_GET "name" IN $my_map;   # Martin
PRINT GET "name" IN $my_map;   # Martin
```

The difference between `GET` and `MAP_KEY_GET` is that if the key does not exist, `GET` will return `NULL` instead of throwing an error.

<br>


#### MAP Structure
You can also create or manipulate a map using the `MAP_SET` command:

```bash
MAP_SET $my_map
    SKIP
```

<br>

Keys of a map can be defined or modified using the `KEY` command (which is very similar to `SET`) within the block:

```bash
MAP_SET $my_map
    KEY "name" AS "Martin"
    KEY "age" AS 22
```

<br>

#### Removing Keys and Values

To remove a key and its associated value from a map, use the `DELETE` command:

```bash
DELETE "age" IN $my_map
```

<br>

Check the complete list of map commands [here](Commands_EN.md#maps).


<br>

### Booleans

Boolean values are fundamental for control logic and conditional evaluations. Boolean values can only be `TRUE` or `FALSE`.

#### Creating a Boolean

You can assign boolean values to variables using the `SET` command:

```bash
SET $is_true AS TRUE
SET $is_false AS FALSE
```

<br>

#### Conversion to Booleans

You can convert other data types to booleans using the `BOOL` command. This conversion is useful for evaluating conditions based on different data types. The `BOOL` command interprets values as follows:

- Any value that is not `FALSE`, `0`, an empty string (`""`), or an empty array/map is considered `TRUE`.
- Values that are `FALSE`, `0`, an empty string (`""`), or empty arrays/maps are considered `FALSE`.

<br>

```bash
SET $number AS 10
BOOL $number AS $is_true
PRINT $is_true    # TRUE

SET $zero AS 0
BOOL $zero AS $is_false
PRINT $is_false    # FALSE
```

<br>

### Patterns

Patterns (PATTERN) allow you to work with regular expressions and perform advanced text processing operations. Below are the key aspects of patterns, including their definition, use of flags, and related commands.

#### Definition of Patterns

To create a pattern in Scriptal, use the `PATTERN` command followed by a regular expression as a string. Patterns enable text searches and manipulations based on regular expressions.

**Syntax:**

```bash
PATTERN "regular_expression"
```

<br>

**Example:**

```bash
PATTERN "^[A-Za-z]+$" # A pattern that matches strings containing only letters
```

<br>

#### Using Flags

Flags modify the behavior of regular expressions. They are specified using `PATTERN` command with an array of flags.

**Syntax:**

```bash
PATTERN "regular_expression" WITH [array_of_flags]
```

<br>

**Available Flags:**

- `i` or `IGNORECASE` - Ignores case distinctions.
- `m` or `MULTILINE` - `^` and `$` match the start and end of each line.
- `s` or `DOTALL` - `.` matches all characters, including newlines.
- `a` or `ASCII` -  Matches only ASCII characters.
- `l` or `LOCALE` - Based on the system locale (less common).
- `u` or `UNICODE` - Allows metacharacters to match Unicode characters, rather than just ASCII characters. This is the default setting and does not need to be specified.

<br>

**Example:**

```bash
PATTERN "michael" WITH ["i"]    # Matches michael, Michael, MICHAEL, etc.
PATTERN "^\d{3}-\d{2}-\d{4}$" WITH ["MULTILINE"]
```

<br>

#### Using Patterns in Commands

Patterns can be used in various functions to perform different operations on text:

- **MATCH**: Searches for a match in the text.
- **MATCH_FIRST**: Finds the first match in the text.
- **MATCH_ALL**: Finds all matches in the text.
- **MATCH_FULL**: Searches for a full match in the text.

<br>

**Example:**

```bash
PATTERN "^Hello" AS $pattern # Defines a pattern to search for strings starting with "Hello"
MATCH $pattern IN "Hello world" AS $result # Searches for a match in the text
PRINT $result
```

<br>

#### Using Raw Strings

To prevent certain special characters like `\n` from being interpreted in patterns, you can use raw strings:

```bash
PATTERN !"\d{2}\s\w+" # A pattern to search for two digits followed by a space and a word, without interpreting special characters
```

<br>


#### Related Commands

Several commands accept both text strings and patterns to perform different operations. These commands are useful for searching, replacing, and manipulating text based on regular expressions. They include: `COUNT`, `SPLIT`, `FIND`, `REPLACE`, etc.

<br>

For more details, you can review the complete list of pattern-related commands [here](Commands_EN.md#patterns-and-regular-expressions).

<br>

## Connector `AS`

The `AS` connector is one of the most commonly used and important connectors. It is generally used to define variables, for example:

```bash
SET $age AS 32
```

<br>

Additionally, it can be optionally used in most commands to store the result in a variable:

```bash
UPPERCASE "Hello World" AS $line
PRINT $line
```

This way, you avoid using [ internal commands ](#internal-commands).

<br>

**IMPORTANT**

Keep in mind that when using this connector to define or modify a variable, the command will not return anything:

```bash
UPPERCASE "Hello"                   # Returns "HOLA"
UPPERCASE "Hello" AS $text          # Returns nothing

# Therefore...
PRINT UPPERCASE "Hello";            # Prints "HOLA"
PRINT UPPERCASE "Hello" AS $text;   # Error: Internal commands must return something.
```

<br>


### Abbreviation

If you prefer, you can use a colon (`:`) as an abbreviation for `AS`:

```bash
# You can use both...
SET $name AS "Martin" 
UPPERCASE $name AS $upper

# Or...
SET $name : "Martin" 
UPPERCASE $name : $upper
```
<br>

However, we recommend using the abbreviation only when the `AS` connector is mandatory. Otherwise, it is recommended to keep it in its original form.

```bash
# The connector is mandatory in commands like "SET", "KEY", etc. Here, we can abbreviate it.
SET $name : "Martin"
MAP_SET $map
    KEY "name" : "Martin"
    

# In commands like "UPPERCASE" and "SPLIT", the connector is optional, so we keep it in its original form
UPPERCASE $name AS $upper
SPLIT "hello world" BY " " AS $words
```
<br>

## Operators

Scriptal supports various types of operators that allow you to perform arithmetic operations, comparisons, and logical operations. These operators are essential for handling and manipulating data in your code.

### Arithmetic Operators

These operators are used to perform basic mathematical operations:

- **Addition (`+`):** Adds two values.
- **Subtraction (`-`):** Subtracts one value from another.
- **Multiplication (`*`):** Multiplies two values.
- **Division (`/`):** Divides one value by another.
- **Modulo (`%`):** Returns the remainder of the division of two values.
- **Exponentiation (`**` or `^`):** Raises a number to the power of another.

<br>

**Example:**

```bash
PRINT 10 + 5    # 15
PRINT 10 - 5    # 5
PRINT 10 * 5    # 50
PRINT 10 / 5    # 2.0
PRINT 10 % 3    # 1
PRINT 2 ** 3    # 8
PRINT 2 ^ 3     # 8
```

<br>

### Comparison Operators

These operators are used to compare two values and return a boolean value (`TRUE` or `FALSE`):

- **`IS` / `==`** - Returns `TRUE` if both values are equal.
- **`IS NOT` / `!=`** - Returns `TRUE` if the values are not equal.
- **`IS GT` / `>`** - Returns `TRUE` if the first value is greater than the second.
- **`IS LT` / `<`** - Returns `TRUE` if the first value is less than the second.
- **`IS GEQ` / `>=`** - Returns `TRUE` if the first value is greater than or equal to the second.
- **`IS LEQ` / `<=`** - Returns `TRUE` if the first value is less than or equal to the second.


<br>

**Example:**

```bash
PRINT 32 IS 32		# TRUE
PRINT 32 IS NOT 45	# TRUE
PRINT 40 IS LT 60 	# FALSE
PRINT $var == $name
```

<br>

### Logical Operators

These operators are used to combine multiple boolean expressions:

- **`AND` (`&&`)** - Returns `TRUE` if both expressions are true.
- **`OR` (`||`)** - Returns `TRUE` if at least one of the expressions is true.
- **`NOT`** - Inverts the value of a boolean expression.

<br>

**Example:**

```bash
PRINT $n > 10 AND $n < 45
PRINT TRUE OR FALSE
PRINT NOT TRUE == TRUE
```

<br>

### Membership Operators

A membership operator is used to identify membership in a sequence (arrays, strings, maps).

- **`IN` (`~~`)** - Returns `TRUE` if the specified value is found in the sequence.
- **`NOT IN` (`!~~`)** - Returns `TRUE` if the specified value is not found in the sequence.
- **`HAS` (`::`)** - Returns `TRUE` if the sequence contains the specified value.
- **`HAS NOT` (`!::`)** - Returns `TRUE` if the sequence does not contain the specified value.

<br>

The difference between `IN` and `HAS` lies in the order of the elements, following the structure of English:

- {element} `IN` {sequence}: Indicates that the element is within the sequence.
- {sequence} `HAS` {element}: Indicates that the sequence contains the element.

**Example:**

```bash
PRINT 32 IN [45, 67, 88]	# FALSE
PRINT "X" IN "John Xavier"	# TRUE
PRINT $fruits HAS "Orange"	# TRUE
```

<br>

### Data Combination

Scriptal allows great flexibility in data combination. You can sum or combine different types of data, such as strings, arrays, and maps.

```bash
PRINT "Hello " + "World"      # Hello World
PRINT [1, 2] + [3, 4]         # [1, 2, 3, 4]
PRINT {"a": 1} + {"b": 2}     # {"a": 1, "b": 2}
```

<br>

### Special Operators vs Connectors

In Scriptal, operators and connectors have specific uses depending on the context in which they are employed. The rules for their usage are detailed below:

#### Special Operators

Logical and comparison operators (`IS`, `NOT`, `IN`, `AND`, `OR`, etc.) are used only in boolean and conditional contexts, such as `WHEN`, `ORWHEN`, `WHILE`, `SET`, `PRINT`, and `BOOL`.

These operators allow you to combine different elements or perform advanced comparisons. In non-boolean or non-conditional contexts, they are considered [**connectors**](#connectors).

<br>

#### Special Operators as Connectors

In a non-boolean or non-conditional context, special operators will be interpreted as connectors rather than operators.

<br>

**Boolean Contexts**

`SET` and `PRINT` allow for a boolean context. This means that special operators can be used with them.

```bash
SET $is_equal AS 32 IS 32     # TRUE
PRINT 45 IS GT 36 AND 48 IS   # TRUE
```

<br>

**Conditional Contexts**

`WHEN` provides a conditional context, allowing the use of special operators:

```bash
WHEN 32 IS NOT 45
    PRINT "It's not"
```

<br>


**Normal Contexts**

`FIND`, on the other hand, does not provide any special context. Therefore, special operators will be considered connectors:

```bash
FIND "hello" IN "hello world"
```

<br>

#### Alternatives

If for some reason you need to use special operators in non-boolean or non-conditional contexts, you can use the equivalent symbols. For example, the `PICK` command takes 3 parameters: option 1, `OR`, and option 2:

`PICK {option1} OR {option2}`

If you want to use the special operator `IN` to check if an element is within an array, you can use the equivalent symbol, i.e., `~~`:

**Example**

```bash
# Use:
PICK 32 ~~ [32, 45] OR FALSE 

# Instead of:
# PICK 32 IN [32, 45] OR FALSE
```

<br>

## Conditions

In Scriptal, conditions are handled using the `WHEN`, `ORWHEN`, and `ELSE` commands. These commands allow you to control the flow of the program based on specific conditions.

### WHEN

The `WHEN` command (known as "if" in other languages) is used to evaluate a condition. If the condition is met, the associated block of code is executed.

**Syntax**
```
WHEN {condition}
    {code}
```

<br>

**Example**
```bash
WHEN $age >= 18
    PRINT "You are of legal age."
```

<br>

### ORWHEN

The `ORWHEN` command is used to evaluate an alternative condition if the initial `WHEN` condition is not met. You can use multiple `ORWHEN` statements after a `WHEN`.

**Syntax**
```
ORWHEN {condition}
    {code}
```

<br>

**Example**
```bash
WHEN $age > 18
    PRINT "You are over 18."
ORWHEN $age == 18
    PRINT "You are exactly 18 years old."
ORWHEN $age > 12
    PRINT "You are a teenager."
```

<br>

### ELSE

The `ELSE` command is used to define a block of code that will be executed if none of the previous `WHEN` or `ORWHEN` conditions are met.

**Syntax**
```
ELSE
    {code}
```

<br>

**Example**
```bash
WHEN $age > 18
    PRINT "Eres mayor de edad."
ORWHEN $age == 18
    PRINT "Tienes justo 18 años."
ELSE
    PRINT "Eres menor de edad."
```

<br>

### AND and OR Commands

In addition to being connectors and operators, `AND` and `OR` also function as commands. They can be used under `WHEN` and `ORWHEN` commands:

```bash
# This:
WHEN $num % 2 == 0
AND $num > 20
    PRINT "The number is even and greater than 20."

# Is exactly the same as this:
WHEN $num % 2 == 0 AND $num > 20
    PRINT "The number is even and greater than 20."
```

<br>

### Interpretation as Booleans

Scriptal interprets other data types as booleans. Any value that is not empty, `FALSE`, or `0` is considered `TRUE`.

```bash
SET $numero AS 10

WHEN $numero
    PRINT "The number is true."    # This will be printed because 10 is considered TRUE

SET $texto AS ""

WHEN $texto
    PRINT "The text is empty."    # This will not be printed because an empty string is considered FALSE
```

<br>

## Keywords

Keywords are predefined constant values that represent specific states or data types. These keywords have intrinsic values used to check or set conditions in the code. Below are the main keywords and their intrinsic values:

**General:**

- `TRUE` - Represents the boolean value true.
- `FALSE` - Represents the boolean value false.
- `NULL` - Represents a null value.
- `NAN` - Represents a non-numeric value.
- `INFINITY` - Represents an infinite number.

<br>

**Special:**

- `EMPTY` - Represents an empty or null value. Returns `NULL`.
- `STRING` - Represents a string value. Used to check or initialize string-type variables. Returns `""` (empty string).
- `NUMBER` - Represents a default integer numeric value. Used to check or initialize numeric variables. Returns `0` (zero).
- `ARRAY` - Represents a collection. Used to initialize or check arrays. Returns `[]` (empty array).
- `MAP` - Represents a map. Used to initialize or check maps. Returns `{}` (empty map).
- `BOOL` - Represents a boolean value. Returns `FALSE`.

<br>

**Text:**

- `DOT` - Represents a period (`.`).
- `SPACE` - Represents a whitespace character (` `).
- `COMMA` - Represents a comma (`,`).
- `HASH` - Represents a hash symbol (`#`).
- `TAB` - Represents a tab character (`\t`).
- `NEWLINE` - Represents a newline character (`\n`).
- `ELLIPSIS` - Represents ellipsis (`...`).

<br>

**Example**

```bash
SET $var_str AS STRING
SET $var_num AS NUMBER

SPLIT $document BY NEWLINE AS $result
```

<br>

## Data Comparison

You can compare data using a series of specific commands designed to check the type of a value. These commands are useful to ensure that the data meets certain criteria before processing.

### Comparison Commands

- `IS_STRING` - Checks if a value is a string.
- `IS_NUMBER` - Checks if a value is a number (includes both integers and floats).
- `IS_FLOAT` - Checks if a value is a floating-point number.
- `IS_INTEGER` - Checks if a value is an integer.
- `IS_ARRAY` - Checks if a value is an array.
- `IS_MAP` - Checks if a value is a map.
- `IS_PATTERN` - Checks if a value is a pattern or regular expression.
- `IS_BOOL` - Checks if a value is a boolean (`TRUE` or `FALSE`).
- `IS_NULL` - Checks if a value is `NULL`.

<br>

**Other Commands:**

- `IS_EMPTY` - Checks if a value is empty.
- `IS_NAN` - Checks if a value is NAN.
- `IS_INFINITY` - Checks if a number is infinite.
- `IS_EVEN` - Checks if a number is even.
- `IS_ODD` - Checks if a number is odd.
- `IS_DATE` - Checks if a value is a date.


<br>

### Comparison with Keywords

You can use [comparison operators](#comparison-operators) `IS` or `IS NOT` with special keywords within conditionals or boolean contexts to check data types and make decisions based on the result.

**General Syntax:**
```
{value} IS {special-keyword}
{value} IS NOT {special-keyword}
```

<br>

**Example**

```bash
WHEN 32 IS NUMBER
    PRINT "The value is a number"

WHEN "hola" IS STRING
    PRINT "The value is a string"

WHEN $text IS NOT EMPTY
    PRINT "The string is not empty."

# "PRINT" and "SET" provide a boolean context, so they can use comparison operators.
SET $name AS "John"
PRINT $name IS STRING     # TRUE

SET $valid AS $name IS NOT NUMBER
PRINT $valid             # TRUE
```
<br>

> [!CAUTION]
> When terms like `STRING`, `NUMBER`, `ARRAY`, etc., are used, they are not referring to commands for converting data. Instead, these refer to **keywords** that are used to describe the expected data type.



```bash
PRINT NUMBER "23";      # We use the "NUMBER" command to convert the value, returns 23.
PRINT 23 IS NUMBER      # We use the "NUMBER" keyword to compare, returns TRUE.
```

<br>

For an element to be considered an internal command, it must include a semicolon `;` at the end or the line must start with it. In contrast, keywords are used directly in comparison expressions to check the data type.

<br>

**NOTE**

Although `EMPTY` and `NULL` have the same value, the comparison between them is quite different. Both keywords function like `IS_EMPTY` and `IS_NULL` respectively:

```bash
PRINT IS_EMPTY [];  # TRUE
PRINT IS_NULL [];   # FALSE

PRINT [] IS EMPTY  # TRUE
PRINT [] IS NULL   # FALSE
```


<br>

### String Comparison Commands

There are also commands to check if strings meet certain criteria, including:

`IS_EMAIL`, `IS_URL`, `IS_DIGIT`, `IS_NUMERIC`, `IS_ALPHA`, `IS_ALPHANUMERIC`.

Check the complete list of string comparison commands [here](Commands_EN.md#string-comparison).

<br>

**Example**

```bash
WHEN IS_EMAIL "johnsmith@gmail.com";
    PRINT "Valid Email"

WHEN NOT IS_URL "323232";
    PRINT "Invalid URL"

WHEN IS_ALPHANUMERIC "helloworld";
    PRINT "Only alphanumeric!"
```

<br>

## Text Processing

The main purpose of Scriptal is text processing, whether from a direct string or from a file. Text processing begins by loading the content you wish to manipulate.

### Load Content

To start working with text in Scriptal, we use the `INPUT` command, which allows you to load content from various sources:

- **From a String:**
```bash
INPUT "Hello World"
```

<br>

- **From a File:**
```bash
INPUT FROM "file.txt"
```

<br>

In short, `INPUT` allows us to read the content from a file or a string and provides certain functions to process this information.

Once the content is loaded, additional features are enabled to facilitate text processing. These features include:

- [ **Events** ](#events)
- [ **Environment Variables** ](#environment-variables)

<br>

> [!IMPORTANT]
> Content can only be loaded once per file.

<br>


### Process and Store

You can save fragments of the processed text and export them to a new file. While you can use commands like `FILE_WRITE` to handle files, a simple way is with the `STORE` command. This command saves text strings as new lines, which you can export or process later as needed.

```bash
STORE "Hello World"
STORE UPPERCASE "Como";  # Stores the string "COMO"
STORE REVERSE "ESTAS";   # Stores the string "SATSE"
```

<br>

### Export

To export the lines you have previously stored, use the `OUTPUT` command. This command combines all the saved lines and writes them to a single file.

```bash
OUTPUT "file.txt"
```

<br>

If you need to clear all the stored lines, use the `CLEAR_STORAGE` command.

<br>

**Additional Notes:**

The `OUTPUT` command is a convenient and general way to export stored content, but it's not the only option. You can also use other file manipulation commands such as `FILE_WRITE`, `JSON_SAVE`, etc., to perform similar operations.

<br>

## Events

In Scriptal, events allow you to execute commands at specific moments during the content loading and analysis process. Events help organize the execution flow and perform tasks at different stages of processing.

### Main Events

1. `START` - Triggered at the beginning of the process. It is useful for declaring variables, setting up the environment, or making initial preparations.
2. `END` - Triggered at the end of the process. It is used for final tasks, such as final adjustments or releasing resources.
3. `GLOBAL` - The **default event**. Any code block not specifically assigned to another event will be executed here.

<br>

### Line Events

1. `EACH_LINE` - Triggered for each line of the loaded content.
2. `EVEN_LINES` - Triggered for lines with an even index.
3. `ODD_LINES` - Triggered for lines with an odd index.
4. `CONTENT_LINES` - Triggered for lines containing text.
5. `EMPTY_LINES` - Triggered for empty lines.

<br>

### Defining Events

To define an event, use the `ON` command followed by the event name. Then, provide the code that will execute when the event is triggered.

**Example:**
```bash
ON START
    PRINT "Process started"

ON END
    PRINT "Process finished"

ON EACH_LINE
    PRINT "Each line"

ON EMPTY_LINES
    PRINT "Empty line detected"
```

<br>

When a command is left outside of an event, it is automatically defined within the **global event**:

```bash
PRINT "Hello World"        # GLOBAL Event
ON START
	PRINT "Hi World"    # START Event
```

<br>

### Execution Order

The execution of events follows the following order of priority, ensuring a controlled flow in content processing:

- `START`
- `GLOBAL`
- `EACH_LINE`, `EVEN_LINES`, `ODD_LINES`, `CONTENT_LINES`, `EMPTY_LINES`
- `END`

<br>

**Example:**
```bash
PRINT "First, I'm outside"          # GLOBAL Event

ON START
    PRINT "Second, I'm inside"      # START Event
```

<br>

**Execution result of the example:**
```
Second, I'm inside
First, I'm outside
```
<br>


> [!IMPORTANT]
> The `ON` command must not be indented and cannot receive dynamic values (such as variables) as parameters.

<br>

## Environment Variables

Environment variables in Scriptal, also known as envars, are special variables that the user cannot modify directly. These variables are generated dynamically based on the execution context and are available at different stages of text processing.

### Global Context

- `@CONTENT` - Returns all the content that has been loaded previously.
- `@LINES`   - Returns all the lines of the loaded content.
- `@STORED`  - Returns the stored lines.

<br>

**Example:**
```bash
INPUT "Sample text\nSecond text"
PRINT @CONTENT       # Displays the full content
PRINT @LINES         # Displays all the lines of the content
```

<br>

### Line Context

These variables are available only within events that process lines, such as `EACH_LINE`, `EVEN_LINES`, `ODD_LINES`, `CONTENT_LINES`, and `EMPTY_LINES`:

- `@LINE` - Returns the current line being processed.
- `@LINE_INDEX` - Returns the index of the current line.
- `@WORDS` - Returns all the words of the current line as an array.
- `@CHARS` - Returns all the characters of the current line as an array.

<br>

**Example:**
```coffeescript
ON EACH_LINE
    PRINT @LINE                # Displays the current line
    PRINT @LINE_INDEX          # Displays the index of the current line
    PRINT @WORDS               # Displays all the words in the current line
    PRINT @CHARS               # Displays all the characters in the current line
```

<br>

### Loop Context

These variables are available only within the `ITERATE` and `REPEAT` loops:

- `@KEY` - Returns the value of the current iteration. If it doesn't exist, returns the index.
- `@INDEX` - Returns the index of the current iteration.

<br>

**Example:**
```coffeescript
SET $array AS ["a", "b", "c"]
ITERATE $array
    PRINT @KEY       # Displays the value of the current iteration
    PRINT @INDEX     # Displays the index of the current iteration
```

<br>

### Definition Context

These variables are available within a definition using the `DEFINE` command:

- `@PARAMS` - Returns an array with the parameters of the used command.
- `@SYNTAX` - Returns the index of the current syntax.
- `@SELF` - Allows exporting the command from its definition.

<br>

**Example:**
```coffeescript
DEFINE MY_FUNCTION
    PARAMS [STRING]
    PRINT @PARAMS   # Displays the parameters passed to the function

MY_FUNCTION "param1"

```

<br>

## Loops

In Scriptal, loops allow you to repeat blocks of code under certain conditions. There are two main types of loops: `ITERATE` and `REPEAT`, as well as `WHILE` for more flexible repetition conditions.

<br>

### ITERATE Loop

The `ITERATE` loop allows you to iterate over elements of an array, characters of a string, or a range of numbers. You can use `ITERATE` to traverse arrays and perform operations on each element or to loop through a range of numbers.

During the execution of this loop, the environment variables `@KEY` and `@INDEX` are available for use within it.

**Iterating an Array**

```bash
SET $array AS ["Apple", "Pear", "Banana"]

ITERATE $array
    PRINT @KEY
```

<br>

**Define Variable for Iteration Value**

You can define a variable to store the current iteration value using the `AS` connector:

```bash
ITERATE $array AS $key
    PRINT $key
```

<br>

If you need to get the index, you can define 2 variables within an array:

```bash
ITERATE $array AS [$index, $key]
    PRINT "N° " + $index + " - " + $key
```

<br>

**Iterate Over a Range**

You can iterate over a range of numbers using the `FROM` and `TO` connectors. You can specify an ascending or descending range:

```bash
# Iterate from 1 to 100
ITERATE FROM 1 TO 100
    PRINT @KEY

# Iterate from 1 to 100 with variable
ITERATE FROM 1 TO 100 AS $count
    PRINT $count
    
# Iterate from 100 to 20 in countdown
ITERATE FROM 100 TO 20
    PRINT @KEY
```

<br>

### REPEAT Loop

The `REPEAT` loop executes a block of code a specific number of times. It is useful when you need to repeat an action a predetermined number of times.

```bash
SET $n AS 0
REPEAT 5
    INCREMENT $n
    PRINT "Hey you! Counting " + $n + " times!"
```

<br>

### WHILE Loop

The `WHILE` loop allows you to repeat a block of code as long as a specific condition is true. It is useful when the number of repetitions is not fixed but depends on a condition that may change during execution.

**WHILE Loop Syntax**

```bash
WHILE {condition}
    # Commands to execute while the condition is true
```

<br>

**Example:**
```bash
SET $count AS 1

WHILE $count <= 5
    PRINT "Count: " + $count
    SET $count AS $count + 1
```

<br>


### BREAK Command

The `BREAK` command is used to exit a loop immediately. When `BREAK` is executed, the control flow is interrupted, and the program continues with the code that follows after the loop.


**Example:**
```bash
SET $numbers AS [1, 2, 3, 4, 5]

ITERATE $numbers
    WHEN @KEY IS 3
        PRINT "Breaking out of the loop"
        BREAK
    PRINT @KEY
```

<br>

### CONTINUE Command

The `CONTINUE` command is used to skip to the next iteration of a loop, bypassing the remaining code in the current iteration. This is useful for skipping certain conditions and continuing with the next cycle of the loop.

**Example:**
```bash
SET $numbers AS [1, 2, 3, 4, 5]

ITERATE $numbers
    WHEN @KEY % 2 IS 0
        PRINT "Skipping even number: " + @KEY
        CONTINUE
    PRINT @KEY
```

<br>

> [!IMPORTANT]
> The `BREAK` and `CONTINUE` commands only have an effect within `ITERATE`, `REPEAT`, and `WHILE` loops.

<br>

## Defining Commands

In Scriptal, you can create your own custom commands and execute them anywhere in your code. To define a new command, use the `DEFINE` command followed by the name of the command. The name cannot contain special characters, except for `_`, and cannot start with numbers.

**Example:**
```bash
# Define the command
DEFINE GREET
    PRINT "HEY"

# Execute the command
GREET
```

<br>

### Parameters

You can make your commands dynamic by using parameters or arguments. To define parameters, use the `PARAMS` command within the command definition.

**Syntax**: `PARAMS {ARRAY}`

`PARAMS` requires a single parameter, which must be an array specifying the expected data types. Valid parameters are:

- `STRING` - Allows only text.
- `NUMBER` - Allows only numbers.
- `ARRAY` - Allows only arrays or lists.
- `MAP` - Allows only maps.
- `BOOL` - Allows only booleans.
- `NULL` - Allows only null values.
- `PATTERN` - Allows only patterns (regular expressions).
- `ANY` - Allows any value.
- `VAR` - Allows the name of a variable in raw form. It will not resolve the variable.

<br>

You can also select different types of arrays:

- `ARRAY_STRING` - Allows only arrays of strings.
- `ARRAY_NUMBER` - Allows only arrays of numbers.
- `ARRAY_BOOL` - Allows only arrays of booleans.
- `ARRAY_ARRAY` - Allows only arrays of arrays.
- `ARRAY_MAP` - Allows only arrays of maps.
- `ARRAY_VAR` - Allows only arrays of raw variables.

<br>

To access parameters, use the environment variable `@PARAMS`.

**Example:**
```bash
DEFINE GREET
    PARAMS [STRING]
    PRINT "Hello " + @PARAMS.1

GREET "Martin" # Result: Hello Martin
GREET 3232 # ERROR: Expected a STRING, not a NUMBER.
```

<br>

**Multiple Parameters:**

If you want to use multiple parameters, you can extend `PARAMS` as follows:

```bash
DEFINE GREET_MULTIPLE
    PARAMS [STRING, STRING, STRING] # Requires 3 parameters of type STRING
    PRINT "Hello " + @PARAMS.1
    PRINT "Hello " + @PARAMS.2
    PRINT "Hello " + @PARAMS.3

GREET_MULTIPLE "Martin" "Mike" "Peter"
```

<br>

**Parameters of Different Types:**

If you want to use multiple data types for a parameter, you can nest the types within an array:

```bash
DEFINE PROCESS
    PARAMS [NUMBER, [STRING, NUMBER], NUMBER]
    PRINT "Number: " + @PARAMS.1
    PRINT "Text or Number: " + @PARAMS.2
    PRINT "Final Number: " + @PARAMS.3

PROCESS 1 "text" 3
PROCESS 1 2 3
```

<br>

**Connectors as Parameters**

When defining parameters with `PARAMS`, connectors are automatically recognized and filtered out, leaving only the values you actually need for processing. This means that `@PARAMS` will only recognize data types and not connectors.

```bash
DEFINE CONNECT
    PARAMS [STRING, TO, STRING]
    PRINT "Connecting " + @PARAMS.1 + " with " + @PARAMS.2

CONNECT "A" TO "B" # Result: Connecting A with B
```

<br>


### Multiple Syntaxes

In Scriptal, native optional parameters do not exist. However, you can simulate this functionality by defining different syntaxes for the same command using the `PARAMS` command multiple times. Each use of `PARAMS` defines a different syntax.

The selected syntax will be available in the environment variable `@SYNTAX`:

```bash
DEFINE TYPE_OF
    PARAMS [STRING]             # Syntax 1
    PARAMS [NUMBER]             # Syntax 2
    PARAMS [STRING, STRING]     # Syntax 3

    WHEN @SYNTAX IS 1
        PRINT "I am a string"
        
    WHEN @SYNTAX IS 2
        PRINT "I am a number"
    
    WHEN @SYNTAX IS 3
        PRINT "We are strings"

TYPE_OF "hello"          # Result: I am a string
TYPE_OF 123             # Result: I am a number
TYPE_OF "hello" "world"  # Result: We are strings
```

In this example, the command `TYPE_OF` can accept a string, a number, or two strings, and will act according to the type and number of parameters it receives.

<br>

### RETURN Command

The `RETURN` command is used to return a value from a user-defined command. This allows you to create more complex and reusable functions.

```bash
DEFINE SUM
    PARAMS [NUMBER, NUMBER]
    SET $result AS @PARAMS.1 + @PARAMS.2
    RETURN $result

SET $total AS SUM 5 10;
PRINT $total # Result: 15
```

<br>

### Global Scope of Commands

All commands are global and do not have a limited scope; however, they will only be available after being defined. This means that a command cannot be used before its definition in the code. Additionally, due to this global nature, **it is not possible to define a command within another command**.

<br>

It is important to define each command independently to avoid conflicts and ensure that the command is accessible from anywhere in the code after its definition.

<br>

### RASSIGN Command

The `RASSIGN` command allows exposing variables to higher contexts, facilitating communication between different [scopes](#scope-of-variables). Below is its usage and purpose.

#### Scope of Variables

Variables created within a definition (using `DEFINE`) are only available in that context. This means they cannot be accessed from outside the definition.

```bash
# Define the command
DEFINE COMMAND
    SET $name AS "Mike"
    PRINT $name # Prints the variable $name (within the definition)

# Execute the command
COMMAND

PRINT $name # Error: The variable does not exist. This is because the variable was created within the definition.

```

Until now, the only known way to expose a variable to a higher context was by creating a global variable using the `GLOBAL_SET` command.

<br>

#### Exposing Variables

The `RASSIGN` command allows creating a variable in a higher context at the end of the execution of the defined command. This facilitates access to variables created within definitions from the context where the command is invoked.

**Syntax**

`RASSIGN {ANY} TO {VAR: STR}`

<br>

**Example:**

```bash
DEFINE EXPOSE_VAR
    RASSIGN "Hello World" TO "$var" # Creates the variable "$var" in the higher scope.
    PRINT $var   # Error: Unknown variable, as $var is not in the current context.

# Execute the command
EXPOSE_VAR
PRINT $var  # "Hello World". The variable was created in the higher context.
```

<br>

#### Null Variable

If the variable that receives `RASSIGN` is `NULL`, the command **will return the value** without creating the variable:

**Example:**

```bash
DEFINE TESTING
    PRINT RASSIGN "Hello World" TO NULL;    # Hello World

TESTING # Hello World
# No variables are created
```

<br>


#### Advanced Uses of `RASSIGN`

The true purpose of `RASSIGN` is to allow the creation of variables in the upper context using the syntax of the [connector `AS`](#connector-as), simulating the syntax of most commands in Scriptal.

**Example:**

```bash
UPPERCASE "Hola" AS $upper
PRINT $upper    # HOLA

PRINT UPPERCASE "Mundo";  # Mundo
```

In this example, `UPPERCASE` can be used both as an [internal command](#internal-commands) and as an independent command.

<br>

You can define custom commands that expose variables in the upper context.

**Example:**

```bash
DEFINE STRETCH_WORD
    # Create 2 syntaxes
    PARAMS [STRING]
    PARAMS [STRING, AS, VAR]

    # Retrieve the value of parameter #2, if it does not exist, it will return NULL.
    GET 2 IN @PARAMS AS $VAR

    # Example: Split the string into individual characters and join with hyphens
    SPLIT @PARAMS.1 BY "" AS $word
    JOIN $word WITH "-" AS $result  # Example: h-e-l-l-o

    # Return the result if $VAR is not defined
    # Otherwise, assign the result to the variable without returning anything.
    RETURN RASSIGN $result TO $VAR;

# Run as an internal command
PRINT STRETCH_WORD "Hola";

# Run as an independent command and store the value in a variable.
STRETCH_WORD "WORLD" AS $stretched
PRINT $stretched
```

<br>

In this example, `STRETCH_WORD` can be invoked both as an internal command to print the result directly and as an independent command to store the result in a variable.

<br>

## Error Handling

In Scriptal, error handling is crucial for ensuring that your code runs correctly and for identifying runtime issues. Below are the commands and best practices for managing errors in Scriptal:

### TRY Command

Starts a block of code where errors can be captured and handled. Use `TRY` to wrap code that might generate an exception. It should be used together with the `CATCH` command.

**Syntax:**
```bash
TRY
    # Code that may cause errors
    # ...
CATCH
    # Code to handle the error
    # ...
```

Errors of type `SyntaxError` cannot be captured.

<br>

### CATCH Command

It is used to handle any error that occurs within the `TRY` block. It specifies the code that should be executed if an error is detected.

**Syntax:**
```bash
CATCH
    # Code to handle the error
```

It can also be used with `CATCH AS {VAR}` to store the details of the error in a variable.

**Example:**
```bash
TRY
    PRINT 3 / 0 # Division by zero
CATCH AS $error
    PRINTF $error
```

<br>

### ERROR Command

Allows you to throw a generic error using a string.

**Example:**
```bash
ERROR "Something went wrong"
```

<br>

### THROW Command

Allows you to manually throw an error. Use `THROW` to generate an exception with a specific message. It is typically used with commands that generate errors in the form of maps, such as `SYNTAX_ERROR`, `VALUE_ERROR`, etc.

**Example:**
```bash
THROW VALUE_ERROR ["The value is not valid"];
THROW RUNTIME_ERROR ["Something bad happened."];
```

<br>

Review the complete list of error handling commands [here](Commands_EN.md#error-handling).

**Example:**

```bash
TRY
    SET $value AS 10
    THROW VALUE_ERROR ["Test error"];
CATCH AS $error_message
    PRINT "An error has occurred:"
    PRINTF $error_message
```


<br>

## Modules

In Scriptal, you can organize your code into modules to keep it clean, reusable, and easy to maintain. Modules allow you to include command definitions, variables, and any other necessary logic. The module functionality is a **BETA** feature, so some aspects are still under development.

### Import Modules

To import a module, use the `IMPORT` command followed by the module file name as a string. The file extension is optional. The file must be in the same directory as your main script or in an accessible path.

**Example:**

Suppose you have a file named `greetings.stal` with the following content:

```bash
# File: greetings.stal

DEFINE GREET
    PRINT "Hello!"

DEFINE FAREWELL
    PRINT "Goodbye!"
    
EXPORT "GREET"
EXPORT "FAREWELL"
```

<br>

To import and use this module in your main script, you would do the following:

```bash
# Main file

IMPORT "greetings"

GREET   # Result: Hello!
FAREWELL  # Result: Goodbye!
```

<br>

### Exporting Variables and Commands

To share variables or commands from one module to another, use the `EXPORT` command.

<br>

**Variables:**

You can export variables individually:

```bash
SET $URL AS "https://example.com"
SET $CONSTANT AS "Hello World!"

EXPORT $URL
EXPORT "$CONSTANT"
```

<br>

**Commands:**

The command can be exported either as a string or from within its definition using `EXPORT @SELF`:

```bash
DEFINE GREET
    PRINT "Hola"

EXPORT "GREET"

DEFINE COUNT_10
    EXPORT @SELF
    ITERATE FROM 1 TO 10 AS $i
        PRINT $i
```

<br>

In the main script, you can access these variables or commands as follows:

```bash
IMPORT "module"

PRINT $URL

GREET
COUNT_10
```

<br>

### Complex Modules

Modules are imported in their entirety and not in portions. You cannot select specific commands to import. Additionally, if a module calls another module that has already been imported, a circular import error will be raised.

This is because Scriptal does not support methods or attributes.


<br>

## Libraries

Libraries allow you to extend the capabilities of the language by providing additional functions and utilities that you can import and use in your scripts. Libraries work similarly to modules, but they are predefined and available to all Scriptal users.

### Import Libraries

To import a library, use the `IMPORT` command followed by the name of the library as a string. If the specified name does not correspond to a file in the project root, Scriptal will look in the predefined libraries.

For example, if you want to import the `COLORS` library, which provides a variety of color-related functions, you can do it as follows:

```bash
IMPORT "COLORS"
```

<br>

If `COLORS` _(COLORS.stal)_ is not a file in the project root, Scriptal will search in the predefined libraries.

<br>

Libraries are located in the `modules` directory at the root of the Scriptal executable. Each library must be inside a folder with the same name to be recognized by Scriptal:

```
modules/
    MyLibrary/
        MyLibrary.stal
```

<br>

### Using Libraries

Once the library is imported, you can use its functions and variables as if they were part of your script.

```bash
# Import the COLORS library
IMPORT "COLORS"

# Use a function from the COLORS library
SET $color AS "#FF5733"
COLOR_DARKEN $color WITH 50 AS $darker_color
PRINT $darker_color  # Result: A darker color than the original

# Another function from COLORS
COLOR_INVERT $color AS $inverted_color
PRINT $inverted_color  # Result: The inverted color
```

<br>

### Available Libraries

| Name    | Description                                                           | Documentation                       |
|---------|-----------------------------------------------------------------------|------------------------------------ |
| `COLORS` | Provides a set of commands for manipulating and working with colors. | [View](../modules/COLORS/COLORS.md) |



<br>

## Conventions

In Scriptal, naming conventions for commands and functions are designed to maintain clarity and consistency in the code. Below are the main conventions described in detail:

<br>

### Word Separation

When a command or function consists of more than one word, an underscore (`_`) is used to separate each word. This helps clearly distinguish each word within the command, enhancing readability.

Example: `FILE_OPEN`

<br>

### Abbreviations

In cases where a word is abbreviated, underscores are not used to separate words within the command. This applies even if the command consists of more than one word. The lack of underscores in abbreviations allows for a compact and recognizable form of commands.

Example: `SQRT` (Square Root), `PRINTF` (Print Format), etc.

<br>

### Glossary

Scriptal maintains a glossary of commonly abbreviated words that are considered complete within the context of commands and functions. These abbreviations are predefined and recognized to facilitate command writing.

- **VAR**: VARIABLE
- **CMD**: COMMAND
- **DIFF**: DIFFERENCE
- **DEC**: DECIMAL
- **HEX**: HEXADECIMAL
- **OCT**: OCTAL
- **BIN**: BINARY

<br>

Thus, there are commands like `VAR_EXISTS`, etc., instead of `VARIABLE_EXISTS`.

<br>

### Variables

Variables follow the `snake_case` naming convention, similar to Python. This means that words are separated by underscores and all letters are in lowercase.

```bash
SET $user_name AS "Martin"
```

<br>

### Constants

Although Scriptal does not support constants in the traditional sense, it is recommended to use uppercase names for variables that should be treated as constants. This helps visually distinguish variables that should not change value.

```bash
SET $MAX_CONNECTIONS AS 10
```

<br>

## Additional Resources

- [Complete List of Commands](./Commands_EN.md)
- [Code Examples](../examples/)


