# Command List

## Introduction to Command Syntax

In this guide, curly braces `{}` are used to specify the expected data type, such as STRING, NUMBER, MAP, ARRAY, BOOL, among others. When you see the `|` (pipe) symbol, it means that the command can accept more than one data type. For example, `{STRING|NUMBER}` indicates that both a string and a number can be used.

If you encounter a name followed by a colon `:`, such as `{FILE: STRING}`, it indicates that the parameter requires a STRING, but the descriptor "FILE" is used only to facilitate understanding of what is expected.

The use of `{VAR}` implies that the command requests a variable in its raw form, meaning one that has not yet been evaluated or assigned, ideal for situations where you want to create or modify variables without reading their value beforehand.

Finally, square brackets `[]` are used to indicate that a parameter is optional. For example, in `UPPERCASE {STRING} [AS {VAR}]`, the `AS {VAR}` is optional and does not need to be provided when using the command.

<br>

## Complete List of Commands in Scriptal

1. [Variables and Values. ](#variables-and-values)
   1. [ Variable and Value Manipulation. ](#variable-and-value-manipulation)
   2. [ Variable and Value Comparison. ](#variable-and-value-comparison)

2. [ Strings. ](#strings)
   1. [ String Manipulation. ](#string-manipulation)
   2. [ Casing. ](#casing)
   3. [ String Comparison. ](#string-comparison)

3. [ Arrays. ](#arrays)

4. [ Maps. ](#maps)

5. [ Conversion. ](#conversion)
   1. [ Type Conversion. ](#type-conversion)
   2. [ Character and Number Conversion. ](#character-and-number-conversion)

6. [ Mathematical Operations. ](#mathematical-operations)

7. [ Patterns and Regular Expressions. ](#patterns-and-regular-expressions)

8. [ Files and Directories. ](#files-and-directories)
   1. [ File Manipulation. ](#file-manipulation)
   2. [ Directory Manipulation. ](#directory-manipulation)
   3. [ File Path Manipulation. ](#file-path-manipulation)
   4. [ JSON Manipulation. ](#json-manipulation)

9. [ Random Values. ](#random-values)

10. [ Control Flow. ](#control-flow)
    1. [ Conditionals. ](#conditionals)
    2. [ Loops. ](#loops)
    3. [ Definitions. ](#definitions)
    4. [ Modules. ](#modules)
    5. [ Error Handling. ](#error-handling)

11. [ Input/Output. ](#inputoutput)
    1. [ Content. ](#content)
    2. [ Misc. ](#misc)

12. [ Dates and Times. ](#dates-and-times)

13. [ HTTP Requests. ](#http-requests)

14. [ Translation. ](#translation)


<br>

## Variables and Values

### Variable and Value Manipulation

**SET**

- **Syntax:** `SET {VAR|ARRAY_VAR} [AS {VALUE}]`
- **Description:** Assign a value to a variable or an array of variables.
- **Example:**
  
    ```bash
    SET $x AS 10
    SET [$x, $y, $z] AS [10, 20, 30]
    ```

<br>

**UNSET**

- **Syntax:** `UNSET {VAR|ARRAY_VAR}`
- **Description:** Remove a variable or an array of variables.
- **Example:**
    ```bash
    UNSET $x
    UNSET [$x, $y, $z]
    ```

<br>

**VAR_SET**

- **Syntax:** `VAR_SET {STRING} AS {VALUE}`
- **Description:** Assign a value to a variable with a specific name.
- **Example:**
    ```bash
    VAR_SET "my_var" AS 10
    ```

<br>

**VAR_GET**

- **Syntax:** `VAR_GET {STRING} [AS {VAR}]`
- **Description:** Retrieve the value of a variable with a specific name.
- **Example:**
    ```bash
    SET $var1 AS 1
    SET $var2 AS 2
    SET $var3 AS 3
    SET $id AS 1

    # var1
    PRINT VAR_GET "var" + $id;
    ```

<br>

**VAR_UNSET**

- **Syntax:** `VAR_UNSET {STRING}`
- **Description:** Remove a variable with a specific name.
- **Example:**
    ```bash
    VAR_UNSET "my_var"
    ```

<br>

**VAR_EXISTS**

- **Syntax:** `VAR_EXISTS {STRING} [AS {VAR}]`
- **Description:** Check if a variable with a specific name exists.
- **Example:**
    ```bash
    VAR_EXISTS "my_var" AS $exists    # TRUE / FALSE
    ```

<br>

**GLOBAL_SET**

- **Syntax:** `GLOBAL_SET {VAR|ARRAY_VAR} [AS {VALUE}]`
- **Description:** Assign a value to a variable or an array of variables globally.
- **Example:**
  
    ```bash
    DEFINE EXAMPLE
        GLOBAL_SET $var AS "Hello World"
    
    EXAMPLE
    PRINT $var  # Hello World
    ```

<br>

**RASSIGN**

- **Syntax:** `RASSIGN {ANY} TO {DEST: STRING|NULL}`
- **Description:** Its name comes from "_Return or Assign_". It allows exposing variables to higher contexts, facilitating communication between different scopes. If `DEST` is `NULL`, the command simply returns the value without assigning it to any variable. It is used to mimic the functionality of `[AS {VAR}]` in custom commands.

- **Example:**
    ``````bash
    DEFINE STRETCH_WORD
        PARAMS [STRING]
        PARAMS [STRING, AS, VAR]

        GET 2 IN @PARAMS AS $VAR

        SPLIT @PARAMS.1 BY "" AS $word
        JOIN $word WITH "-" AS $result  # Example: h-e-l-l-o

        # Returns the result if $VAR is not defined
        # Otherwise, assigns the result to the variable without returning anything.

        RETURN RASSIGN $result TO $VAR;

    PRINT STRETCH_WORD "hello";         # In this case, it will return a value
    STRETCH_WORD "hello" AS $stretched  # Here it returns nothing, but the value is assigned to the variable
    PRINT $stretched


<br>

**CLEAR**

- **Syntax:** `CLEAR {VAR}`
- **Description:** Clear the content of a variable.
- **Example:**
    ```bash
    CLEAR $x    # Clear the content of the variable $x.
    ```

<br>

**COPY**

- **Syntax:** `COPY {ANY} [AS {VAR}] `
- **Description:** Copy the data type or value of a variable.
- **Example:**
    ```bash
    COPY $x AS $y
    ```

<br>

**CLONE**

- **Syntax:** `CLONE {ANY} AS {VAR}`
- **Description:** Create an exact copy of a data type or variable.
- **Example:**
    ```bash
    CLONE $x AS $clone
    ```

<br>

**PICK**

- **Syntax:** 
    1. `PICK {ANY} OR {ANY} [AS {VAR}]`
    2. `PICK {ARRAY} [AS {VAR}]`

- **Description:** Select the first true value between two options or from an array.
- **Ejemplo**
    ```bash
    PICK "" OR 32   # Returns 32
    PICK [FALSE, 0, "", TRUE]   # Returns TRUE
    ```

<br>

**LENGTH**

- **Syntax:** `LENGTH {ANY} [AS {VAR}]`
- **Description:** Return the length of an array, string, or the number of keys in a map.
- **Example:**
    ```bash
    LENGTH "Hello" AS $len  # Returns 5
    ```

<br>

**INCREMENT**

- **Syntax:** `INCREMENT {VAR} [WITH {NUMBER}]`
- **Description:** Increment the value of a numeric variable by a specific value (default is increment by 1).
- **Example:**
    ```bash
    INCREMENT $x WITH 2    # Increases $x by 2
    INCREMENT $x    # Increases $x by 1
    ```

<br>

**DECREMENT**

- **Syntax:** `DECREMENT {VAR} [WITH {NUMBER}]`
- **Description:** Decrement the value of a numeric variable by a specific value (default is decrement by 1).
- **Example:**
    ```bash
    DECREMENT $x WITH 2    # Decreases $x by 2
    DECREMENT $x    # Decreases $x by 1
    ```

<br>

### Variable and Value Comparison


**IS_STRING**

- **Syntax:** `IS_STRING {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a string.

- **Example:**

  ```bash
  IS_STRING "Hello, world!" AS $result
  ```

<br>

**IS_NUMBER**

- **Syntax:** `IS_NUMBER {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a number (integer or float).

- **Example:**

  ```bash
  IS_NUMBER 42 AS $result
  ```

<br>

**IS_INTEGER**

- **Syntax:** `IS_INTEGER {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is an integer.

- **Example:**

  ```bash
  IS_INTEGER 123 AS $result
  ```

<br>

**IS_FLOAT**

- **Syntax:** `IS_FLOAT {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a float.

- **Example:**

  ```bash
  IS_FLOAT 12.34 AS $result
  ```

<br>

**IS_ARRAY**

- **Syntax:** `IS_ARRAY {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is an array.

- **Example:**

  ```bash
  IS_ARRAY [1, 2, 3] AS $result
  ```

<br>

**IS_MAP**

- **Syntax:** `IS_MAP {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a map (dictionary).

- **Example:**

  ```bash
  IS_MAP {"key": "value"} AS $result
  ```

<br>

**IS_BOOL**

- **Syntax:** `IS_BOOL {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a boolean.

- **Example:**

  ```bash
  IS_BOOL TRUE AS $result
  ```

<br>

**IS_PATTERN**

- **Syntax:** `IS_PATTERN {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a pattern (regular expression).

- **Example:**

  ```bash
  IS_PATTERN $pattern AS $result
  ```

<br>

**IS_EMPTY**

- **Syntax:** `IS_EMPTY {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is empty.

- **Example:**

  ```bash
  IS_EMPTY "" AS $result
  ```

<br>

**IS_EVEN**

- **Syntax:** `IS_EVEN {NUMBER} [AS {VAR}]`

- **Description:** Checks if the specified number is even.

- **Example:**

  ```bash
  PRINT IS_EVEN 2 $num; # TRUE
  ```

<br>

**IS_ODD**

- **Syntax:** `IS_ODD {NUMBER} [AS {VAR}]`

- **Description:** Checks if the specified number is odd.

- **Example:**

  ```bash
  PRINT IS_ODD 3 $num; # TRUE
  ```

<br>

**IS_NAN**

- **Syntax:** `IS_NAN {ANY} [AS {VAR}]`

- **Description:** Checks if the specified value is [NAN](https://en.wikipedia.org/wiki/NaN) (Not a Number).

- **Example:**

  ```bash
  SET $num AS NAN
  IS_NAN $num AS $result
  ```

<br>

**IS_INFINITY**

- **Syntax:** `IS_INFINITY {ANY} [AS {VAR}]`

- **Description:** Checks if the specified value is infinite.

- **Example:**

  ```bash
  SET $num AS 1e308 * 10
  IS_INFINITY $num AS $result
  ```

<br>

**IS_DATE**

- **Syntax:** `IS_DATE {ANY} [AS {VAR}]`

- **Description:** Validates if the specified value is a date map.

- **Example:**

  ```bash
  IS_DATE $datemap AS $result
  ```

<br>



<br>

## Strings

### String Manipulation

**TRIM**

- **Syntax:** `TRIM {STRING} [AS {VAR}]`
- **Description:** Removes leading and trailing whitespace from the text.
- **Example:**
    ```bash
    TRIM "   Hello World!   " AS $trimmed   # "Hello World!"
    ```

<br>

**TRIM_START**

- **Syntax:** `TRIM_START {STRING} [AS {VAR}]`
- **Description:** Removes leading whitespace from the text.
- **Example:**
    ```bash
    TRIM_START "   Hello World!   " AS $trimmed_start   # "Hello World!   "
    ```

<br>

**TRIM_END**

- **Syntax:** `TRIM_END {STRING} [AS {VAR}]`
- **Description:** Removes trailing whitespace from the text.
- **Example:**
    ```bash
    TRIM_END "   Hello World!   " AS $trimmed_end   # "   Hello World!"
    ```

<br>

**CLEAN**

- **Syntax:** `CLEAN {STRING} [AS {VAR}]`
- **Description:** Removes additional whitespace in a string. Unlike `TRIM`, `CLEAN` removes whitespace at any position.
- **Example:**
    ```bash
    PRINT CLEAN "Hello      World       Lorem       Ipsum";  # Hello Wolrd Lorem Ipsum
    ```

<br>

**PAD**

- **Syntax:** `PAD {STRING} TO {NUMBER} WITH {FILL: STRING} [AS {VAR}]`
- **Description:** Pads the text `STRING` to a length of `NUMBER` using the padding character `FILL`.
- **Example:**
    ```bash
    PAD "bird" TO 10 WITH "-"   # ---bird--- 
    ```

<br>

**PAD_START**

- **Syntax:** `PAD_START {STRING} TO {NUMBER} WITH {FILL: STRING} [AS {VAR}]`
- **Description:** Pads the text `STRING` with the padding character `FILL` at the beginning to a length of `NUMBER`.
- **Example:**
    ```bash
    PRINT PAD_START "2" TO 2 WITH "0";  # 02
    PRINT PAD_START "10" TO 2 WITH "0"; # 10
    ```

<br>

**PAD_END**

- **Syntax:** `PAD_END {STRING} TO {NUMBER} WITH {FILL: STRING} [AS {VAR}]`
- **Description:** Pads the text `STRING` with the padding character `FILL` at the end to a length of `NUMBER`.
- **Example:**
    ```bash
    PAD_END "Hello" TO 10 WITH "-" AS $padded_end
    ```
    <br>

**REPLACE**

- **Syntax:** `REPLACE {X: NUMBER} WITH {Y: NUMBER} IN {Z: NUMBER} [AS {VAR}]`
- **Description:** Replaces the first occurrence of `X` with `Y` in the text `Z`.
- **Example:**
    ```bash
    REPLACE "world" WITH "there" IN "Hello world!" AS $replaced # Hello there!
    ```

<br>

**REPLACE_ALL**

- **Syntax:** `REPLACE_ALL {X: NUMBER} WITH {Y: NUMBER} IN {Z: NUMBER} [AS {VAR}]`
- **Description:** Replaces all occurrences of `X` with `Y` in the text `Z`.
- **Example:**
    ```bash
    REPLACE_ALL "my" WITH "your" IN "Hi, my name is John. This is my house." AS $replaced   # Hi, your name is John. This is your house.
    ```

<br>

**INSERT**

- **Syntax:** `INSERT {SUBSTR} AT {INDEX: NUMBER} IN {STRING} [AS {VAR}]`

- **Description:** Inserts the text `SUBSTR` at position `INDEX` in the original text `STRING`.
- **Example:**
    ```bash
    INSERT "Wonderful " AT 7 IN "Hello World!" AS $inserted   # Hello Wonderful World!
    ```

<br>

**REMOVE**

- **Syntax:**
  
    1. `REMOVE {SUBSTR} IN {STRING} [AS {VAR}]`
    2. `REMOVE FROM {X: NUMBER} TO {Y: NUMBER} IN {STRING} [AS {VAR}]`
    
- **Description:** 
    1. Removes all occurrences of `SUBSTR` in the text `STRING`.
    2. Removes from `X` to `Y` in the text `STRING`.
- **Example:**
    ```bash
    REMOVE "world" IN "Hello world!" AS $removed        # Hello !
    REMOVE FROM 1 TO 6 IN "Hello world!" AS $removed    # world!
    ```

<br>

**SPLIT**

- **Syntax:** `SPLIT {STRING} BY {SEP: STRING} [AS {VAR}]`
- **Description:** Splits the text `STRING` into an array using `SEP` as the delimiter.
- **Example:**
    ```bash
    SPLIT "a,b,c" BY "," AS $split  # ["a", "b", "c"]
    ```

<br>

**REVERSE**

- **Syntax:** `REVERSE {STRING} [AS {VAR}]`
- **Description:** Reverses the text `STRING`.
- **Example:**
    ```bash
    REVERSE "Hello" AS $reversed    # olleH
    ```

<br>

**COUNT**

- **Syntax:** `COUNT {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Counts the occurrences of `SUBSTR` in the text `STRING`.
- **Example:**
    ```bash
    COUNT "l" IN "Hello" AS $count  # 2
    ```

<br>

**JOIN**

- **Syntax:** `JOIN {ARRAY} WITH {TOKEN: STRING} [AS {VAR}]`
- **Description:** Joins the elements of the array `ARRAY` using `TOKEN` as the separator.
- **Example:**
    ```bash
    JOIN ["a", "b", "c"] WITH "-" AS $joined    # "a-b-c"
    ```

<br>

**FIND**

- **Syntax:** `FIND {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Searches for the first occurrence of `SUBSTR` in the text `STRING`.
- **Example:**
    ```bash
    FIND "lo" IN "Hello" AS $found  # 4
    ```

<br>

**STARTS**

- **Syntax:** `STARTS {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Checks if the text `STRING` starts with `SUBSTR`.
- **Example:**
    ```bash
    STARTS "Hel" IN "Hello" # TRUE
    ```

<br>

**ENDS**

- **Syntax:** `ENDS {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Checks if the text `STRING` ends with `SUBSTR`.
- **Example:**
    ```bash
    ENDS "llo" IN "Hello"   # TRUE
    ```

<br>

**CONTAINS**

- **Syntax:** `CONTAINS {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Checks if the text `STRING` contains `SUBSTR`.
- **Example:**
    ```bash
    CONTAINS "beautiful" IN "You are so beautiful" AS $exists
    ```

<br>


**TRUNCATE**

- **Syntax:** 
    1. `TRUNCATE {STRING} TO {INDEX: NUMBER} [AS {VAR}]`
    1. `TRUNCATE {STRING} TO {INDEX: NUMBER} WITH {TOKEN} [AS {VAR}]`
- **Description:** Truncates the text `STRING` up to index `INDEX` and trims using `...` by default or using `TOKEN`.
- **Example:**
    ```bash
    PRINT TRUNCATE "Hello World" TO 5;          # Hello...
    PRINT TRUNCATE "Hello World" TO 5 WITH "***"; # Hello***
    ```

<br>

**ESCAPE**

- **Syntax:** `ESCAPE {STRING} [AS {VAR}]`
- **Description:** Escapes special characters in the text `STRING`.
- **Example:**
    ```bash
    ESCAPE "Hello\nworld!" AS $escaped  # Hello\nworld!
    ```

<br>

**UNESCAPE**

- **Syntax:** `UNESCAPE {STRING} [AS {VAR}]`
- **Description:** Unescapes special characters in the text `STRING`.
- **Example:**
    ```bash
    UNESCAPE "Hello\\nWorld!" AS $unescaped # Hello
    # World!
    ```

<br>

**NORMALIZE**

- **Syntax:** `NORMALIZE {STRING} TO {FORM: STRING} [AS {VAR}]`
- **Description:** Normalizes the text `STRING` to the specified `FORM` according to Unicode standards. Normalization ensures that characters are consistently represented, which is useful for text comparison, data storage, or uniform processing of inputs.

    <br>

    `FORM`: The desired normalization form, which can be `NFC`, `NFD`, `NFKC`, or `NFKD`.

    <br>

    For more information: [Unicode equivalence - Wikipedia](https://en.wikipedia.org/wiki/Unicode_equivalence)

- **Example:**
    ```bash
    # Texts to Compare
    SET $a AS "é" # e + combining acute accent
    SET $b AS "é"


    # Before Normalization
    PRINT $a IS $b    # FALSE


    # Normalize both texts to NFC
    NORMALIZE $a TO "NFC" AS $normmalized_a
    NORMALIZE $b TO "NFC" AS $normmalized_b


    # After Normalization
    PRINT $normmalized_a IS $normmalized_b  # TRUE
    ```

<br>

**TRANSLIT**

- **Syntax:** `TRANSLIT {STRING} [AS {VAR}]`
- **Description:** Transliterates text from any Unicode alphabet to an ASCII representation. This allows text, originally in alphabets like Cyrillic, Greek, or Chinese, to be transformed into a more accessible form for systems that only handle ASCII characters.
    
    <br>

    `TRANSLIT` provides an approximation, and may not always match the most accurate or expected transliterations, especially for texts with special characters or complex alphabets.

- **Example:**
    ```bash
    PRINT TRANSLIT "Καλημέρα κόσμεβ";  # Kalemera kosmeb
    PRINT TRANSLIT "Текст на кириллице";    # Tekst na kirillitse
    PRINT TRANSLIT "こんにちは";  # konnichiha
    ```

<br>

**REMOVE_ACCENTS**

- **Syntax:** `REMOVE_ACCENTS {STRING} [AS {VAR}]`
- **Description:** Cleans the text `STRING` to a standard form by removing accents and diacritics. This operation converts accented or special characters to their basic unaccented equivalents.
- **Example:**
    ```bash
    PRINT REMOVE_ACCENTS "café";  # cafe
    PRINT REMOVE_ACCENTS "A criança está à espera do ônibus na praça";  # A crianca esta a espera do onibus na praca
    ```

<br>

**CHAR_AT**

- **Syntax:** `CHAR_AT {N: NUMBER} IN {STRING} [AS {VAR}]`
- **Description:** Retrieves the character at position `N` in the text `STRING`.
- **Example:**
    ```bash
    CHAR_AT 2 IN "Hello" AS $char   # e
    ```

<br>

**FORMAT**

- **Syntax:** `FORMAT {STRING} WITH {ARRAY} [AS {VAR}]`
- **Description:** Formats the text `STRING` using the values from the array `ARRAY`.
- **Example:**
    ```bash
    FORMAT "Hello {1}" WITH ["world"] AS $formatted # Hello world
    ```

<br>

**EXTRACT**

- **Syntax:** 
    1. `EXTRACT FROM {X: NUMBER} TO {Y: NUMBER} IN {STRING} [AS {VAR}]`
    2. `EXTRACT BETWEEN {SUBSTR1} TO {SUBSTR2} IN {STRING} [AS {VAR}]`

- **Description:**
    1. Extracts a substring from position `X` to position `Y` in the text `STRING`.
    2. Extracts text between two substrings `SUBSTR1` and `SUBSTR2` in the text `STRING`.

- **Example:**
    ```bash
    EXTRACT FROM 1 TO 5 IN "Hello World" AS $extracted  # Hello
    EXTRACT BETWEEN "(" AND ")" IN "I am free. (I am not) Lorem Ipsum" AS $extracted2   # I am not
    ```

<br>

**EXTRACT_FIRST**

- **Syntax:** `EXTRACT_FIRST TO {N: NUMBER} IN {STRING} [AS {VAR}]`
- **Description:** Extracts the first `N` characters from the text `STRING`.
- **Example:**
    ```bash
    EXTRACT_FIRST TO 5 IN "Hello World" AS $start_extracted   # Hello
    ```

<br>

**EXTRACT_LAST**

- **Syntax:** `EXTRACT_LAST TO {N: NUMBER} IN {STRING} [AS {VAR}]`
- **Description:** Extracts the last `N` characters from the text `STRING`.
- **Example:**
    ```bash
     EXTRACT_LAST TO 5 IN "Hello World" AS $end_extracted   # World
    ```

<br>

### Casing

**UPPERCASE**

- **Syntax:** `UPPERCASE {STRING} [AS {VAR}]`

- **Description:** Converts the specified text to uppercase.

- **Example:**

  ```bash
  UPPERCASE "Hello world!" AS $upper  # HELLO WORLD!
  ```

<br>

**LOWERCASE**

- **Syntax:** `LOWERCASE {STRING} [AS {VAR}]`

- **Description:** Converts the specified text to lowercase.

- **Example:**

  ```bash
  LOWERCASE "Hello World!" AS $lower  # hello world!
  ```

<br>

**TITLECASE**

- **Syntax:** `TITLECASE {STRING} [AS {VAR}]`

- **Description:** Converts the specified text to title case (each word starts with an uppercase letter).

- **Example:**

  ```bash
  TITLECASE "hello world" AS $title   # Hello World
  ```

<br>

**SWAPCASE**

- **Syntax:** `SWAPCASE {STRING} [AS {VAR}]`

- **Description:** Converts uppercase letters to lowercase and vice versa in the text.

- **Example:**

  ```bash
  SWAPCASE "Hello World!" AS $swapped # hELLO wORLD!
  ```

  <br>

**CAMELCASE**

- **Syntax:** `CAMELCASE {STRING} [AS {VAR}]`

- **Description:** Converts the text to camelCase, where the first word starts with a lowercase letter and subsequent words start with an uppercase letter.

- **Example:**

  ```bash
  CAMELCASE "hello world example" AS $camelcase   # helloWorldExample
  ```

<br>

**KEBABCASE**

- **Syntax:** `KEBABCASE {STRING} [AS {VAR}]`

- **Description:** Converts the text to kebab-case, where words are separated by hyphens and all letters are in lowercase.

- **Example:**

  ```bash
  KEBABCASE "hello world example" AS $kebabcase   # hello-world-example
  ```

  <br>

**SLUG**

- **Syntax:** `SLUG {STRING} [AS {VAR}]`

- **Description:** Converts the text to a slug format, using only lowercase characters and separating words with hyphens. The command also removes special characters and accents to generate a URL-friendly format.

- **Example:**

  ```bash
  SLUG "Ich heiße" AS $slug            #ich-heisse
  KEBABCASE "Ich heiße" AS $kebabcase  #ich-hei-e
  ```


<br>

**MACROCASE**

- **Syntax:** `MACROCASE {STRING} [AS {VAR}]`

- **Description:** Converts the text to MACROCASE, where all words are in uppercase and separated by underscores.

- **Example:**

  ```bash
  MACROCASE "hello world example" AS $macrocase   # HELLO_WORLD_EXAMPLE
  ```

<br>

**CASEFOLD**

- **Syntax:** `CASEFOLD {STRING} [AS {VAR}]`

- **Description:** Normalizes the text for case-insensitive comparison by converting it to a standard comparison form.

- **Example:**

  ```bash
  CASEFOLD "Hello World!" AS $casefolded  # hello world!
  ```

<br>

**CAPITALIZE**

- **Syntax:** `CAPITALIZE {STRING} [AS {VAR}]`

- **Description:** Capitalizes the first letter of the text, converting the first letter to uppercase and the rest to lowercase.

- **Example:**

  ```bash
  CAPITALIZE "hello world" AS $capitalized    # Hello world
  ```

<br>



### String Comparison

**IS_EMAIL**

- **Syntax:** `IS_EMAIL {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string is a valid email address.

- **Example:**

  ```bash
  IS_EMAIL "example@example.com" AS $result
  ```

<br>

**IS_URL**

- **Syntax:** `IS_URL {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string is a valid URL.

- **Example:**

  ```bash
  IS_URL "https://www.example.com" AS $result
  ```

<br>

**IS_DIGIT**

- **Syntax:** `IS_DIGIT {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string contains only digits.

- **Example:**

  ```bash
  IS_DIGIT "123456" AS $result
  ```

<br>

**IS_NUMERIC**

- **Syntax:** `IS_NUMERIC {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string is numeric, allowing integers, floats, and exponents (e.g., ², ¾).

- **Example:**

  ```bash
  IS_NUMERIC "123.45" AS $result    # TRUE
  IS_NUMERIC "¾" AS $result2        # TRUE
  IS_NUMERIC "-43.322" AS $result3  # TRUE
  ```

<br>

**IS_ALPHA**

- **Syntax:** `IS_ALPHA {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string contains only alphabetic characters.

- **Example:**

  ```bash
  IS_ALPHA "Hello" AS $result
  ```

<br>

**IS_ALPHANUMERIC**

- **Syntax:** `IS_ALPHANUMERIC {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string contains only alphanumeric characters.

- **Example:**

  ```bash
  IS_ALPHANUMERIC "Hello123" AS $result
  ```

<br>

**IS_UPPERCASE**

- **Syntax:** `IS_UPPERCASE {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string is in uppercase.

- **Example:**

  ```bash
  IS_UPPERCASE "HELLO" AS $result
  ```

<br>

**IS_LOWERCASE**

- **Syntax:** `IS_LOWERCASE {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string is in lowercase.

- **Example:**

  ```bash
  IS_LOWERCASE "hello" AS $result
  ```

<br>

**IS_TITLECASE**

- **Syntax:** `IS_TITLECASE {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string is in title case (capitalization of the first letter of each word).

- **Example:**

  ```bash
  IS_TITLECASE "Hello World" AS $result
  ```

<br>

**IS_DEC**

- **Syntax:** `IS_DEC {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string represents a number in the decimal system.

- **Example:**

  ```bash
  IS_DEC "255" AS $result
  ```

<br>

**IS_HEX**

- **Syntax:** `IS_HEX {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string represents a hexadecimal number.

- **Example:**

  ```bash
  IS_HEX "1A3F" AS $result
  ```

<br>

**IS_ASCII**

- **Syntax:** `IS_ASCII {STRING} [AS {VAR}]`

- **Description:** Validates if the specified string contains only ASCII characters (basic English alphabet characters and standard symbols).

- **Example:**

  ```bash
  IS_ASCII "Hello!" AS $result
  ```

  <br>


##  Arrays

**GET**

- **Syntax:** `GET {INDEX: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Description:** Retrieves the element at the specified position within the array. If the index does not exist, returns `NULL`.
- **Example:**
    ```bash
    SET $array AS ["Paris", "London", "Tokyo"]
    GET 2 IN $array AS $element    # London
    ```

<br>

**APPEND**

- **Syntax:** `APPEND {ANY} TO {ARRAY}`
- **Description:** Adds an element to the end of an array.
- **Example:**
    ```bash
    APPEND "new_element" TO $array
    ```

<br>

**PUT**

- **Syntax:** 
    1. `PUT {ANY} IN {ARRAY}`
    2. `PUT {ANY} AT {INDEX: NUMBER} IN {ARRAY}`

- **Description:**
    1. Adds an element to the end of the array.
    2. Modifies or replaces the value at the specified index.

- **Example:**
    ```bash
    SET $array AS [1, 2, 3, 4 ,5]
    PUT "new" IN $array               # [1, 2, 3, 4, 5, "new"]
    PUT "modified" AT 3 IN $array     # [1, 2, "modified", 4, 5, "new"]
    ```

<br>

**INSERT**

- **Syntax:** `INSERT {ANY} AT {INDEX: NUMBER} IN {ARRAY}`
- **Description:** Inserts a new element at the specified position within an array.
- **Example:**
    ```bash
    SET $array AS [1, 2, 3]
    INSERT "new_element" AT 1 IN $array    # [1, "new_element", 2, 3]
    ```

<br>

**REMOVE**

- **Syntax:** 
    1. `REMOVE {INDEX: NUMBER} IN {ARRAY}`
    2. `REMOVE FROM {X: NUMBER} TO {Y: NUMBER} IN {ARRAY}`
- **Description:** Removes an element at a specific index or a range of indices in an array.
- **Example:**
    ```bash
    SET $array AS [1, 2, 3, 4, 5, 6, 7]
    REMOVE 2 IN $array              # [1, 3, 4, 5, 6, 7]
    REMOVE FROM 1 TO 3 IN $array    # [5, 6, 7]
    ```

<br>

**REMOVE_FIRST**

- **Syntax:** `REMOVE_FIRST {ARRAY}`
- **Description:** Removes the first element of an array.
- **Example:**
    ```bash
    SET $array AS [1, 2, 3]
    REMOVE_FIRST $array    # [2, 3]
    ```

<br>

**REMOVE_LAST**

- **Syntax:** `REMOVE_LAST {ARRAY}`
- **Description:** Removes the last element of an array.
- **Example:**
    ```bash
    SET $array AS [1, 2, 3]
    REMOVE_LAST $array  # [1, 2]
    ```

<br>

**REMOVE_DUPLICATES**

- **Syntax:** `REMOVE_DUPLICATES {ARRAY} [AS {VAR}]`
- **Description:** Returns a new array with duplicate elements removed.
- **Example:**
    ```bash
    REMOVE_DUPLICATES ["hello", "world", "hello", "there", "hello", "sir"]    # ["hello", "world", "there", "sir"]
    ```
    <br>

**DELETE**

- **Syntax:** `DELETE {ELEM: ANY} IN {ARRAY}`
- **Description:** Removes all occurrences of a specific value in an array.
- **Example:**
    ```bash
    DELETE "hola" IN ["world", "hello", "hola"]    # Removes "hola" from the array.
    ```


**SORT**

- **Syntax:** `SORT {ARRAY}`
- **Description:** Sorts an array in ascending order.
- **Example:**
    ```bash
    SET $array AS ["b", "c", "a"]
    SORT $array # ["a", "b", "c"]
    ```

<br>

**REVERSE**

- **Syntax:** `REVERSE {ARRAY}`
- **Description:** Reverses the order of elements in an array.
- **Example:**
    ```bash
    SET $array AS [1, 2, 3]
    REVERSE $array  # [3, 2, 1]
    ```

<br>

**SHUFFLE**

- **Syntax:** `SHUFFLE {ARRAY}`
- **Description:** Shuffles the elements of an array randomly.
- **Example:**
    ```bash
    SHUFFLE $array
    ```

<br>


**EXTEND**

- **Syntax:** `EXTEND {ARRAY} WITH {ARRAY}`
- **Description:** Adds the elements of one array to the end of another array, modifying the original array.
- **Example:**
    ```bash
    EXTEND $array1 WITH $array2    # Appends the elements of $array2 to the end of $array1
    ```

<br>

**FLATTEN**

- **Syntax:** `FLATTEN {ARRAY} [AS {VAR}]`
- **Description:** Flattens an array containing sub-arrays into a single level.
- **Example:**
    ```bash
    FLATTEN ["a", "b", ["c", "d", ["e"]]] AS $flat_array    # Returns ["a", "b", "c", "d", "e"]
    ```

<br>

**CLEAN**

- **Syntax:** `CLEAN {ARRAY} [AS {VAR}]`
- **Description:** Removes falsy values (`0`, `""`, `NULL`, `FALSE`) from an array.
- **Example:**
    ```bash
    CLEAN [1, 2, "", 0, NULL, TRUE] AS $clean_array    # Returns [1, 2, TRUE]
    ```

<br>

**DUPLICATES**

- **Syntax:** `DUPLICATES IN {ARRAY} [AS {VAR}]`
- **Description:** Returns an array with the duplicate elements from the original array.
- **Example:**
    ```bash
    DUPLICATES IN [1, 2, 2, 3, 4, 4, 5] AS $dups    # Returns [2, 4]
    ```

<br>

**INTERSECT**

- **Syntax:** `INTERSECT {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Description:** Returns an array with the common elements in both arrays.
- **Example:**
    ```bash
    INTERSECT [1, 2, 3] WITH [2, 3, 4] AS $common_elements    # Returns [2, 3]
    ```

<br>

**DIFFERENCE**

- **Syntax:** `DIFFERENCE {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Description:** Returns an array with the elements that are in the first array but not in the second.
- **Example:**
    ```bash
    DIFFERENCE [1, 2, 3] WITH [2, 4] AS $difference    # Returns [1, 3]
    ```

<br>

**MERGE**

- **Syntax:** `MERGE {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Description:** Returns the union of two arrays, removing duplicate elements.
- **Example:**
    ```bash
    MERGE [1, 2, 3] WITH [2, 3, 4] AS $merged_array    # Returns [1, 2, 3, 4]
    ```

<br>

**CONCAT**

- **Syntax:** `CONCAT {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Description:** Combines two arrays and returns a new array. Similar to `MERGE`, but without removing duplicates.
- **Example:**
    ```bash
    CONCAT [1, 2, 3] WITH [2, 3, 4] AS $result_array    # [1, 2, 3, 2, 3, 4]
    ```

<br>


**CHUNK**

- **Syntax:** `CHUNK {ARRAY} BY {NUMBER} [AS {VAR}]`
- **Description:** Divides an array into smaller chunks of specified size.
- **Example:**
    ```bash
    CHUNK [1, 2, 3, 4, 5] BY 2 AS $chunks    # Returns [[1, 2], [3, 4], [5]]
    ```

<br>

**TRANSFORM**

- **Syntax:** `TRANSFORM {ARRAY} WITH {COMMAND} [AS {VAR}]`
- **Description:** Applies a command to each element of an array, returning a new array with the results.
- **Example:**
    ```bash
    TRANSFORM ["hello", "world", "my", "name", "is", "John"] WITH "UPPERCASE" AS $transformed_array
    ```

<br>

**EXTRACT**

- **Syntax:** `EXTRACT FROM {X: NUMBER} TO {Y: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Description:** Extracts a portion of an array from index `X` to index `Y`.
- **Example:**
    ```bash
    SET $array AS [1, 2, 3, 4, 5, 6]
    EXTRACT FROM 1 TO 3 IN $array    # [1, 2, 3]
    ```

<br>

**EXTRACT_FIRST**

- **Syntax:** `EXTRACT_FIRST TO {N: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Description:** Extracts the first `N` elements from the array.
- **Example:**
    ```bash
    EXTRACT_FIRST TO 2 IN ["USA", "Spain", "UK", "Italy", "France"] AS $start_extracted   # ["USA", "Spain"]
    ```

<br>

**EXTRACT_LAST**

- **Syntax:** `EXTRACT_LAST TO {N: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Description:** Extracts the last `N` elements from the array.
- **Example:**
    ```bash
    EXTRACT_LAST TO 2 IN ["USA", "Spain", "UK", "Italy", "France"] AS $end_extracted   # ["Italy", "France"]
    ```

<br>

**COUNT**

- **Syntax:** `COUNT {ELEM: ANY} IN {ARRAY} [AS {VAR}]`
- **Description:** Counts the number of times an element appears in an array.
- **Example:**
    ```bash
    SET $array AS ["hello", "world", "hello", "there", "hello", "sir"] 
    COUNT "hello" IN $array AS $count    # 3
    ```
    <br>

**FIND**

- **Syntax:** `FIND {ANY} IN {ARRAY} [AS {VAR}]`
- **Description:** Finds the position of the first match in the array.
- **Example:**
    ```bash
    FIND "red" IN ["blue", "pink", "yellow", "red", "white"] AS $position    # Returns 4
    ```

<br>

**CONTAINS**

- **Syntax:** `CONTAINS {ELEM: ANY} IN {ARRAY} [AS {VAR}]`
- **Description:** Checks if an array contains the specified value.
- **Example:**
    ```bash
    CONTAINS "apple" IN ["pear", "watermelon", "apple", "lemon"] AS $exists
    ```

<br>


## Maps

**MAP_SET**

- **Syntax:** `MAP_SET {VAR}`
- **Description:** Defines or modifies a variable with a map.
- **Example:**
    ```bash
    MAP $data
    ```
    <br>

    Este comando ofrece definir una estructura, donde podrás anidar el comando `KEY` para crear claves:

    ```bash
    MAP $data
        KEY "user" : "John"
        KEY "age" : 30
        KEY "city" : "Spain"
    ```

<br>

**KEY**

- **Syntax:** `KEY {NAME} AS {VALUE}`
- **Description:** Defines or modifies a key in the current map structure. Must be within a `MAP` command.
- **Example:**
    ```bash
    MAP $dict
        KEY "hello" AS "Hola"
        KEY "world" : "Mundo" # Remember you can use "AS" or ":" 
    ```

<br>

**MAP_SET_KEY**

- **Syntax:** `MAP_SET_KEY {KEY: STRING} AS {VALUE} IN {MAP}`
- **Description:** Sets or updates a value for a key in a map.
- **Example:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    MAP_SET_KEY "name" AS "Doe" IN $map
    ```

<br>

**GET**

- **Syntax:** `GET {KEY: STRING} IN {MAP} [AS {VAR}]`
- **Description:** Gets the value of a key from a map. If the key does not exist, returns `NULL`.
- **Example:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    GET "name" IN $map AS $element    # John
    ```

<br>


**MAP_GET_KEY**

- **Syntax:** `MAP_GET_KEY {KEY: STRING} AS {VALUE} IN {MAP}`
- **Description:** Gets the value of a key from a map. Unlike `GET`, `MAP_GET_KEY` will throw an error if the key does not exist.
- **Example:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    MAP_SET_KEY "name" AS "Doe" IN $map
    ```

<br>

**MAP_KEYS**

- **Syntax:** `MAP_KEYS {MAP} [AS {VAR}]`
- **Description:** Gets all the keys from a map.
- **Example:**
    ```bash
    MAP_KEYS {"name": "John", "age": 30} AS $keys   # ["name", "age"]
    ```

<br>

**MAP_VALUES**

- **Syntax:** `MAP_VALUES {MAP} [AS {VAR}]`
- **Description:** Gets all the values from a map.
- **Example:**
    ```bash
    MAP_VALUES {"name": "John", "age": 30} AS $values   # ["John", 30] 
    ```

<br>

**MAP_ITEMS**

- **Syntax:** `MAP_ITEMS {MAP} [AS {VAR}]`
- **Description:** Gets all key-value pairs from a map as an array of arrays.
- **Example:**
    ```bash
    MAP_ITEMS {"name": "John", "age": 30} AS $items # [["name", "John"], ["age", 30]]
    ```
    <br>

**DELETE**

- **Syntax:** `DELETE {KEY: STRING} IN {MAP}`
- **Description:** Removes a key and its associated value from a map.
- **Example:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    DELETE "age" IN $map
    ```

<br>

**CONTAINS**

- **Syntax:** `CONTAINS {KEY: STRING} IN {MAP} [AS {VAR}]`
- **Description:** Checks if a map contains a specific key.
- **Example:**
    ```bash
    CONTAINS "name" IN {"name": "John", "age": 30} AS $exists
    ```

<br>

**MERGE**

- **Syntax:** `MERGE {MAP} WITH {MAP} [AS {VAR}]`
- **Description:** Creates a new map by merging two maps. Functions the same as adding maps with the `+` operator.
- **Example:**
    ```bash
    MERGE {"name": "John"} WITH {"age": 30} AS $mergedMap
    ```

<br>

**UPDATE**

- **Syntax:** `UPDATE {MAP} WITH {MAP}`
- **Description:** Merges two maps, modifying the original map.
- **Example:**
    ```bash
    SET $map1 AS {"name": "John"}
    SET $map2 AS {"age": 30}
    
    UPDATE $map1 WITH $map2
    PRINT $map1 # {"name": "John", "age": 30}
    ```

<br>

## Conversion

### Type Conversion

**TYPE**

- **Syntax:** `TYPE {ANY} [AS {VAR}]`
- **Description:** Returns the type of the given value.
- **Example:**
    ```bash
    TYPE "Hello" AS $type    # "STRING"
    ```

<br>

**STRING**

- **Syntax:** `STRING {ANY} [AS {VAR}]`
- **Description:** Converts a value to a string.
- **Example:**
    ```bash
    STRING 123 AS $str    # "123"
    ```

<br>

**NUMBER**

- **Syntax:** `NUMBER {ANY} [AS {VAR}]`
- **Description:** Converts a value to an integer. If the value cannot be converted, an error will be thrown.
- **Example:**
    ```bash
    NUMBER "56" AS $num    # 56
    ```

<br>

**FLOAT**

- **Syntax:** `FLOAT {ANY} [AS {VAR}]`
- **Description:** Converts a value to a float. If the value cannot be converted, an error will be thrown.
- **Example:**
    ```bash
    FLOAT "56.25" AS $float    # 56.25
    ```

<br>

**BOOL**

- **Syntax:** `BOOL {ANY} [AS {VAR}]`
- **Description:** Converts a value to a boolean.
- **Example:**
    ```bash
    BOOL "true" AS $bool    # TRUE
    ```

<br>

**ARRAY**

- **Syntax:** `ARRAY {ANY} [AS {VAR}]`
- **Description:** Converts a value to an array.
- **Example:**
    ```bash
    ARRAY {"name": "Martin", "age": 23} AS $array
    ```

<br>

**MAP**

- **Syntax:** `MAP {ANY} [AS {VAR}]`
- **Description:** Converts a value to a map.
- **Example:**
    ```bash
    MAP [["name", "Martin"], ["age", 23]] AS $map
    ```

<br>

### Character and Number Conversion

**CHAR**

- **Syntax:** `CHAR {NUMBER} [AS {VAR}]`

- **Description:** Returns the character corresponding to the input Unicode code for display.

- **Example:**

  ```bash
  CHAR 65 AS $char_value  # A
  ```

<br>

**ORD**

- **Syntax:** `ORD {CHAR} [AS {VAR}]`

- **Description:** Returns the Unicode (UTF8) value for the character.

- **Example:**

  ```bash
  ORD "A" AS $ord_value   # 65
  ```

<br>

**HEX**

- **Syntax:** `HEX {NUMBER} [AS {VAR}]`

- **Description:** Converts a decimal number to its hexadecimal representation.

- **Example:**

  ```bash
  HEX 255 AS $hex_value
  ```

<br>

**BIN**

- **Syntax:** `BIN {NUMBER} [AS {VAR}]`

- **Description:** Converts a decimal number to its binary representation.

- **Example:**

  ```bash
  BIN 10 AS $bin_value
  ```

<br>

**OCT**

- **Syntax:** `OCT {NUMBER} [AS {VAR}]`

- **Description:** Converts a decimal number to its octal representation.

- **Example:**

  ```bash
  OCT 64 AS $oct_value
  ```

<br>

**HEX_TO_DEC**

- **Syntax:** `HEX_TO_DEC {STRING} [AS {VAR}]`

- **Description:** Converts a hexadecimal value to its decimal representation.

- **Example:**

  ```bash
  HEX_TO_DEC "FF" AS $decimal_value
  ```

<br>

**BIN_TO_DEC**

- **Syntax:** `BIN_TO_DEC {STRING} [AS {VAR}]`

- **Description:** Converts a binary value to its decimal representation.

- **Example:**

  ```bash
  BIN_TO_DEC "1010" AS $decimal_value
  ```

<br>

**OCT_TO_DEC**

- **Syntax:** `OCT_TO_DEC {STRING} [AS {VAR}]`

- **Description:** Converts an octal value to its decimal representation.

- **Example:**

  ```bash
  OCT_TO_DEC "100" AS $decimal_value
  ```

<br>

## Mathematical Operations

**SUM**

- **Syntax:** 
    1. `SUM {NUMBER} TO {NUMBER} [AS {VAR}]`
    2. `SUM {ARRAY} [AS {VAR}]`
- **Description:**
    1. Add two numbers.
    2. Add all numbers in an array.
- **Example:**
    ```bash
    SUM 2 TO 3 AS $result          # Returns 5
    SUM [1, 2, 3, 4] AS $result    # Returns 10
    ```

<br>

**SUBTRACT**

- **Syntax:** 
    1. `SUBTRACT {X: NUMBER} WITH {Y: NUMBER} [AS {VAR}]`
    2. `SUBTRACT {ARRAY} [AS {VAR}]`
- **Description:** 
    1. Subtract two numbers.
    2. Subtract all numbers in an array sequentially.
- **Ejemplos**
    ```bash
    SUBTRACT 5 WITH 3 AS $result      # Returns 2
    SUBTRACT [10, 3, 2] AS $result    # Returns 5
    ```

<br>

**MULTIPLY**

- **Syntax:** 
    1. `MULTIPLY {X: NUMBER} BY {Y: NUMBER} [AS {VAR}]`
    2. `MULTIPLY {ARRAY} [AS {VAR}]`
- **Description:**
    1. Multiply two numbers.
    2. Multiply all numbers in an array.
- **Example:**
    ```bash
    MULTIPLY 4 BY 5 AS $result          # Returns 20
    MULTIPLY [1, 2, 3, 4] AS $result    # Returns 24
    ```
    <br>

**DIVIDE**

- **Syntax:** 
    1. Divide two numbers.
    2. Divide all numbers in an array sequentially.
- **Description:** 
    1. Divide dos números.
    2. Divide secuencialmente todos los números en un array.
- **Ejemplo 1:**
    ```bash
    DIVIDE 10 BY 2 AS $result        # Returns 5
    DIVIDE [100, 2, 5] AS $result    # Returns 10
    ```

<br>

**MIN**

- **Syntax:** `MIN {ARRAY} [AS {VAR}]`
- **Description:** Finds the minimum value in an array.
- **Example:**
    ```bash
    MIN [3, 1, 4, 1, 5] AS $min    # Returns 1
    ```

<br>

**MAX**

- **Syntax:** `MAX {ARRAY} [AS {VAR}]`
- **Description:** Finds the maximum value in an array.
- **Example:**
    ```bash
    MAX [3, 1, 4, 1, 5] AS $max    # Returns 5
    ```

<br>

**BOUND**

- **Syntax:** `BOUND {NUMBER} WITH {MIN: NUMBER} AND {MAX: NUMBER} [AS {VAR}]`
- **Description:** Limits a number to a specific range.
- **Example:**
    ```bash
    BOUND 10 WITH 1 AND 5 AS $bounded    # Returns 5
    ```

<br>

**ROUND**

- **Syntax:** `ROUND {NUMBER} [AS {VAR}]`
- **Description:** Rounds a number to the nearest integer.
- **Example:**
    ```bash
    ROUND 3.14 AS $rounded    # Returns 3
    ```

<br>

**ROUND_UP**

- **Syntax:** 
    1. `ROUND_UP {NUMBER} [AS {VAR}]`
    2. `ROUND_UP {NUMBER} TO {DEC} [AS {VAR}]`
- **Description:**
    1. Round a number up to the nearest integer.
    2. Round a number up to a specific number of decimal places.

- **Example:**
    ```bash
    ROUND_UP 3.14 AS $rounded    # Returns 4
    ROUND_UP 23.34121215 TO 3 AS $rounded    # Returns 23.342
    ```

<br>

**ROUND_DOWN**

- **Syntax:** 
    1. `ROUND_DOWN {NUMBER} [AS {VAR}]`
    2. `ROUND_DOWN {NUMBER} TO {DEC} [AS {VAR}]`
- **Description:**
    1. Round a number down to the nearest integer.
    2. Round a number down to a specific number of decimal places.
- **Example:**
    ```bash
    ROUND_DOWN 3.14 AS $rounded    # Returns 3
    ROUND_DOWN 23.34121215 TO 3 AS $rounded    # Returns 23.341
    ```

<br>

**FRAC**

- **Syntax:** `FRAC {NUMBER} [AS {VAR}]`
- **Description:** Returns the decimal part of a number. Ideal for obtaining the exact fraction after the decimal point.
- **Example:**
    ```bash
    FRAC 3.14 AS $frac    # Returns 0.14
    ```

<br>

**FRACTIONAL**

- **Syntax:** `FRACTIONAL {NUMBER} [AS {VAR}]`
- **Description:** Returns the decimal part of a number with greater precision. Ideal for calculations requiring accuracy in the decimal part.
- **Example:**
    ```bash
    FRACTIONAL 3.14 AS $frac    # Returns 0.14000000000000012
    ```

<br>

**POWER / POW**

- **Syntax:** `POWER {BASE: NUMBER} TO {EXPONENT: NUMBER} [AS {VAR}]`
- **Description:** Raises a number to the power of another number.
- **Example:**
    ```bash
    POWER 2 TO 3 AS $result    # Returns 8
    ```

<br>

**SQRT**

- **Syntax:** `SQRT {NUMBER} [AS {VAR}]`
- **Description:** Calculates the square root of a number.
- **Example:**
    ```bash
    SQRT 16 AS $sqrt    # Returns 4.0
    ```

<br>

**MOD**

- **Syntax:** `MOD {DIVIDEND: NUMBER} BY {DIVISOR: NUMBER} [AS {VAR}]`
- **Description:** Calculates the remainder (modulo) of a division.
- **Example:**
    ```bash
    MOD 10 BY 3 AS $mod    # Returns 1
    ```

<br>

**ABS**

- **Syntax:** `ABS {NUMBER} [AS {VAR}]`
- **Description:** Calculates the absolute value of a number.
- **Example:**
    ```bash
    ABS -5 AS $abs    # Returns 5
    ```

<br>

**SIN**

- **Syntax:** `SIN {RADIANS: NUMBER} [AS {VAR}]`
- **Description:** Calculates the sine of an angle in radians.
- **Example:**
    ```bash
    SIN 1.5708 AS $sin    # Returns 0.9999999999932537
    ```

<br>

**COS**

- **Syntax:** `COS {RADIANS: NUMBER} [AS {VAR}]`
- **Description:** Calculates the cosine of an angle in radians.
- **Example:**
    ```bash
    COS 0 AS $cos    # Returns 1.0
    ```

<br>

**TAN**

- **Syntax:** `TAN {RADIANS: NUMBER} [AS {VAR}]`
- **Description:** Calculates the tangent of an angle in radians.
- **Example:**
    ```bash
    TAN 0.7854 AS $tan    # Returns 1.0000036732118496
    ```

<br>

**LOG**

- **Syntax:** `LOG {NUMBER} [AS {VAR}]`
- **Description:** Calculates the natural logarithm of a number.
- **Example:**
    ```bash
    LOG 2.7183 AS $log    # Returns 1
    ```

<br>

**LOGN**

- **Syntax:** `LOGN {BASE: NUMBER} OF {NUMBER: NUMBER} [AS {VAR}]`
- **Description:** Calculates the logarithm of a number with a specific base.
- **Example:**
    ```bash
    LOGN 2 OF 8 AS $logn    # Returns 3
    ```

<br>

**LOG10**

- **Syntax:** `LOG10 {NUMBER} [AS {VAR}]`
- **Description:** Calculates the base 10 logarithm of a number.
- **Example:**
    ```bash
    LOG10 100 AS $log10    # Returns 2
    ```

<br>

**FACTORIAL**

- **Syntax:** `FACTORIAL {NUMBER} [AS {VAR}]`
- **Description:** Calculates the factorial of a number.
- **Example:**
    ```bash
    FACTORIAL 5 AS $fact    # Returns 120
    ```

<br>

**EXP**

- **Syntax:** `EXP {NUMBER} [AS {VAR}]`
- **Description:** Calculates the value of e raised to the power of a number.
- **Example:**
    ```bash
    EXP 1 AS $exp    # Returns 2.7183
    ```

<br>

### Estadísticas

**STAT_MEDIAN**

- **Syntax:** `STAT_MEDIAN {ARRAY} [AS {VAR}]`
- **Description:** Calculates the median of an array of numbers.
- **Example:**
    ```bash
    STAT_MEDIAN [1, 3, 3, 6, 7, 8, 9] AS $median    # Returns 6
    ```

<br>

**STAT_AVERAGE**

- **Syntax:** `STAT_AVERAGE {ARRAY} [AS {VAR}]`
- **Description:** Calculates the average of an array of numbers.
- **Example:**
    ```bash
    STAT_AVERAGE [1, 2, 3, 4, 5] AS $avg    # Returns 3
    ```

<br>

**STAT_VARIANCE**

- **Syntax:** `STAT_VARIANCE {ARRAY} [AS {VAR}]`
- **Description:** Calculates the variance of an array of numbers.
- **Example:**
    ```bash
    STAT_VARIANCE [1, 2, 3, 4, 5] AS $variance    # Returns 2.5
    ```

<br>

**STAT_STDEV**

- **Syntax:** `STAT_STDEV {ARRAY} [AS {VAR}]`
- **Description:** Calculates the standard deviation of an array of numbers.
- **Example:**
    ```bash
    STAT_STDEV [1, 2, 3, 4, 5] AS $stdev    # Returns 1.5811
    ```

<br>

**STAT_MODE**

- **Syntax:** `STAT_MODE {ARRAY} [AS {VAR}]`
- **Description:** Finds the most frequent value in an array.
- **Example:**
    ```bash
    STAT_MODE [1, 2, 2, 3, 4] AS $mode    # Returns 2
    ```
    <br>

## Patterns and Regular Expressions

**PATTERN**

- **Syntax:** 
    1. `PATTERN {STRING} [AS {VAR}]`
    2. `PATTERN {STRING} WITH {FLAGS} [AS {VAR}]`
- **Description:** 
    1. Creates a pattern (regular expression) from a string.
    2. Creates a pattern (regular expression) from a string with an array of flags.

- **Example:**
    ```bash
    PATTERN "[0-9]+" AS $pattern
    PATTERN "[0-9]+" WITH [IGNORECASE, MULTILINE] AS $pattern
    PATTERN "[0-9]+" WITH ["i", "m"] AS $pattern
    ```

<br>

**PATTERN_INFO**

- **Syntax:** `PATTERN_INFO {PATTERN} [AS {VAR}]`
- **Description:** Returns a map with information about the pattern.
- **Example:**
    ```bash
    PATTERN_INFO $pattern AS $details
    ```

<br>

**MATCH**

- **Syntax:** `MATCH {PATTERN} IN {STRING} [AS {VAR}]`
- **Description:** Finds all matches of the pattern in the text, similar to `re.findall` in Python or `match` in JavaScript.
- **Example:**
    ```bash
    MATCH $pattern IN "abc123xyz" AS $matches    # Returns ["123"]
    ```

<br>

**MATCH_FIRST**

- **Syntax:** `MATCH_FIRST {PATTERN} IN {STRING} [AS {VAR}]`
- **Description:** Finds the first match of the pattern in the text.
- **Example:**
    ```bash
    MATCH_FIRST $pattern IN "abc123xyz456" AS $first_match
    ```

<br>

**MATCH_ALL**

- **Syntax:** `MATCH_ALL {PATTERN} IN {STRING} [AS {VAR}]`
- **Description:** Finds all matches of the pattern in the text and returns a map of results with their positions, similar to `re.finditer` in Python.
- **Example:**
    ```bash
    MATCH_ALL $pattern IN "abc123xyz456" AS $all_matches 
    ```

<br>

**MATCH_FULL**

- **Syntax:** `MATCH_FULL {PATTERN} IN {STRING} [AS {VAR}]`
- **Description:** Checks if the entire text matches the pattern.
- **Example:**
    ```bash
    MATCH_FULL $pattern IN "123" AS $is_full_match
    ```

<br>

**COUNT**

- **Syntax:** `COUNT {PATTERN} IN {STRING} [AS {VAR}]`
- **Description:** Counts how many times the pattern matches in the text.
- **Example:**
    ```bash
    PATTERN "\d+" AS $pattern
    COUNT $pattern IN "abc123xyz456" AS $count    # Returns 2
    ```

<br>

**SPLIT**

- **Syntax:** `SPLIT {STRING} BY {PATTERN} [AS {VAR}]`
- **Description:** Splits a string using a pattern as a delimiter.
- **Example:**
    ```bash
    PATTERN "\d+" AS $pattern 
    SPLIT "hello45world77world22hello" BY $pattern AS $parts    # Returns ["hello", "world", "world", "hello"]
    ```

<br>

**FIND**

- **Syntax:** `FIND {PATTERN} IN {STRING} [AS {VAR}]`
- **Description:** Finds the position of the first match of the pattern in the string.
- **Example:**
    ```bash
    PATTERN "\d+" AS $pattern 
    FIND $pattern IN "abc123xyz" AS $position    # Returns 4
    ```

<br>

**REPLACE**

- **Syntax:** `REPLACE {PATTERN} WITH {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Replaces the first match of the pattern in the text with another string.
- **Example:**
    ```bash
    PATTERN "[0-9]+" AS $pattern
    REPLACE $pattern WITH "X" IN "abc123xyz456" AS $new_text    # Returns abcXxyz456
    ```
    <br>

**REPLACE_ALL**

- **Syntax:** `REPLACE_ALL {PATTERN} WITH {SUBSTR} IN {STRING} [AS {VAR}]`
- **Description:** Replaces all matches of the pattern in the text with another string.
- **Example:**
    ```bash
    PATTERN "[0-9]+" AS $pattern
    REPLACE_ALL $pattern WITH "X" IN "abc123xyz456" AS $new_text    # Returns abcXxyzX
    ```

<br>


## Files and Directories

### File Manipulation

**FILE_EXISTS**

- **Syntax:** `FILE_EXISTS {FILE: STRING} [AS {VAR}]`
- **Description:** Checks if a file exists in the specified path.
- **Example:**
    ```bash
    FILE_EXISTS "example.txt" AS $exists
    ```

<br>

**FILE_DELETE**

- **Syntax:** `FILE_DELETE {FILE: STRING}`
- **Description:** Deletes a specified file.
- **Example:**
    ```bash
    FILE_DELETE "example.txt"
    ```

<br>

**FILE_MOVE**

- **Syntax:** `FILE_MOVE {FILE: STRING} TO {DEST}`
- **Description:** Moves a file (`FILE`) to a new location (`DEST`).
- **Example:**
    ```bash
    FILE_MOVE "example.txt" TO "new_folder/example.txt"
    ```

<br>

**FILE_RENAME**

- **Syntax:** `FILE_RENAME {OLD: STRING} TO {NEW: STRING}`
- **Description:** Renames a specified file.
- **Example:**
    ```bash
    FILE_RENAME "old_name.txt" TO "new_name.txt"
    ```

<br>

**FILE_SIZE**

- **Syntax:** `FILE_SIZE {FILE: STRING} [AS {VAR}]`
- **Description:** Gets the size of a file in bytes.
- **Example:**
    ```bash
    FILE_SIZE "example.txt" AS $size
    ```

<br>

**FILE_OPEN**

- **Syntax:** `FILE_OPEN {FILE: STRING}`
- **Description:** Opens a file for reading or writing, returning the file ID.
- **Example:**
    ```bash
    FILE_OPEN "example.txt" AS $file_id
    ```

<br>

**FILE_WRITE**

- **Syntax:** `FILE_WRITE {ID: NUMBER} WITH {STRING}`
- **Description:** Writes text to an open file.
- **Example:**
    ```bash
    FILE_WRITE $file_id WITH "Hello, world!"
    ```

<br>

**FILE_READ**

- **Syntax:** `FILE_READ {ID: NUMBER} [AS {VAR}]`
- **Description:** Reads the content of an open file.
- **Example:**
    ```bash
    FILE_READ $file_id AS $content
    ```

<br>

**FILE_APPEND**

- **Syntax:** `FILE_APPEND {ID: NUMBER} WITH {STRING}`
- **Description:** Appends text to the end of an open file.
- **Example:**
    ```bash
    FILE_APPEND $file_id WITH "More text."
    ```

<br>

**FILE_CLOSE**

- **Syntax:** `FILE_CLOSE {ID: NUMBER}`
- **Description:** Closes an open file.
- **Example:**
    ```bash
    FILE_CLOSE $file_id
    ```

<br>

### Directory Manipulation

**DIRECTORY_CREATE**

- **Syntax:** `DIRECTORY_CREATE {DIR: STRING}`
- **Description:** Creates a new directory.
- **Example:**
    ```bash
    DIRECTORY_CREATE "new_folder"
    ```

<br>

**DIRECTORY_DELETE**

- **Syntax:** `DIRECTORY_DELETE {DIR: STRING}`
- **Description:** Deletes a specified directory.
- **Example:**
    ```bash
    DIRECTORY_DELETE "old_folder"
    ```

<br>

**DIRECTORY_RENAME**

- **Syntax:** `DIRECTORY_RENAME {OLD: STRING} TO {NEW: STRING}`
- **Description:** Renames a specified directory.
- **Example:**
    ```bash
    DIRECTORY_RENAME "old_folder" TO "new_folder"
    ```

<br>

**DIRECTORY_EXISTS**

- **Syntax:** `DIRECTORY_EXISTS {DIR: STRING} [AS {VAR}]`
- **Description:** Checks if a directory exists in the specified path.
- **Example:**
    ```bash
    DIRECTORY_EXISTS "example_folder" AS $exists
    ```

<br>

**DIRECTORY_COPY**

- **Syntax:** `DIRECTORY_COPY {DIR: STRING} TO {DEST}`
- **Description:** Copies a directory to a new location.
- **Example:**
    ```bash
    DIRECTORY_COPY "example_folder" TO "backup_folder"
    ```

<br>

**DIRECTORY_MOVE**

- **Syntax:** `DIRECTORY_MOVE {DIR: STRING} TO {DESC}`
- **Description:** Moves a directory to a new location.
- **Example:**
    ```bash
    DIRECTORY_MOVE "example_folder" TO "new_location"
    ```

<br>

**DIRECTORY_SIZE**

- **Syntax:** `DIRECTORY_SIZE {DIR: STRING} [AS {VAR}]`
- **Description:** Gets the total size of a directory and its contents.
- **Example:**
    ```bash
    DIRECTORY_SIZE "example_folder" AS $size
    ```

<br>

**DIRECTORY_LIST**

- **Syntax:** `DIRECTORY_LIST {DIR: STRING} [AS {VAR}]`
- **Description:** Lists all files and directories in a directory.
- **Example:**
    ```bash
    DIRECTORY_LIST "example_folder" AS $items
    ```

<br>

**DIRECTORY_LIST_FILES**

- **Syntax:** `DIRECTORY_LIST_FILES {DIR: STRING} [AS {VAR}]`
- **Description:** Lists only the files in a directory.
- **Example:**
    ```bash
    DIRECTORY_LIST_FILES "example_folder" AS $files
    ```

<br>

**DIRECTORY_LIST_DIRS**

- **Syntax:** `DIRECTORY_LIST_DIRS {DIR: STRING} [AS {VAR}]`
- **Description:** Lists only the subdirectories in a directory.
- **Example:**
    ```bash
    DIRECTORY_LIST_DIRS "example_folder" AS $dirs
    ```

<br>

### File Path Manipulation

**PATH_FILENAME**

- **Syntax:** `PATH_FILENAME {PATH: STRING} [AS {VAR}]`
- **Description:** Gets the filename from a full path.
- **Example:**
    ```bash
    PATH_FILENAME "/path/to/file.txt" AS $filename  # file.txt
    ```

<br>

**PATH_EXT**

- **Syntax:** `PATH_EXT {PATH: STRING} [AS {VAR}]`
- **Description:** Gets the file extension from a full path.
- **Example:**
    ```bash
    PATH_EXT "/path/to/file.txt" AS $extension  # .txt
    ```

<br>

**PATH_DIR**

- **Syntax:** `PATH_DIR {PATH: STRING} [AS {VAR}]`
- **Description:** Gets the directory from a full path.
- **Example:**
    ```bash
    PATH_DIR "/path/to/file.txt" AS $directory  # /path/to
    ```

<br>

**PATH_JOIN**

- **Syntax:** `PATH_JOIN {ARRAY} [AS {VAR}]`
- **Description:** Combines path elements into a single string.
- **Example:**
    ```bash
    PATH_JOIN ["/path", "to", "file.txt"] AS $full_path # path/to/file.txt
    ```

<br>

**PATH_ABSOLUTE**

- **Syntax:** `PATH_ABSOLUTE {PATH: STRING} [AS {VAR}]`
- **Description:** Converts a relative path into an absolute path.
- **Example:**
    ```bash
    PATH_ABSOLUTE "relative/path" AS $absolute_path # C:\Users\PC\example\relative\path
    ```

<br>

**PATH_RELATIVE**

- **Syntax:** 
    1. `PATH_RELATIVE {PATH: STRING} [AS {VAR}]`
    2. `PATH_RELATIVE {PATH: STRING} FROM {STARTDIR: STRING} [AS {VAR}]`
- **Description:** 
    1. Converts an absolute path to a relative path.
    2. Converts an absolute path to a relative path based on a specific starting directory.
- **Example:**
    ```bash
    PATH_RELATIVE "/absolute/path" AS $relative_path
    PATH_RELATIVE "/absolute/path" FROM "/start/dir" AS $relative_path
    ```

<br>

**PATH_PARENT**

- **Syntax:** `PATH_PARENT {PATH: STRING} [AS {VAR}]`
- **Description:** Obtains the parent directory of a specified path.
- **Example:**
    ```bash
    PATH_PARENT "/path/to/file.txt" AS $parent_dir
    ```

<br>

**PATH_NORMALIZE**

- **Syntax:** `PATH_NORMALIZE {PATH: STRING} [AS {VAR}]`
- **Description:** Normalizes a file path by removing redundant components.
- **Example:**
    ```bash
    PATH_NORMALIZE "/path/./to/../file.txt" AS $normalized_path
    ```

<br>

**PATH_SPLIT**

- **Syntax:** `PATH_SPLIT {PATH: STRING} [AS {VAR}]`
- **Description:** Splits a file path into components (filename, directory, extension).
- **Example:**
    ```bash
    PATH_SPLIT "/path/to/file.txt" AS $components   # ["file", "/path/to", ".txt"]
    ```

<br>

### JSON Manipulation

**JSON_PARSE**

- **Syntax:** `JSON_PARSE {STRING} [AS {VAR}]`
- **Description:** Converts a string (in JSON format) into a map.
- **Example:**
    ```bash
    JSON_PARSE '{"key": "value"}' AS $parsed_map
    PRINTF $parsed_map
    ```

<br>

**JSON_TO_STRING**

- **Syntax:** `JSON_TO_STRING {MAP} [AS {VAR}]`
- **Description:** Converts a map into a string in JSON format.
- **Example:**
    ```bash
    JSON_TO_STRING {"key": "value"} AS $json_string
    ```

<br>

**JSON_TO_PRETTY**

- **Syntax:** `JSON_TO_PRETTY {MAP} WITH {SPACES: STRING} [AS {VAR}]`
- **Description:** Converts a map into a string in JSON format with defined spacing.
- **Example:**
    ```bash
    JSON_TO_PRETTY {"key": "value"} WITH 4 AS $json_string
    ```

<br>

**JSON_LOAD**

- **Syntax:** `JSON_LOAD {FILE: STRING} [AS {VAR}]`
- **Description:** Loads a JSON file and converts it into a map.
- **Example:**
    ```bash
    JSON_LOAD "data.json" AS $data_map
    ```

<br>

**JSON_SAVE**

- **Syntax:** `JSON_SAVE {MAP} TO {FILE: STRING}`
- **Description:** Saves a map into a file in JSON format.
- **Example:**
    ```bash
    JSON_SAVE {"key": "value"} TO "output.json"
    ```
    

<br>

## Random Values

**RANDOM**

- **Syntax:**
    1. `RANDOM {N: STRING} [AS {VAR}]`
    2. `RANDOM FROM {X: NUMBER} TO {Y: NUMBER} [AS {VAR}]`
- **Description:**
    1. Generates a random number between 0 and the specified value.
    2. Generates a random number within the specified range between `X` and `Y`.
- **Example:**
    ```bash
    RANDOM 100 AS $random_number
    RANDOM FROM 1 TO 10 AS $random_range
    ```

<br>


**RANDOM_CHOICE**

- **Syntax:** `RANDOM_CHOICE {ARRAY} [AS {VAR}]`
- **Description:** Selects a random element from an array.
- **Example:**
    ```bash
    RANDOM_CHOICE [1, 2, 3, 4] AS $random_element
    ```


<br>

## Control Flow

### Conditionals

**WHEN**

- **Syntax:** `WHEN {CONDITION: ANY}`

- **Description:** Executes a block of code if the specified condition is true.

- **Example:**

  ```bash
  WHEN $count > 5
      PRINT "The value is greater than 5"
  ```

<br>

**ORWHEN**

- **Syntax:** `ORWHEN {CONDITION: ANY}`

- **Description:** Used to add an alternative condition in a `WHEN` control structure, so if any of the specified conditions are true, the corresponding block of code will be executed.

- **Example:**

  ```bash
  WHEN $count > 10
      PRINT "The value is greater than 10"
  ORWHEN $count == 10
      PRINT "The value is exactly  10"
  ```

<br>

**AND**

- **Syntax:** `AND {CONDITION: ANY}`

- **Description:** Used together with `WHEN` or `ORWHEN` to combine multiple conditions with `AND` logic.

- **Example:**

  ```bash
  WHEN $count > 5
  AND $count < 10
      PRINT "The value is between 5 and 10"
  ```

<br>

**OR**

- **Syntax:** `OR {CONDITION: ANY}`

- **Description:** Used together with `WHEN` or `ORWHEN` to combine multiple conditions with `OR` logic.

- **Example:**

  ```bash
  WHEN $count < 5
  OR $count > 10
      PRINT "The value is outside the range of 5 to 10"
  ```

<br>

**ELSE**

- **Syntax:** `ELSE`

- **Description:** Used to specify a block of code that will execute if none of the previous conditions with `WHEN` or `ORWHEN` are true.

- **Example:**

  ```bash
  WHEN $count > 10
      PRINT "The value is greater than 10"
  ELSE
      PRINT "The value is 10 or less"
  ```

<br>



### Loops

**WHILE**

- **Syntax:** `WHILE {CONDITION: ANY}`
- **Description:** Executes a block of code while the specified condition is true.
- **Example:**
    ```bash
    WHILE $count < 10
        PRINT $count
        INCREMENT $count
    ```

<br>

**ITERATE**

- **Syntax:** 
    1. `ITERATE {STRING|ARRAY} [AS {VAR}]`
    2. `ITERATE FROM {X: NUMBER} TO {Y: NUMBER} [AS {VAR}]`
- **Description:** Iterates over the elements of a string or array, or over a range of indices. For arrays and strings, the current value is stored in the specified variable.
- **Example:**
    ```bash
    ITERATE $array AS $element
        PRINT $element
    
    ITERATE $array AS [$index, $element]
        PRINT "" + $index + ". " + $element
    ```

    ```bash
    ITERATE FROM 1 TO 5 AS $index
        PRINT $index
    ```

<br>

**REPEAT**

- **Syntax:** `REPEAT {NUMBER}`
- **Description:** Executes a block of code a specified number of times.
- **Example:**
    ```bash
    REPEAT 5
        PRINT "Hello World"
    ```

<br>

**BREAK**

- **Syntax:** `BREAK`
- **Description:** Exits the loop or `ITERATE` or `WHILE` block it is in.
- **Example:**
    ```bash
    WHILE $count < 10
        PRINT $count
        WHEN $count == 5
            BREAK
        INCREMENT $count
    ```

<br>

**CONTINUE**

- **Syntax:** `CONTINUE`
- **Description:** Skips to the next iteration of the loop or `ITERATE` or `WHILE` block.
- **Example:**
    ```bash
    WHILE $count < 10
        WHEN $count % 2 == 0
            INCREMENT $count
            CONTINUE
        PRINT $count
        INCREMENT $count
    ```

<br>

### Definitions

**DEFINE**

- **Syntax:** `DEFINE {NAME: STRING}`

- **Description:** Defines a custom command with the specified name.

- **Example:**

  ```bash
  DEFINE GREET
      PRINT "Hello, World!"
  ```

<br>

**PARAMS**

- **Syntax:** `PARAMS {ARRAY}`

- **Description:** Specifies the parameters that the `DEFINE` command will receive. Parameters must be in an array.

- **Example:**

  ```bash
  DEFINE GREET
      PARAMS [STRING, NUMBER]
      PRINT "Hello, " + @PARAMS.1 + ". You have " + @PARAMS.2 +" new messages."
  
  GREET "Martin" 32   # Hello, Martin. You have 32 new messages.
  ```

<br>

**RETURN**

- **Syntax:** `RETURN {ANY}`

- **Description:** Returns a value from a command defined with `DEFINE`.

- **Example:**

  ```bash
  DEFINE ADD
      PARAMS [NUMBER, NUMBER]
      SET $result AS @PARAMS.1 + @PARAMS.2
      RETURN $result
  ```

<br>

**ON**

- **Syntax:** `ON {EVENT}`

- **Description:** Executes a block of code on a specific event. Visit the [Events](Guide_EN.md#events) section for more information.

- **Example:**

  ```bash
  ON START
      PRINT "Hello"
  
  ON END
      PRINT "Bye"
  ```

<br>

### Modules

**IMPORT**

- **Syntax:** `IMPORT {FILE: STRING}`
- **Description:** Imports a file containing command definitions, variables, or data that can be used in the current code. This command allows for code reuse and modularization in your project. You can also import libraries.
- **Example:**
    ```bash
    IMPORT "myfunctions.stal"
    IMPORT "myfunctions"    # The extension can be omitted
    ```

<br>

**EXPORT**

- **Syntax:** 
    1. `EXPORT {VAR}`
    3. `EXPORT {CMD: STRING}`
    
- **Description:** Exports a variable or command (as a string) so that they can be used in other files or contexts. This command is useful for sharing data or commands between different parts of the project or with libraries.

- **Example:**
    ```bash
    EXPORT $myVariable      # Export a variable
    EXPORT "MYFUNCTION"     # Export a command
    ```
<br>

### Error Handling

**TRY**

- **Syntax:** `TRY`
- **Description:** Starts a block of code that may throw errors, which will be handled with `CATCH`.
- **Example:**
    ```bash
    TRY
        PRINT 1 / 0
    CATCH
        # ...
    ```

<br>

**CATCH**

- **Syntax:** `CATCH [AS {VAR}]`
- **Description:** Catches and handles errors thrown in the `TRY` block. The captured error is stored in the specified variable.
- **Example:**
    ```bash
    TRY
        PRINT 1 / 0
    CATCH AS $error
        PRINTF $error
    ```

<br>

**ERROR**

- **Syntax:** `ERROR {STRING}`
- **Description:** Throws a generic error.
- **Example:**
    ```bash
    SET $n AS 0
    WHEN $n IS LEQ 0
        ERROR "The number must be greater than 0."
    ```

<br>

**THROW**

- **Syntax:** `THROW {ARRAY}`
- **Description:** Throws a custom error. Usually used with commands like `SYNTAX_ERROR`, `VALUE_ERROR`, `RUNTIME_ERROR`, etc.
- **Example:**
    ```bash
    THROW VALUE_ERROR ["The value is invalid."];
    ```

<br>

**SYNTAX_ERROR** — **VALUE_ERROR** — **TYPE_ERROR** — **RUNTIME_ERROR** — **NOT_FOUND_ERROR** — **FILESYSTEM_ERROR**

- **Syntax:** `COMMAND {ARRAY}`
- **Description:** Receives an array as a parameter, where the first element represents the error description and the second index is the error name. Returns a map with an error of type `SyntaxError`, `ValueError`, `TypeError`, `RuntimeError`, `NotFoundError`, and `FilesystemError`, respectively. 
- **Example:**
    ```bash
    THROW SYNTAX_ERROR ["Invalid Syntax"];
    THROW VALUE_ERROR ["Invalid value"];
    THROW TYPE_ERROR ["Unsupported Type"];
    THROW RUNTIME_ERROR ["Missing values", "MISSING_PARAMETERS"];
    ```
    <br>

## Input/Output

### Content 

**INPUT**

- **Syntax:**
    1. `INPUT {STRING}`
    2. `INPUT FROM {FILENAME}`
- **Description:**  Loads an input string from a string or a file.
- **Example:**
    ```bash
    INPUT "Hello, world!"
    INPUT FROM "data.txt"
    ```

<br>

**STORE**

- **Syntax:** `STORE {STRING}`
- **Description:** Stores a string as a line, to later be exported as a file using the `OUTPUT` command.
- **Example:**
    ```bash
    STORE "Este es el contenido a guardar."
    ```

<br>

**CLEAR_STORAGE**

- **Syntax:** `CLEAR_STORAGE`
- **Description:** Empties the global line storage, removing all stored strings and resetting the storage to an empty state.
- **Example:**
    ```bash
    CLEAR_STORAGE
    ```

<br>

**OUTPUT**

- **Syntax:** `OUTPUT {FILE: STRING}`
- **Description:** Exports all stored content to a file with the specified name.
- **Example:**
    ```bash
    INPUT "file.txt"
    
    ON EACH_LINE
        STORE @LINE
    
    OUTPUT "result.txt"
    ```

<br>



### Misc

**PRINT**

- **Syntax:** `PRINT {ANY}`
- **Description:** Prints the value of the specified variable or text to standard output.
- **Example:**
    ```bash
    PRINT "Hello, world!"
    ```

<br>

**PRINTF**

- **Syntax:** `PRINTF {ANY}`
- **Description:** Prints the specified value with formatting, providing a more structured and readable presentation.
- **Example:**
    ```bash
    PRINTF {"name": "John", "age": 30}
    ```

<br>

**PRINTL**

- **Syntax:** `PRINTL {ARRAY}`
- **Description:** Prints the elements of an array on a single line, separated by spaces.
- **Example:**
    ```bash
    PRINTL ["hola", "como", "estas"]  # Output: hola como estas
    ```



<br>

## Dates and Times

### Generar y Formatear Fechas

**DATE_CREATE**

- **Syntax:** `DATE_CREATE {STRING|ARRAY} [AS {VAR}]`
- **Description:** Creates a new date from a string or array. Returns a date map.
    
    If an array is used, it must contain 7 elements: year, month, day, hour, minute, second, and microsecond.

    If a string is used, it must be structured in a valid format such as: `YYYY-MM-DD`, `MM-DD-YYYY`, `MM/DD/YYYY`, `YYYY-MM-DD HH:mm:ss`, etc.
    
- **Example:**
    ```bash
    DATE_CREATE "2020-10-10"
    DATE_CREATE [2024, 8, 12, 12, 0, 0, 0] AS $date
    ```

<br>

**DATE_NOW**

- **Syntax:** `DATE_NOW [AS {VAR}]`
- **Description:** Obtains the current date and time. Returns a date map.
- **Example:**
    ```bash
    DATE_NOW AS $current_date
    ```

<br>

**DATE_FORMAT**

- **Syntax:** 
    1. `DATE_FORMAT {DATE: MAP} [AS {VAR}]`
    2. `DATE_FORMAT {DATE: MAP} TO {FORMAT} [AS {VAR}]`
- **Description:** Formats a date map according to the global format or a specified format.
- **Example:**
    ```bash
    DATE_CREATE "2024-06-03" AS $date
    DATE_FORMAT $date # Formato global
    DATE_FORMAT $date TO "MM/DD/YYYY" AS $formatted_date 
    ```

<br>

**DATE_FORMAT_NOW**

- **Syntax:** `DATE_FORMAT_NOW [AS {VAR}]`
- **Description:** Formats the current date and time according to the global format.
- **Example:**
    ```bash
    DATE_FORMAT_NOW AS $formatted_now
    ```

<br>


### Comparaciones y Verificaciones

**IS_DATE**

- **Syntax:** `IS_DATE {ANY} [AS {VAR}]`
- **Description:** Verifies if the specified value is a valid date map.
- **Example:**
    ```bash
    IS_DATE $date AS $is_valid_date
    ```

<br>

**DATE_IS_LEAP_YEAR**

- **Syntax:** `DATE_IS_LEAP_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Checks if the specified year is a leap year.
- **Example:**
    ```bash
    DATE_IS_LEAP_YEAR $date
    ```

<br>

**DATE_COMPARE**

- **Syntax:** `DATE_COMPARE {DATE: MAP} AND {DATE: MAP} [AS {VAR}]`
- **Description:** Compares two dates and returns 1 if the first is greater, -1 if it is lesser, and 0 if they are equal.
- **Example:**
    ```bash
    DATE_COMPARE $date1 AND $date2
    ```

<br>

**DATE_IS_BEFORE**

- **Syntax:** `DATE_IS_BEFORE {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Verifies if `DATE1` is earlier than `DATE2`.
- **Example:**
    ```bash
    DATE_IS_BEFORE $date1 AND $date2
    ```

<br>

**DATE_IS_AFTER**

- **Syntax:** `DATE_IS_AFTER {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Verifies if `DATE1` is later than `DATE2`.
- **Example:**
    ```bash
    DATE_IS_AFTER $date1 AND $date2
    ```

<br>

**DATE_IS_SAME**

- **Syntax:** `DATE_IS_SAME {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Verifies if `DATE1` and `DATE2` are equal.
- **Example:**
    ```bash
    DATE_IS_SAME $date1 AND $date2
    ```

<br>

**DATE_SINCE**

- **Syntax:** `DATE_SINCE {DATE: MAP} [AS {VAR}]`

- **Description:** Obtains the elapsed time from the specified date until now.

- **Example:**

  ```bash
  DATE_CREATE "2024-06-03" AS $date
  DATE_SINCE $date AS $elapsed_time    # 2 months ago 
  ```

<br>

**DATE_DISTANCE**

- **Syntax:** 

  1. `DATE_DISTANCE {DATE: MAP} [AS {VAR}]`
  2. `DATE_DISTANCE {DATE: MAP} TO {DATE: MAP} [AS {VAR}]`

- **Description:**

    1. Gets the elapsed time distance from the specified date to now.
    2. Gets the time distance between two dates.

- **Example:**

  ```bash
  DATE_DISTANCE $date1 AS $distance    # 1 year
  DATE_DISTANCE $date1 TO $date2 AS $time_distance    # 4 years
  ```

<br>

### Diferencias de Fechas

**DATE_DIFF_YEARS**

- **Syntax:** `DATE_DIFF_YEARS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in years between two dates.
- **Example:**
    ```bash
    DATE_DIFF_YEARS $date1 AND $date2
    ```

<br>

**DATE_DIFF_MONTHS**

- **Syntax:** `DATE_DIFF_MONTHS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in months between two dates.
- **Example:**
    ```bash
    DATE_DIFF_MONTHS $date1 AND $date2
    ```

<br>

**DATE_DIFF_WEEKS**

- **Syntax:** `DATE_DIFF_WEEKS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in weeks between two dates.
- **Example:**
    ```bash
    DATE_DIFF_WEEKS $date1 AND $date2
    ```

<br>

**DATE_DIFF_DAYS**

- **Syntax:** `DATE_DIFF_DAYS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in days between two dates.
- **Example:**
    ```bash
    DATE_DIFF_DAYS $date1 AND $date2
    ```

<br>

**DATE_DIFF_HOURS**

- **Syntax:** `DATE_DIFF_HOURS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in hours between two dates.
- **Example:**
    ```bash
    DATE_CREATE "2024-01-01T12:00:00" AS $date1
    DATE_CREATE "2020-01-01T12:00:00" AS $date2
    DATE_DIFF_HOURS $date1 AND $date2 AS $diff
    ```

<br>

**DATE_DIFF_MINUTES**

- **Syntax:** `DATE_DIFF_MINUTES {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in minutes between two dates.
- **Example:**
    ```bash
    DATE_DIFF_MINUTES $date1 AND $date2
    ```

<br>

**DATE_DIFF_SECONDS**

- **Syntax:** `DATE_DIFF_SECONDS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in seconds between two dates.
- **Example:**
    ```bash
    DATE_DIFF_SECONDS $date1 AND $date2
    ```

<br>

**DATE_DIFF_MICROSECONDS**

- **Syntax:** `DATE_DIFF_MICROSECONDS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Description:** Calculates the difference in microseconds between two dates.
- **Example:**
    ```bash
    DATE_DIFF_MICROSECONDS $date1 AND $date2
    ```

<br>

### Componentes de Fechas

**DATE_GET_YEAR**

- **Syntax:** `DATE_GET_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the year from the specified date.
- **Example:**
    ```bash
    DATE_GET_YEAR $date AS $year
    ```

<br>

**DATE_GET_MONTH**

- **Syntax:** `DATE_GET_MONTH {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the month from the specified date.
- **Example:**
    ```bash
    DATE_GET_MONTH $date AS $month
    ```

<br>

**DATE_GET_WEEK**

- **Syntax:** `DATE_GET_WEEK {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the week of the year from the specified date.
- **Example:**
    ```bash
    DATE_GET_WEEK $date AS $week
    ```

<br>

**DATE_GET_DAY**

- **Syntax:** `DATE_GET_DAY {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the day of the month from the specified date.
- **Example:**
    ```bash
    DATE_GET_DAY $date AS $day
    ```

<br>

**DATE_GET_HOUR**

- **Syntax:** `DATE_GET_HOUR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the hour from the specified date.
- **Example:**
    ```bash
    DATE_GET_HOUR $date AS $hour
    ```

<br>

**DATE_GET_MINUTE**

- **Syntax:** `DATE_GET_MINUTE {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the minute from the specified date.
- **Example:**
    ```bash
    DATE_GET_MINUTE $date AS $minute
    ```

<br>

**DATE_GET_SECOND**

- **Syntax:** `DATE_GET_SECOND {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the second from the specified date.
- **Example:**
    ```bash
    DATE_GET_SECOND $date" AS $second
    ```

<br>

**DATE_GET_MICROSECOND**

- **Syntax:** `DATE_GET_MICROSECOND {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the microsecond from the specified date.
- **Example:**
    ```bash
    DATE_GET_MICROSECOND $date AS $microsecond
    ```

<br>

**DATE_GET_WEEKDAY**

- **Syntax:** `DATE_GET_WEEKDAY {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the day of the week number from the specified date (1 to 7).
- **Example:**
    ```bash
    DATE_NOW AS $date
    DATE_GET_WEEKDAY $date AS $weekday  # 5
    ```

<br>

**DATE_GET_WEEKDAY_NAME**

- **Syntax:** `DATE_GET_WEEKDAY_NAME {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the day of the week name from the specified date.
- **Example:**
    ```bash
    DATE_NOW AS $date
    DATE_GET_WEEKDAY_NAME $date AS $weekday_name    # Friday
    ```

<br>

**DATE_GET_DAYS_IN_MONTH**

- **Syntax:** `DATE_GET_DAYS_IN_MONTH {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the number of days in the month of the specified date.
- **Example:**
    ```bash
    DATE_GET_DAYS_IN_MONTH $date AS $days_in_month  # 31
    ```

<br>

**DATE_GET_DAYS_IN_YEAR**

- **Syntax:** `DATE_GET_DAYS_IN_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the number of days in the year of the specified date.
- **Example:**
    ```bash
    DATE_GET_DAYS_IN_YEAR $date AS $days_in_year    # 365 or 366
    ```

<br>

**DATE_GET_TIMESTAMP**

- **Syntax:** `DATE_GET_TIMESTAMP {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the Unix timestamp of the specified date.
- **Example:**
    ```bash
    DATE_GET_TIMESTAMP $date AS $timestamp
    ```

<br>

### Contexto Anual

**DATE_GET_WEEK_OF_YEAR**

- **Syntax:** `DATE_GET_WEEK_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the week number of the year for the specified date.
- **Example:**
    ```bash
    DATE_GET_WEEK_OF_YEAR $date AS $week_of_year
    ```

<br>

**DATE_GET_DAY_OF_YEAR**

- **Syntax:** `DATE_GET_DAY_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the day number of the year for the specified date.
- **Example:**
    ```bash
    DATE_GET_DAY_OF_YEAR $date AS $day_of_year
    ```

<br>

**DATE_GET_HOUR_OF_YEAR**

- **Syntax:** `DATE_GET_HOUR_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the hour of the year for the specified date.
- **Example:**
    ```bash
    DATE_GET_HOUR_OF_YEAR $date AS $hour_of_year
    ```

<br>

**DATE_GET_MINUTE_OF_YEAR**

- **Syntax:** `DATE_GET_MINUTE_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the minute of the year for the specified date.
- **Example:**
    ```bash
    DATE_GET_MINUTE_OF_YEAR $date AS $minute_of_year
    ```

<br>

**DATE_GET_SECOND_OF_YEAR**

- **Syntax:** `DATE_GET_SECOND_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the second of the year for the specified date.
- **Example:**
    ```bash
    DATE_GET_SECOND_OF_YEAR $date AS $second_of_year
    ```

<br>

**DATE_GET_MICROSECOND_OF_YEAR**

- **Syntax:** `DATE_GET_MICROSECOND_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Description:** Gets the microsecond of the year for the specified date.
- **Example:**
    ```bash
    DATE_GET_MICROSECOND_OF_YEAR $date AS $microsecond_of_year
    ```

<br>

### Modificadores

**DATE_INCREMENT_YEAR**

- **Syntax:** `DATE_INCREMENT_YEAR {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the year of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_YEAR $date WITH 5 AS $new_date
    ```

<br>

**DATE_INCREMENT_MONTH**

- **Syntax:** `DATE_INCREMENT_MONTH {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the month of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_MONTH $date WITH -3 AS $new_date
    ```

<br>

**DATE_INCREMENT_WEEK**

- **Syntax:** `DATE_INCREMENT_WEEK {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the week of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_WEEK $date WITH -3 AS $new_date
    ```

<br>

**DATE_INCREMENT_DAY**

- **Syntax:** `DATE_INCREMENT_DAY {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the day of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_DAY $date WITH 10 AS $new_date
    ```

<br>

**DATE_INCREMENT_HOUR**

- **Syntax:** `DATE_INCREMENT_HOUR {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the hour of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_HOUR $date WITH 5 AS $new_date
    ```

<br>

**DATE_INCREMENT_MINUTE**

- **Syntax:** `DATE_INCREMENT_MINUTE {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the minute of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_MINUTE $date WITH 30 AS $new_date
    ```

<br>

**DATE_INCREMENT_SECOND**

- **Syntax:** `DATE_INCREMENT_SECOND {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the second of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_SECOND $date WITH 15 AS $new_date
    ```

<br>

**DATE_INCREMENT_MICROSECOND**

- **Syntax:** `DATE_INCREMENT_MICROSECOND {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Description:** Increases the microsecond of the specified date by the given number. Returns a new date map.
- **Example:**
    ```bash
    DATE_INCREMENT_MICROSECOND $date WITH 500 AS $new_date
    ```

<br>

### Configuración Global de Fechas


**DATE_SET_LOCALE**

- **Syntax:** `DATE_SET_LOCALE {LOCAL: STRING}`
- **Description:** Sets the locale. Receives a language code as a parameter.
- **Example:**
    ```bash
    DATE_GET_WEEKDAY_NAME $result # Friday
    DATE_SET_LOCALE "es_ES"
    DATE_GET_WEEKDAY_NAME $result2 # viernes
    ```

<br>

**DATE_SET_TIMEZONE**

- **Syntax:** `DATE_SET_TIMEZONE {TIMEZONE: STRING}`
- **Description:** Sets the time zone.
- **Example:**
    ```bash
    DATE_SET_TIMEZONE "Europe/Berlin"
    DATE_SET_TIMEZONE "LOCAL"
    ```

<br>

**DATE_SET_FORMAT**

- **Syntax:** `DATE_SET_FORMAT {FORMAT: STRING} AS {format}`
- **Description:** Sets the global format.
- **Example:**
    ```bash
    DATE_SET_FORMAT "YYYY-MM-DD"
    PRINT DATE_FORMAT_NOW;
    ```

<br>

**DATE_GET_LOCALE**

- **Syntax:** `DATE_GET_LOCALE [AS {VAR}]`
- **Description:** Returns the locale code for date formatting, such as `en-us`.
- **Example:**
    ```bash
    DATE_GET_LOCALE AS $locale # en-us
    ```

<br>

**DATE_GET_TIMEZONE**

- **Syntax:** `DATE_GET_TIMEZONE [AS {VAR}]`
- **Description:** Returns the set time zone.
- **Example:**
    ```bash
    PRINT DATE_GET_TIMEZONE;
    ```

<br>

**DATE_GET_FORMAT**

- **Syntax:** `DATE_GET_FORMAT [AS {VAR}]`
- **Description:** Returns the global format.
- **Example:**
    ```bash
    PRINT DATE_GET_FORMAT;
    ```

<br>

<br>

## HTTP Requests

**HTTP_SET_PARAMS**

- **Syntax:** `HTTP_SET_PARAMS {MAP}`
- **Description:** Sets the parameters for the HTTP request.
- **Example:**
    ```bash
    HTTP_SET_PARAMS {"key": "value"}
    ```

<br>

**HTTP_SET_HEADER**

- **Syntax:** `HTTP_SET_HEADER {MAP}`
- **Description:** Sets the headers for the HTTP request.
- **Example:**
    ```bash
    HTTP_SET_HEADER {"Content-Type": "application/json"}
    ```

<br>

**HTTP_SET_METHOD**

- **Syntax:** `HTTP_SET_METHOD {STRING}`
- **Description:** Defines the HTTP method to be used (e.g., `POST`, `GET`, etc.).
- **Example:**
    ```bash
    HTTP_SET_METHOD "POST"
    ```

<br>

**HTTP_SET_DATA**

- **Syntax:** `HTTP_SET_DATA {MAP}`
- **Description:** Sets the data to be sent in the HTTP request.
- **Example:**
    ```bash
    HTTP_SET_DATA {"name": "John", "age": 30}
    ```

<br>

**HTTP_SET_TIMEOUT**

- **Syntax:** `HTTP_SET_TIMEOUT {TIME: NUMBER}`
- **Description:** Sets the timeout (in seconds) for the HTTP request. Default is 30 seconds.
- **Example:**
    ```bash
    HTTP_SET_TIMEOUT 10
    ```

<br>

**HTTP_RESET**

- **Syntax:** `HTTP_RESET`
- **Description:** Resets all configured parameters for the HTTP request.
- **Example:**
    ```bash
    HTTP_RESET
    ```

<br>

**HTTP_REQUEST**

- **Syntax:** `HTTP_REQUEST {URL: STRING} [AS {VAR}]`
- **Description:** Makes an HTTP request to the specified URL and stores the response in a variable.
- **Example:**
    ```bash
    HTTP_REQUEST "https://example.com/api" AS $response
    ```

<br>

**HTTP_REQUEST_EXT**

- **Syntax:** `HTTP_REQUEST_EXT {URL: STRING} [AS {VAR}]`
- **Description:** Makes an HTTP request to the specified URL and returns extended response information (e.g., redirects, encoding, history, text, etc.).
- **Example:**
    ```bash
    HTTP_REQUEST_EXT "https://example.com/api" AS $extended_response
    ```

<br>

**HTTP_GET**

- **Syntax:** `HTTP_GET {URL: STRING} [AS {VAR}]`
- **Description:** Makes a GET request to the specified URL and returns the response.
- **Example:**
    ```bash
    HTTP_GET "https://example.com/api" AS $get_response
    ```

<br>

**HTTP_POST**

- **Syntax:** `HTTP_POST {URL: STRING} [AS {VAR}]`
- **Description:** Makes a POST request to the specified URL and returns the response.
- **Example:**
    ```bash
    HTTP_POST "https://example.com/api" AS $post_response
    ```

<br>

**HTTP_DOWNLOAD**

- **Syntax:** `HTTP_DOWNLOAD {URL: STRING} TO {FILE: STRING}`
- **Description:** Downloads a file from the specified URL and saves it to the specified location.
- **Example:**
    ```bash
    HTTP_DOWNLOAD "https://example.com/file.zip" TO "downloads/file.zip"
    ```

<br>

**HTTP_UPLOAD**

- **Syntax:** `HTTP_UPLOAD {FILE: STRING} TO {URL: STRING} [AS {VAR}]`
- **Description:** Uploads a file to the specified URL and stores the response in a variable.
- **Example:**
    ```bash
    HTTP_UPLOAD "path/to/file.zip" TO "https://example.com/upload" AS $upload_response
    ```

<br>

**HTTP_GET_SETTINGS**

- **Syntax:** `HTTP_GET_SETTINGS [AS {VAR}]`
- **Description:** Returns the current parameters, headers, and other settings of the HTTP request.
- **Example:**
    ```bash
    HTTP_GET_SETTINGS AS $settings
    ```

<br>

**Ejemplo Completo**

```bash
HTTP_SET_METHOD "GET"
HTTP_REQUEST "https://jsonplaceholder.typicode.com/users" AS $response
PRINTF $response.content    # Acceder al contenido de la respuesta
```



<br>

## Translation

Scriptal provides built-in functions to translate text between various languages using the Google Translator API. However, it is important to note that the availability and accuracy of the translation service are subject to Google's conditions and potential interruptions. This could result in failures, usage limitations, or occasional blocks.

You can use both the language code (`en`) and its name (`english`). You can obtain all available languages for translation using the `TRANSLATE_SUPPORTED_CODES` command.

<br>

**TRANSLATE FROM**

- **Syntax:** `TRANSLATE FROM {SOURCE_LANG: STRING} TO {TARGET_LANG: STRING}`
- **Description:** Configures the source and target languages globally.
- **Example:**
    ```bash
    TRANSLATE FROM "en" TO "it"
    ```

<br>

**TRANSLATE**

- **Syntax:** `TRANSLATE {STRING} [AS {VAR}]`
- **Description:** Translates the specified text to the default target language. If the translation fails, it will throw an error.
- **Example:**
    ```bash
    TRANSLATE "Hello, world!" AS $translated_text
    ```

<br>

**TRANSLATES**

- **Syntax:** `TRANSLATES {STRING} [AS {VAR}]`
- **Description:** Translates the specified text to the default target language. If the translation fails, the result will be the same as the input text.
- **Example:**
    ```bash
    TRANSLATES "How are you?" AS $translated_text   # Si falla la traducción, el resultado será "How are you?"
    ```

<br>

**TRANSLATE_ATTEMPTS**

- **Syntax:** `TRANSLATE_ATTEMPTS {NUMBER}`
- **Description:** Sets the number of retry attempts for translation in case of failures.
- **Example:**
    ```bash
    TRANSLATE_ATTEMPTS 3
    TRANSLATE ""    # Un string vacío normalmente produce errores.
    ```

<br>

**TRANSLATE_FROM**

- **Syntax:** `TRANSLATE_FROM {SOURCE_LANG: STRING}`
- **Description:** Sets the source language for future translations.
- **Example:**
    ```bash
    TRANSLATE_FROM "en"
    ```

<br>

**TRANSLATE_TO**

- **Syntax:** `TRANSLATE_TO {TARGET_LANG: STRING}`
- **Description:** Sets the target language for future translations.
- **Example:**
    ```bash
    TRANSLATE_TO "es"
    ```

<br>

**TRANSLATE_SUPPORTED_CODES**

- **Syntax:** `TRANSLATE_SUPPORTED_CODES [AS {VAR}]`
- **Description:** Returns the supported language codes for translation.
- **Example:**
    ```bash
    TRANSLATE_SUPPORTED_CODES AS $supported_codes
    ```

<br>

**TRANSLATE_SUPPORTED_LANGS**

- **Syntax:** `TRANSLATE_SUPPORTED_LANGS [AS {VAR}]`
- **Description:** Returns the supported languages for translation.
- **Example:**
    ```bash
    TRANSLATE_SUPPORTED_LANGS AS $supported_langs
    ```
