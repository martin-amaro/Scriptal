# Lista de Comandos

## Introducción a la Sintaxis de Comandos

En esta guía, se utilizan corchetes `{}` para especificar que se espera un tipo de dato, como STRING, NUMBER, MAP, ARRAY, BOOL, entre otros. Cuando ves el símbolo `|` (pipe), significa que el comando puede aceptar más de un tipo de dato. Por ejemplo, `{STRING|NUMBER}` indica que se puede usar tanto un string como un número.

Si encuentras un nombre seguido de dos puntos `:`, como en `{FILE: STRING}`, esto indica que el parámetro requiere un STRING, pero se usa el descriptor "FILE" solo para facilitar la comprensión de lo que se espera.

El uso de `{VAR}` implica que el comando solicita una variable en su forma cruda, es decir, una que aún no ha sido evaluada o asignada, ideal para situaciones en las que se busca crear o modificar variables sin leer su valor previamente.

Finalmente, los corchetes `[]` se emplean para señalar que un parámetro es opcional. Por ejemplo, en `UPPERCASE {STRING} [AS {VAR}]`, el `AS {VAR}` es opcional y no es obligatorio proporcionarlo al usar el comando.

<br>

## Lista Completa de Comandos en Scriptal

1. [Variables y Valores. ](#variables-y-valores)
   1. [ Manipulación de Variables y Valores. ](#manipulación-de-variables-y-valores)
   2. [ Comparación de Variables y Valores. ](#comparación-de-variables-y-valores)

2. [ Strings. ](#strings)
   1. [ Manipulación de Strings. ](#manipulación-de-strings)
   2. [ Casing. ](#casing)
   3. [ Comparación de Strings. ](#comparación-de-strings)

3. [ Arrays. ](#arrays)

4. [ Mapas. ](#mapas)

5. [ Conversión. ](#conversión)

   1. [ Conversión de Tipos. ](#conversión-de-tipos)
   2. [ Conversión de Caracteres y Números. ](#conversión-de-caracteres-y-números)

6. [ Operaciones Matemáticas. ](#operaciones-matemáticas)

7. [ Patrones y Expresiones Regulares. ](#patrones-y-expresiones-regulares)

8. [ Archivos y Directorios. ](#archivos-y-directorios)
   1. [ Manipulación de Archivos. ](#manipulación-de-archivos)
   2. [ Manipulación de Directorios. ](#manipulación-de-directorios)
   3. [ Manipulación de Rutas de Archivos. ](#manipulación-de-rutas-de-archivos)
   4. [ Manipulación de JSON. ](#manipulación-de-json)

9. [ Valores Aleatorios. ](#valores-aleatorios)

10. [ Control de Flujo. ](#control-de-flujo)
    1. [ Condicionales. ](#condicionales)
    2. [ Bucles. ](#bucles)
    3. [ Definiciones. ](#definiciones)
    4. [ Módulos. ](#módulos)
    5. [ Manejo de Errores. ](#manejo-de-errores)

11. [ Entrada/Salida.](#entradasalida)
    1. [ Contenido. ](#contenido)
    2. [ Misc. ](#misc)

12. [ Fechas y Tiempos. ](#fechas-y-tiempos)

13. [ Gestión de sistemas y procesos. ](#gestión-de-sistemas-y-procesos)

14. [ Solicitudes HTTP. ](#solicitudes-http)

15. [ Traducción. ](#traducción)


<br>

## Variables y Valores

### Manipulación de Variables y Valores

**SET**

- **Sintaxis:** `SET {VAR|ARRAY_VAR} [AS {VALUE}]`
- **Descripción:** Asigna un valor a una variable o a un array de variables.
- **Ejemplo:**
  
    ```bash
    SET $x AS 10
    SET [$x, $y, $z] AS [10, 20, 30]
    ```

<br>

**UNSET**

- **Sintaxis:** `UNSET {VAR|ARRAY_VAR}`
- **Descripción:** Elimina una variable o array de variables.
- **Ejemplo:**
    ```bash
    UNSET $x
    UNSET [$x, $y, $z]
    ```

<br>

**VAR_SET**

- **Sintaxis:** `VAR_SET {STRING} AS {VALUE}`
- **Descripción:** Asigna un valor a una variable con nombre específico.
- **Ejemplo:**
    ```bash
    VAR_SET "my_var" AS 10
    ```

<br>

**VAR_GET**

- **Sintaxis:** `VAR_GET {STRING} [AS {VAR}]`
- **Descripción:** Obtiene el valor de una variable con nombre específico.
- **Ejemplo:**
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

- **Sintaxis:** `VAR_UNSET {STRING}`
- **Descripción:** Elimina una variable con nombre específico.
- **Ejemplo:**
    ```bash
    VAR_UNSET "my_var"
    ```

<br>

**VAR_EXISTS**

- **Sintaxis:** `VAR_EXISTS {STRING} [AS {VAR}]`
- **Descripción:** Verifica si una variable con nombre específico existe.
- **Ejemplo:**
    ```bash
    VAR_EXISTS "my_var" AS $exists    # TRUE / FALSE
    ```

<br>

**GLOBAL_SET**

- **Sintaxis:** `GLOBAL_SET {VAR|ARRAY_VAR} [AS {VALUE}]`
- **Descripción:** Asigna un valor a una variable o a un array de variables de forma global.
- **Ejemplo:**
  
    ```bash
    DEFINE EXAMPLE
        GLOBAL_SET $var AS "Hello World"
    
    EXAMPLE
    PRINT $var  # Hello World
    ```

<br>

**RASSIGN**

- **Sintaxis:** `RASSIGN {ANY} TO {DEST: STRING|NULL}`
- **Descripción:** Su nombre proviene de "_Return or Assign_". Permite exponer variables a contextos superiores, facilitando la comunicación entre diferentes scopes. Si `DEST` es `NULL`, el comando simplemente retorna el valor sin asignarlo a ninguna variable. Se utiliza para imitar la funcionalidad de `[AS {VAR}]` en comandos personalizados.


- **Ejemplo:**
    ```bash
    DEFINE STRETCH_WORD
        PARAMS [STRING]
        PARAMS [STRING, AS, VAR]

        GET 2 IN @PARAMS AS $VAR

        SPLIT @PARAMS.1 BY "" AS $word
        JOIN $word WITH "-" AS $result  # Example: h-e-l-l-o

        # Retorna el resultado si $VAR no se define
        # De lo contrario, asigna el resultado a la variable sin retornar nada.

        RETURN RASSIGN $result TO $VAR;


    PRINT STRETCH_WORD "hello";         # En este caso, retornará un valor
    STRETCH_WORD "hello" AS $stretched  # Acá no retorna nada, pero el valor es asignado a la variable
    PRINT $stretched
    ```
    <br>

<br>

**CLEAR**

- **Sintaxis:** `CLEAR {VAR}`
- **Descripción:** Limpia el contenido de una variable.
- **Ejemplo:**
    ```bash
    CLEAR $x    # Limpia el contenido de la variable $x
    ```

<br>

**COPY**

- **Sintaxis:** `COPY {ANY} [AS {VAR}] `
- **Descripción:** Copia el tipo de dato o valor de una variable.
- **Ejemplo:**
    ```bash
    COPY $x AS $y
    ```

<br>

**CLONE**

- **Sintaxis:** `CLONE {ANY} AS {VAR}`
- **Descripción:** Crea una copia exacta de un tipo de dato o variable.
- **Ejemplo:**
    ```bash
    CLONE $x AS $clone
    ```

<br>

**PICK**

- **Sintaxis:** 
    1. `PICK {ANY} OR {ANY} [AS {VAR}]`
    2. `PICK {ARRAY} [AS {VAR}]`

- **Descripción:** Selecciona el primer valor verdadero entre dos opciones o de un array.
- **Ejemplo**
    ```bash
    PICK "" OR 32    # Retorna 32
    PICK [FALSE, 0, "", TRUE]    # Retorna TRUE
    ```

<br>

**LENGTH**

- **Sintaxis:** `LENGTH {ANY} [AS {VAR}]`
- **Descripción:** Retorna la longitud de un array, string, o número de claves de un mapa.
- **Ejemplo:**
    ```bash
    LENGTH "Hello" AS $len    # Retorna 5
    ```

<br>

**INCREMENT**

- **Sintaxis:** `INCREMENT {VAR} [WITH {NUMBER}]`
- **Descripción:** Incrementa el valor de una variable numérica en un valor específico (por defecto, incrementa en 1).
- **Ejemplo:**
    ```bash
    INCREMENT $x WITH 2    # Incrementa $x en 2
    INCREMENT $x           # Incrementa $x en 1
    ```

<br>

**DECREMENT**

- **Sintaxis:** `DECREMENT {VAR} [WITH {NUMBER}]`
- **Descripción:** Decrementa el valor de una variable numérica en un valor específico (por defecto, decrementa en 1).
- **Ejemplo:**
    ```bash
    DECREMENT $x WITH 2    # Decrementa $x en 2
    DECREMENT $x           # Decrementa $x en 1
    ```

<br>

### Comparación de Variables y Valores



**IS_STRING**

- **Sintaxis:** `IS_STRING {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un string.

- **Ejemplo:**

  ```bash
  IS_STRING "Hello, world!" AS $result
  ```

<br>

**IS_NUMBER**

- **Sintaxis:** `IS_NUMBER {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un número (entero o flotante).

- **Ejemplo:**

  ```bash
  IS_NUMBER 42 AS $result
  ```

<br>

**IS_INTEGER**

- **Sintaxis:** `IS_INTEGER {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un número entero.

- **Ejemplo:**

  ```bash
  IS_INTEGER 123 AS $result
  ```

<br>

**IS_FLOAT**

- **Sintaxis:** `IS_FLOAT {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un número flotante.

- **Ejemplo:**

  ```bash
  IS_FLOAT 12.34 AS $result
  ```

<br>

**IS_ARRAY**

- **Sintaxis:** `IS_ARRAY {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un array.

- **Ejemplo:**

  ```bash
  IS_ARRAY [1, 2, 3] AS $result
  ```

<br>

**IS_MAP**

- **Sintaxis:** `IS_MAP {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un mapa (diccionario).

- **Ejemplo:**

  ```bash
  IS_MAP {"key": "value"} AS $result
  ```

<br>

**IS_BOOL**

- **Sintaxis:** `IS_BOOL {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un booleano.

- **Ejemplo:**

  ```bash
  IS_BOOL TRUE AS $result
  ```

<br>

**IS_PATTERN**

- **Sintaxis:** `IS_PATTERN {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un patrón (regular expression).

- **Ejemplo:**

  ```bash
  IS_PATTERN $pattern AS $result
  ```

<br>

**IS_EMPTY**

- **Sintaxis:** `IS_EMPTY {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado está vacío.

- **Ejemplo:**

  ```bash
  IS_EMPTY "" AS $result
  ```

<br>

**IS_EVEN**

- **Sintaxis:** `IS_EVEN {NUMBER} [AS {VAR}]`

- **Descripción:** Verifica si el número especificado es par.

- **Ejemplo:**

  ```bash
  PRINT IS_EVEN 2 $num; # TRUE
  ```

<br>

**IS_ODD**

- **Sintaxis:** `IS_ODD {NUMBER} [AS {VAR}]`

- **Descripción:** Verifica si el número especificado es impar.

- **Ejemplo:**

  ```bash
  PRINT IS_ODD 3 $num; # TRUE
  ```

<br>

**IS_NAN**

- **Sintaxis:** `IS_NAN {ANY} [AS {VAR}]`

- **Descripción:** Verifica si el valor especificado es [NAN](https://en.wikipedia.org/wiki/NaN) (Not a number).

- **Ejemplo:**

  ```bash
  SET $num AS NAN
  IS_NAN $num AS $result
  ```

<br>

**IS_INFINITY**

- **Sintaxis:** `IS_INFINITY {ANY} [AS {VAR}]`

- **Descripción:** Verifica si el valor especificado es infinito.

- **Ejemplo:**

  ```bash
  SET $num AS 1e308 * 10
  IS_INFINITY $num AS $result
  ```

<br>

**IS_DATE**

- **Sintaxis:** `IS_DATE {ANY} [AS {VAR}]`

- **Descripción:** Valida si el valor especificado es un mapa de fecha.

- **Ejemplo:**

  ```bash
  IS_DATE $datemap AS $result
  ```

<br>



<br>

## Strings


### Manipulación de Strings

**TRIM**

- **Sintaxis:** `TRIM {STRING} [AS {VAR}]`
- **Descripción:** Elimina los espacios en blanco al inicio y al final del texto.
- **Ejemplo:**
    ```bash
    TRIM "   Hello World!   " AS $trimmed   # "Hello World!"
    ```

<br>

**TRIM_START**

- **Sintaxis:** `TRIM_START {STRING} [AS {VAR}]`
- **Descripción:** Elimina los espacios en blanco al inicio del texto.
- **Ejemplo:**
    ```bash
    TRIM_START "   Hello World!   " AS $trimmed_start   # "Hello World!   "
    ```

<br>

**TRIM_END**

- **Sintaxis:** `TRIM_END {STRING} [AS {VAR}]`
- **Descripción:** Elimina los espacios en blanco al final del texto.
- **Ejemplo:**
    ```bash
    TRIM_END "   Hello World!   " AS $trimmed_end   # "   Hello World!"
    ```

<br>

**CLEAN**

- **Sintaxis:** `CLEAN {STRING} [AS {VAR}]`
- **Descripción:** Elimina los espacios en blaco adicionales en un string. A diferencia de `TRIM`, `CLEAN` elimina los espacios en blanco en cualquier posición.
- **Ejemplo:**
    ```bash
    PRINT CLEAN "Hello      World       Lorem       Ipsum";  # Hello Wolrd Lorem Ipsum
    ```

<br>

**PAD**

- **Sintaxis:** `PAD {STRING} TO {NUMBER} WITH {FILL: STRING} [AS {VAR}]`
- **Descripción:** Rellena el texto `STRING` hasta alcanzar una longitud de `NUMBER` usando el carácter de relleno `FILL`. 
- **Ejemplo:**
    ```bash
    PAD "bird" TO 10 WITH "-"   # ---bird--- 
    ```

<br>

**PAD_START**

- **Sintaxis:** `PAD_START {STRING} TO {NUMBER} WITH {FILL: STRING} [AS {VAR}]`
- **Descripción:** Rellena el texto `STRING` con el carácter de relleno `FILL` al inicio hasta alcanzar una longitud de `NUMBER`.
- **Ejemplo:**
    ```bash
    PRINT PAD_START "2" TO 2 WITH "0";  # 02
    PRINT PAD_START "10" TO 2 WITH "0"; # 10
    ```

<br>

**PAD_END**

- **Sintaxis:** `PAD_END {STRING} TO {NUMBER} WITH {FILL: STRING} [AS {VAR}]`
- **Descripción:** Rellena el texto `STRING` con el carácter de relleno `FILL` al final hasta alcanzar una longitud de `NUMBER`.
- **Ejemplo:**
    ```bash
    PAD_END "Hello" TO 10 WITH "-" AS $padded_end
    ```
    <br>

**REPLACE**

- **Sintaxis:** `REPLACE {X: NUMBER} WITH {Y: NUMBER} IN {Z: NUMBER} [AS {VAR}]`
- **Descripción:** Reemplaza la primera ocurrencia de `X` con `Y` en el texto `Z`.
- **Ejemplo:**
    ```bash
    REPLACE "world" WITH "there" IN "Hello world!" AS $replaced # Hello there!
    ```

<br>

**REPLACE_ALL**

- **Sintaxis:** `REPLACE_ALL {X: NUMBER} WITH {Y: NUMBER} IN {Z: NUMBER} [AS {VAR}]`
- **Descripción:** Reemplaza todas las ocurrencias de `X` con `Y` en el texto `Z`.
- **Ejemplo:**
    ```bash
    REPLACE_ALL "my" WITH "your" IN "Hi, my name is John. This is my house." AS $replaced   # Hi, your name is John. This is your house.
    ```

<br>

**INSERT**

- **Sintaxis:** `INSERT {SUBSTR} AT {INDEX: NUMBER} IN {STRING} [AS {VAR}]`

- **Descripción:** Inserta el texto `SUBSTR` en la posición `INDEX` del texto original `STRING`.
- **Ejemplo:**
    ```bash
    INSERT "Wonderful " AT 7 IN "Hello World!" AS $inserted   # Hello Wonderful World!
    ```

<br>

**REMOVE**

- **Sintaxis:**
  
    1. `REMOVE {SUBSTR} IN {STRING} [AS {VAR}]`
    2. `REMOVE FROM {X: NUMBER} TO {Y: NUMBER} IN {STRING} [AS {VAR}]`
    
- **Descripción:** 
    1. Elimina todas las ocurrencias de `SUBSTR` en el texto `STRING`.
    2. Elimina desde `X` hasta `Y` en el texto `STRING`.
- **Ejemplo:**
    ```bash
    REMOVE "world" IN "Hello world!" AS $removed        # Hello !
    REMOVE FROM 1 TO 6 IN "Hello world!" AS $removed    # world!
    ```

<br>

**SPLIT**

- **Sintaxis:** `SPLIT {STRING} BY {SEP: STRING} [AS {VAR}]`
- **Descripción:** Divide el texto `STRING` en un array utilizando `SEP` como delimitador.
- **Ejemplo:**
    ```bash
    SPLIT "a,b,c" BY "," AS $split  # ["a", "b", "c"]
    ```

<br>

**REVERSE**

- **Sintaxis:** `REVERSE {STRING} [AS {VAR}]`
- **Descripción:** Invierte el texto `STRING`.
- **Ejemplo:**
    ```bash
    REVERSE "Hello" AS $reversed    # olleH
    ```

<br>

**COUNT**

- **Sintaxis:** `COUNT {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Cuenta las ocurrencias de `SUBSTR` en el texto `STRING`.
- **Ejemplo:**
    ```bash
    COUNT "l" IN "Hello" AS $count  # 2
    ```

<br>

**JOIN**

- **Sintaxis:** `JOIN {ARRAY} WITH {TOKEN: STRING} [AS {VAR}]`
- **Descripción:** Une los elementos del array `ARRAY` utilizando `TOKEN` como separador.
- **Ejemplo:**
    ```bash
    JOIN ["a", "b", "c"] WITH "-" AS $joined    # "a-b-c"
    ```

<br>

**FIND**

- **Sintaxis:** `FIND {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Busca la primera ocurrencia de `SUBSTR` en el texto `STRING`.
- **Ejemplo:**
    ```bash
    FIND "lo" IN "Hello" AS $found  # 4
    ```

<br>

**STARTS**

- **Sintaxis:** `STARTS {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Verifica si el texto `STRING` comienza con `SUBSTR`.
- **Ejemplo:**
    ```bash
    STARTS "Hel" IN "Hello" # TRUE
    ```

<br>

**ENDS**

- **Sintaxis:** `ENDS {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Verifica si el texto `STRING` termina con `SUBSTR`.
- **Ejemplo:**
    ```bash
    ENDS "llo" IN "Hello"   # TRUE
    ```

<br>

**CONTAINS**

- **Sintaxis:** `CONTAINS {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Verifica si el texto `STRING` contiene `SUBSTR`.
- **Ejemplo:**
    ```bash
    CONTAINS "beautiful" IN "You are so beautiful" AS $exists
    ```

<br>


**TRUNCATE**

- **Sintaxis:** 
    1. `TRUNCATE {STRING} TO {INDEX: NUMBER} [AS {VAR}]`
    1. `TRUNCATE {STRING} TO {INDEX: NUMBER} WITH {TOKEN} [AS {VAR}]`
- **Descripción:** Trunca el texto `STRING` hasta el índice `INDEX` y recorta usando `...` por defecto o usando `TOKEN`.
- **Ejemplo:**
    ```bash
    PRINT TRUNCATE "Hello World" TO 5;          # Hello...
    PRINT TRUNCATE "Hello World" TO 5 WITH "***"; # Hello***
    ```

<br>

**ESCAPE**

- **Sintaxis:** `ESCAPE {STRING} [AS {VAR}]`
- **Descripción:** Escapa caracteres especiales en el texto `STRING`.
- **Ejemplo:**
    ```bash
    ESCAPE "Hello\nworld!" AS $escaped  # Hello\nworld!
    ```

<br>

**UNESCAPE**

- **Sintaxis:** `UNESCAPE {STRING} [AS {VAR}]`
- **Descripción:** Desescapa caracteres especiales en el texto `STRING`.
- **Ejemplo:**
    ```bash
    UNESCAPE "Hello\\nWorld!" AS $unescaped # Hello
    # World!
    ```

<br>

**NORMALIZE**

- **Sintaxis:** `NORMALIZE {STRING} TO {FORM: STRING} [AS {VAR}]`
- **Descripción:** Normaliza el texto `STRING` a la forma especificada `FORM` de acuerdo con las normas Unicode. La normalización asegura que los caracteres se representen de manera consistente, lo que es útil para comparar textos, almacenar datos o procesar entradas de manera uniforme. 

    <br>

    `FORM`: La forma de normalización deseada, que puede ser `NFC`, `NFD`, `NFKC`, o `NFKD`.

    <br>

    Para más información: [Unicode equivalence - Wikipedia](https://en.wikipedia.org/wiki/Unicode_equivalence)

- **Ejemplo:**
    ```bash
    # Textos a comparar
    SET $a AS "é" # e + combining acute accent
    SET $b AS "é"


    # Antes de la normalización
    PRINT $a IS $b    # FALSE


    # Normalizamos ambos textos a NFC
    NORMALIZE $a TO "NFC" AS $normmalized_a
    NORMALIZE $b TO "NFC" AS $normmalized_b


    # Después de la normalización
    PRINT $normmalized_a IS $normmalized_b  # TRUE
    ```

<br>

**TRANSLIT**

- **Sintaxis:** `TRANSLIT {STRING} [AS {VAR}]`
- **Descripción:** Translitera el texto de cualquier alfabeto Unicode a una representación en ASCII. Esto permite que el texto, originalmente en alfabetos como el cirílico, griego o chino, se transforme en una forma más accesible para sistemas que solo manejan caracteres ASCII.
    
    <br>

    `TRANSLIT` ofrece una aproximación, y es posible que no siempre coincida con las transliteraciones más precisas o esperadas, especialmente para textos con caracteres especiales o en alfabetos complejos.

- **Ejemplo:**
    ```bash
    PRINT TRANSLIT "Καλημέρα κόσμεβ";  # Kalemera kosmeb
    PRINT TRANSLIT "Текст на кириллице";    # Tekst na kirillitse
    PRINT TRANSLIT "こんにちは";  # konnichiha
    ```

<br>

**REMOVE_ACCENTS**

- **Sintaxis:** `REMOVE_ACCENTS {STRING} [AS {VAR}]`
- **Descripción:** Limpia el texto `STRING` a una forma estándar, eliminando acentos y diacríticos. Esta operación convierte caracteres acentuados o especiales en sus equivalentes básicos sin acento.
- **Ejemplo:**
    ```bash
    PRINT REMOVE_ACCENTS "café";  # cafe
    PRINT REMOVE_ACCENTS "A criança está à espera do ônibus na praça";  # A crianca esta a espera do onibus na praca
    ```

<br>

**CHAR_AT**

- **Sintaxis:** `CHAR_AT {N: NUMBER} IN {STRING} [AS {VAR}]`
- **Descripción:** Obtiene el carácter en la posición `N` del texto `STRING`.
- **Ejemplo:**
    ```bash
    CHAR_AT 2 IN "Hello" AS $char   # e
    ```

<br>

**FORMAT**

- **Sintaxis:** `FORMAT {STRING} WITH {ARRAY} [AS {VAR}]`
- **Descripción:** Formatea el texto `STRING` usando los valores del array `ARRAY`.
- **Ejemplo:**
    ```bash
    FORMAT "Hello {1}" WITH ["world"] AS $formatted # Hello world
    ```

<br>

**EXTRACT**

- **Sintaxis:** 
    1. `EXTRACT FROM {X: NUMBER} TO {Y: NUMBER} IN {STRING} [AS {VAR}]`
    2. `EXTRACT BETWEEN {SUBSTR1} TO {SUBSTR2} IN {STRING} [AS {VAR}]`

- **Descripción:**
    1. Extrae una subcadena desde la posición `X` hasta la posición `Y` en el texto `STRING`.
    2. Extrae el texto entre dos subcadenas `SUBSTR1` y `SUBSTR2` en el texto `STRING`.

- **Ejemplo:**
    ```bash
    EXTRACT FROM 1 TO 5 IN "Hello World" AS $extracted  # Hello
    EXTRACT BETWEEN "(" AND ")" IN "I am free. (I am not) Lorem Ipsum" AS $extracted2   # I am not
    ```

<br>

**EXTRACT_FIRST**

- **Sintaxis:** `EXTRACT_FIRST TO {N: NUMBER} IN {STRING} [AS {VAR}]`
- **Descripción:** Extrae los primeros `N` caracteres del texto `STRING`.
- **Ejemplo:**
    ```bash
    EXTRACT_FIRST TO 5 IN "Hello World" AS $start_extracted   # Hello
    ```

<br>

**EXTRACT_LAST**

- **Sintaxis:** `EXTRACT_LAST TO {N: NUMBER} IN {STRING} [AS {VAR}]`
- **Descripción:** Extrae los últimos `N` caracteres del texto `STRING`.
- **Ejemplo:**
    ```bash
     EXTRACT_LAST TO 5 IN "Hello World" AS $end_extracted   # World
    ```

<br>

### Casing

**UPPERCASE**

- **Sintaxis:** `UPPERCASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto especificado a mayúsculas.

- **Ejemplo:**

  ```bash
  UPPERCASE "Hello world!" AS $upper  # HELLO WORLD!
  ```

<br>

**LOWERCASE**

- **Sintaxis:** `LOWERCASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto especificado a minúsculas.

- **Ejemplo:**

  ```bash
  LOWERCASE "Hello World!" AS $lower  # hello world!
  ```

<br>

**TITLECASE**

- **Sintaxis:** `TITLECASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto especificado a título (cada palabra inicia con mayúscula).

- **Ejemplo:**

  ```bash
  TITLECASE "hello world" AS $title   # Hello World
  ```

<br>

**SWAPCASE**

- **Sintaxis:** `SWAPCASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte las letras mayúsculas en minúsculas y viceversa en el texto.

- **Ejemplo:**

  ```bash
  SWAPCASE "Hello World!" AS $swapped # hELLO wORLD!
  ```

  <br>

**CAMELCASE**

- **Sintaxis:** `CAMELCASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto a camelCase, donde la primera palabra comienza en minúscula y las palabras subsiguientes comienzan con mayúscula.

- **Ejemplo:**

  ```bash
  CAMELCASE "hello world example" AS $camelcase   # helloWorldExample
  ```

<br>

**KEBABCASE**

- **Sintaxis:** `KEBABCASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto a kebab-case, donde las palabras están separadas por guiones y todas las letras están en minúscula.

- **Ejemplo:**

  ```bash
  KEBABCASE "hello world example" AS $kebabcase   # hello-world-example
  ```

  <br>

**SLUG**

- **Sintaxis:** `SLUG {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto a un formato de slug, utilizando solo caracteres en minúscula y separando las palabras con guiones. El comando también elimina caracteres especiales y acentos para generar una URL amigable.

- **Ejemplo:**

  ```bash
  SLUG "Ich heiße" AS $slug            #ich-heisse
  KEBABCASE "Ich heiße" AS $kebabcase  #ich-hei-e
  ```


<br>

**MACROCASE**

- **Sintaxis:** `MACROCASE {STRING} [AS {VAR}]`

- **Descripción:** Convierte el texto a MACROCASE, donde todas las palabras están en mayúsculas y separadas por guiones bajos.

- **Ejemplo:**

  ```bash
  MACROCASE "hello world example" AS $macrocase   # HELLO_WORLD_EXAMPLE
  ```

<br>

**CASEFOLD**

- **Sintaxis:** `CASEFOLD {STRING} [AS {VAR}]`

- **Descripción:** Normaliza el texto para comparación insensible a mayúsculas y minúsculas, convirtiéndolo a una forma estándar de comparación.

- **Ejemplo:**

  ```bash
  CASEFOLD "Hello World!" AS $casefolded  # hello world!
  ```

<br>

**CAPITALIZE**

- **Sintaxis:** `CAPITALIZE {STRING} [AS {VAR}]`

- **Descripción:** Capitaliza la primera letra del texto, convirtiendo la primera letra a mayúscula y el resto a minúsculas.

- **Ejemplo:**

  ```bash
  CAPITALIZE "hello world" AS $capitalized    # Hello world
  ```

<br>



### Comparación de Strings

**IS_EMAIL**

- **Sintaxis:** `IS_EMAIL {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado es una dirección de correo electrónico válida.

- **Ejemplo:**

  ```bash
  IS_EMAIL "example@example.com" AS $result
  ```

<br>

**IS_URL**

- **Sintaxis:** `IS_URL {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado es una URL válida.

- **Ejemplo:**

  ```bash
  IS_URL "https://www.example.com" AS $result
  ```

<br>

**IS_DIGIT**

- **Sintaxis:** `IS_DIGIT {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado contiene solo dígitos.

- **Ejemplo:**

  ```bash
  IS_DIGIT "123456" AS $result
  ```

<br>

**IS_NUMERIC**

- **Sintaxis:** `IS_NUMERIC {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado es numérico, permitiendo tanto enteros, flotantes y exponentes (ej. ², ¾).

- **Ejemplo:**

  ```bash
  IS_NUMERIC "123.45" AS $result    # TRUE
  IS_NUMERIC "¾" AS $result2        # TRUE
  IS_NUMERIC "-43.322" AS $result3  # TRUE
  ```

<br>

**IS_ALPHA**

- **Sintaxis:** `IS_ALPHA {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado contiene solo caracteres alfabéticos.

- **Ejemplo:**

  ```bash
  IS_ALPHA "Hello" AS $result
  ```

<br>

**IS_ALPHANUMERIC**

- **Sintaxis:** `IS_ALPHANUMERIC {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado contiene solo caracteres alfabéticos y números.

- **Ejemplo:**

  ```bash
  IS_ALPHANUMERIC "Hello123" AS $result
  ```

<br>

**IS_UPPERCASE**

- **Sintaxis:** `IS_UPPERCASE {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado está en mayúsculas.

- **Ejemplo:**

  ```bash
  IS_UPPERCASE "HELLO" AS $result
  ```

<br>

**IS_LOWERCASE**

- **Sintaxis:** `IS_LOWERCASE {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado está en minúsculas.

- **Ejemplo:**

  ```bash
  IS_LOWERCASE "hello" AS $result
  ```

<br>

**IS_TITLECASE**

- **Sintaxis:** `IS_TITLECASE {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado está en formato de título (capitalización de la primera letra de cada palabra).

- **Ejemplo:**

  ```bash
  IS_TITLECASE "Hello World" AS $result
  ```

<br>

**IS_DEC**

- **Sintaxis:** `IS_DEC {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado representa un número en el sistema decimal.

- **Ejemplo:**

  ```bash
  IS_DEC "255" AS $result
  ```

<br>

**IS_HEX**

- **Sintaxis:** `IS_HEX {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado representa un número hexadecimal.

- **Ejemplo:**

  ```bash
  IS_HEX "1A3F" AS $result
  ```

<br>

**IS_ASCII**

- **Sintaxis:** `IS_ASCII {STRING} [AS {VAR}]`

- **Descripción:** Valida si el string especificado contiene solo caracteres ASCII (caracteres básicos del alfabeto inglés y símbolos estándar).

- **Ejemplo:**

  ```bash
  IS_ASCII "Hello!" AS $result
  ```

  <br>


##  Arrays

**GET**

- **Sintaxis:** `GET {INDEX: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Obtiene el elemento en la posición especificada dentro del array. Si el índice no existe, retorna `NULL`.
- **Ejemplo:**
    ```bash
    SET $array AS ["Paris", "London", "Tokyo"]
    GET 2 IN $array AS $element    # London
    ```

<br>

**APPEND**

- **Sintaxis:** `APPEND {ANY} TO {ARRAY}`
- **Descripción:** Añade un elemento al final de un array.
- **Ejemplo:**
    ```bash
    APPEND "nuevo_elemento" TO $array
    ```

<br>

**PUT**

- **Sintaxis:** 
    1. `PUT {ANY} IN {ARRAY}`
    2. `PUT {ANY} AT {INDEX: NUMBER} IN {ARRAY}`

- **Descripción:**
    1. Añade un elemento al final del array
    2. Modifica o reemplaza el valor en el índice especificado.

- **Ejemplos:**
    ```bash
    SET $array AS [1, 2, 3, 4 ,5]
    PUT "nuevo" IN $array               # [1, 2, 3, 4, 5, "nuevo"]
    PUT "modificado" AT 3 IN $array     # [1, 2, "modificado", 4, 5, "nuevo"]
    ```

<br>

**INSERT**

- **Sintaxis:** `INSERT {ANY} AT {INDEX: NUMBER} IN {ARRAY}`
- **Descripción:** Inserta un nuevo elemento en la posición especificada dentro de un array.
- **Ejemplo:**
    ```bash
    SET $array AS [1, 2, 3]
    INSERT "nuevo_elemento" AT 1 IN $array    # [1, "nuevo_elemento", 2, 3]
    ```

<br>

**REMOVE**

- **Sintaxis:** 
    1. `REMOVE {INDEX: NUMBER} IN {ARRAY}`
    2. `REMOVE FROM {X: NUMBER} TO {Y: NUMBER} IN {ARRAY}`
- **Descripción:** Elimina un elemento en un índice específico o un rango de índices en un array.
- **Ejemplo:**
    ```bash
    SET $array AS [1, 2, 3, 4, 5, 6, 7]
    REMOVE 2 IN $array              # [1, 3, 4, 5, 6, 7]
    REMOVE FROM 1 TO 3 IN $array    # [5, 6, 7]
    ```

<br>

**REMOVE_FIRST**

- **Sintaxis:** `REMOVE_FIRST {ARRAY}`
- **Descripción:** Elimina el primer elemento de un array.
- **Ejemplo:**
    ```bash
    SET $array AS [1, 2, 3]
    REMOVE_FIRST $array    # [2, 3]
    ```

<br>

**REMOVE_LAST**

- **Sintaxis:** `REMOVE_LAST {ARRAY}`
- **Descripción:** Elimina el último elemento de un array.
- **Ejemplo:**
    ```bash
    SET $array AS [1, 2, 3]
    REMOVE_LAST $array  # [1, 2]
    ```

<br>

**REMOVE_DUPLICATES**

- **Sintaxis:** `REMOVE_DUPLICATES {ARRAY} [AS {VAR}]`
- **Descripción:** Devuelve un nuevo array sin elementos duplicados.
- **Ejemplo:**
    ```bash
    REMOVE_DUPLICATES ["hello", "world", "hello", "there", "hello", "sir"]    # ["hello", "world", "there", "sir"]
    ```
    <br>

**DELETE**

- **Sintaxis:** `DELETE {ELEM: ANY} IN {ARRAY}`
- **Descripción:** Elimina todas las ocurrencias de un valor específico en un array.
- **Ejemplo:**
    ```bash
    DELETE "hola" IN ["world", "hello", "hola"]    # Elimina "hola" del array
    ```


**SORT**

- **Sintaxis:** `SORT {ARRAY}`
- **Descripción:** Ordena un array en orden ascendente.
- **Ejemplo:**
    ```bash
    SET $array AS ["b", "c", "a"]
    SORT $array # ["a", "b", "c"]
    ```

<br>

**REVERSE**

- **Sintaxis:** `REVERSE {ARRAY}`
- **Descripción:** Invierte el orden de los elementos en un array.
- **Ejemplo:**
    ```bash
    SET $array AS [1, 2, 3]
    REVERSE $array  # [3, 2, 1]
    ```

<br>

**SHUFFLE**

- **Sintaxis:** `SHUFFLE {ARRAY}`
- **Descripción:** Mezcla aleatoriamente los elementos de un array.
- **Ejemplo:**
    ```bash
    SHUFFLE $array    # Mezcla los elementos de $array
    ```

<br>


**EXTEND**

- **Sintaxis:** `EXTEND {ARRAY} WITH {ARRAY}`
- **Descripción:** Añade los elementos de un array al final de otro array, modificando el array original.
- **Ejemplo:**
    ```bash
    EXTEND $array1 WITH $array2    # Añade los elementos de $array2 al final de $array1
    ```

<br>

**FLATTEN**

- **Sintaxis:** `FLATTEN {ARRAY} [AS {VAR}]`
- **Descripción:** Aplana un array que contiene sub-arrays en un solo nivel.
- **Ejemplo:**
    ```bash
    FLATTEN ["a", "b", ["c", "d", ["e"]]] AS $flat_array    # Retorna ["a", "b", "c", "d", "e"]
    ```

<br>

**CLEAN**

- **Sintaxis:** `CLEAN {ARRAY} [AS {VAR}]`
- **Descripción:** Elimina los valores falsy (`0`, `""`, `NULL`, `FALSE`) de un array.
- **Ejemplo:**
    ```bash
    CLEAN [1, 2, "", 0, NULL, TRUE] AS $clean_array    # Retorna [1, 2, TRUE]
    ```

<br>

**DUPLICATES**

- **Sintaxis:** `DUPLICATES IN {ARRAY} [AS {VAR}]`
- **Descripción:** Devuelve un array con los elementos duplicados en el array original.
- **Ejemplo:**
    ```bash
    DUPLICATES IN [1, 2, 2, 3, 4, 4, 5] AS $dups    # Retorna [2, 4]
    ```

<br>

**INTERSECT**

- **Sintaxis:** `INTERSECT {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Descripción:** Devuelve un array con los elementos comunes en ambos arrays.
- **Ejemplo:**
    ```bash
    INTERSECT [1, 2, 3] WITH [2, 3, 4] AS $common_elements    # Retorna [2, 3]
    ```

<br>

**DIFFERENCE**

- **Sintaxis:** `DIFFERENCE {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Descripción:** Devuelve un array con los elementos que están en el primer array pero no en el segundo.
- **Ejemplo:**
    ```bash
    DIFFERENCE [1, 2, 3] WITH [2, 4] AS $difference    # Retorna [1, 3]
    ```

<br>

**MERGE**

- **Sintaxis:** `MERGE {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Descripción:** Devuelve la fusión entre dos arrays, eliminando elementos duplicados.
- **Ejemplo:**
    ```bash
    MERGE [1, 2, 3] WITH [2, 3, 4] AS $merged_array    # Retorna [1, 2, 3, 4]
    ```

<br>

**CONCAT**

- **Sintaxis:** `CONCAT {ARRAY} WITH {ARRAY} [AS {VAR}]`
- **Descripción:** Combina dos arrays y devuelve un nuevo array. Similar a `MERGE`, pero sin eliminar duplicados
- **Ejemplo:**
    ```bash
    CONCAT [1, 2, 3] WITH [2, 3, 4] AS $result_array    # [1, 2, 3, 2, 3, 4]
    ```

<br>


**CHUNK**

- **Sintaxis:** `CHUNK {ARRAY} BY {NUMBER} [AS {VAR}]`
- **Descripción:** Divide un array en fragmentos más pequeños de tamaño especificado.
- **Ejemplo:**
    ```bash
    CHUNK [1, 2, 3, 4, 5] BY 2 AS $chunks    # Retorna [[1, 2], [3, 4], [5]]
    ```

<br>

**TRANSFORM**

- **Sintaxis:** `TRANSFORM {ARRAY} WITH {COMMAND} [AS {VAR}]`
- **Descripción:** Aplica un comando a cada elemento de un array, devolviendo un nuevo array con los resultados.
- **Ejemplo:**
    ```bash
    TRANSFORM ["hello", "world", "my", "name", "is", "John"] WITH "UPPERCASE" AS $transformed_array
    ```

<br>

**EXTRACT**

- **Sintaxis:** `EXTRACT FROM {X: NUMBER} TO {Y: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Extrae una porción de un array desde el índice `X` hasta el índice `Y`.
- **Ejemplo:**
    ```bash
    SET $array AS [1, 2, 3, 4, 5, 6]
    EXTRACT FROM 1 TO 3 IN $array    # [1, 2, 3]
    ```

<br>

**EXTRACT_FIRST**

- **Sintaxis:** `EXTRACT_FIRST TO {N: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Extrae los `N` primeros elementos del array.
- **Ejemplo:**
    ```bash
    EXTRACT_FIRST TO 2 IN ["USA", "Spain", "UK", "Italy", "France"] AS $start_extracted   # ["USA", "Spain"]
    ```

<br>

**EXTRACT_LAST**

- **Sintaxis:** `EXTRACT_LAST TO {N: NUMBER} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Extrae los últimos `N` elementos del array.
- **Ejemplo:**
    ```bash
    EXTRACT_LAST TO 2 IN ["USA", "Spain", "UK", "Italy", "France"] AS $end_extracted   # ["Italy", "France"]
    ```

<br>

**COUNT**

- **Sintaxis:** `COUNT {ELEM: ANY} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Cuenta el número de veces que un elemento aparece en un array.
- **Ejemplo:**
    ```bash
    SET $array AS ["hello", "world", "hello", "there", "hello", "sir"] 
    COUNT "hello" IN $array AS $count    # 3
    ```
    <br>

**FIND**

- **Sintaxis:** `FIND {ANY} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Encuentra la posición de la primera coincidencia en el array.
- **Ejemplo:**
    ```bash
    FIND "red" IN ["blue", "pink", "yellow", "red", "white"] AS $position    # Retorna 4
    ```

<br>

**CONTAINS**

- **Sintaxis:** `CONTAINS {ELEM: ANY} IN {ARRAY} [AS {VAR}]`
- **Descripción:** Verifica si un array contiene el valor especificado.
- **Ejemplo:**
    ```bash
    CONTAINS "apple" IN ["pear", "watermelon", "apple", "lemon"] AS $exists
    ```

<br>


## Mapas

**MAP_SET**

- **Sintaxis:** `MAP_SET {VAR}`
- **Descripción:** Define o modifica una variable con un mapa. 
- **Ejemplo:**
    ```bash
    MAP $data   # Creamos un nuevo mapa
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

- **Sintaxis:** `KEY {NAME} AS {VALUE}`
- **Descripción:** Define o modifica una clave en la estructura del mapa actual. Debe estar dentro de un comando `MAP`.
- **Ejemplo:**
    ```bash
    MAP $dict
        KEY "hello" AS "Hola"
        KEY "world" : "Mundo" # Puedes usar "AS" o su forma abreviada ":"
    ```

<br>

**MAP_SET_KEY**

- **Sintaxis:** `MAP_SET_KEY {KEY: STRING} AS {VALUE} IN {MAP}`
- **Descripción:** Establece o actualiza un valor para una clave en un mapa.
- **Ejemplo:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    MAP_SET_KEY "name" AS "Doe" IN $map
    ```

<br>

**GET**

- **Sintaxis:** `GET {KEY: STRING} IN {MAP} [AS {VAR}]`
- **Descripción:** Obtiene el valor de la clave de un mapa. Si la clave no existe, retorna `NULL`.
- **Ejemplo:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    GET "name" IN $map AS $element    # John
    ```

<br>


**MAP_GET_KEY**

- **Sintaxis:** `MAP_GET_KEY {KEY: STRING} AS {VALUE} IN {MAP}`
- **Descripción:** Obtiene el valor de la clave de un mapa. A diferencia de `GET`, `MAP_GET_KEY` lanzará error si la clave no existe.
- **Ejemplo:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    MAP_SET_KEY "name" AS "Doe" IN $map
    ```

<br>

**MAP_KEYS**

- **Sintaxis:** `MAP_KEYS {MAP} [AS {VAR}]`
- **Descripción:** Obtiene todas las claves de un mapa.
- **Ejemplo:**
    ```bash
    MAP_KEYS {"name": "John", "age": 30} AS $keys   # ["name", "age"]
    ```

<br>

**MAP_VALUES**

- **Sintaxis:** `MAP_VALUES {MAP} [AS {VAR}]`
- **Descripción:** Obtiene todos los valores de un mapa.
- **Ejemplo:**
    ```bash
    MAP_VALUES {"name": "John", "age": 30} AS $values   # ["John", 30] 
    ```

<br>

**MAP_ITEMS**

- **Sintaxis:** `MAP_ITEMS {MAP} [AS {VAR}]`
- **Descripción:** Obtiene todos los pares clave-valor de un mapa como un array de arrays.
- **Ejemplo:**
    ```bash
    MAP_ITEMS {"name": "John", "age": 30} AS $items # [["name", "John"], ["age", 30]]
    ```
    <br>

**DELETE**

- **Sintaxis:** `DELETE {KEY: STRING} IN {MAP}`
- **Descripción:** Elimina una clave y su valor asociado de un mapa.
- **Ejemplo:**
    ```bash
    SET $map AS {"name": "John", "age": 30}
    DELETE "age" IN $map
    ```

<br>

**CONTAINS**

- **Sintaxis:** `CONTAINS {KEY: STRING} IN {MAP} [AS {VAR}]`
- **Descripción:** Verifica si un mapa contiene una clave específica.
- **Ejemplo:**
    ```bash
    CONTAINS "name" IN {"name": "John", "age": 30} AS $exists
    ```

<br>

**MERGE**

- **Sintaxis:** `MERGE {MAP} WITH {MAP} [AS {VAR}]`
- **Descripción:** Crea un nuevo mapa fusionando dos mapas. Funciona igual a la suma de mapas con el operador `+`.
- **Ejemplo:**
    ```bash
    MERGE {"name": "John"} WITH {"age": 30} AS $mergedMap
    ```

<br>

**UPDATE**

- **Sintaxis:** `UPDATE {MAP} WITH {MAP}`
- **Descripción:** Fusiona dos mapas, modificando el mapa original.
- **Ejemplo:**
    ```bash
    SET $map1 AS {"name": "John"}
    SET $map2 AS {"age": 30}
    
    UPDATE $map1 WITH $map2
    PRINT $map1 # {"name": "John", "age": 30}
    ```

<br>

## Conversión

### Conversión de Tipos

**TYPE**

- **Sintaxis:** `TYPE {ANY} [AS {VAR}]`
- **Descripción:** Retorna el tipo del valor dado.
- **Ejemplo:**
    ```bash
    TYPE "Hello" AS $type    # "STRING"
    ```

<br>

**STRING**

- **Sintaxis:** `STRING {ANY} [AS {VAR}]`
- **Descripción:** Convierte un valor a string.
- **Ejemplo:**
    ```bash
    STRING 123 AS $str    # "123"
    ```

<br>

**NUMBER**

- **Sintaxis:** `NUMBER {ANY} [AS {VAR}]`
- **Descripción:** Convierte un valor a entero. Si el valor no puede ser convertido, se lanzará un error.
- **Ejemplo:**
    ```bash
    NUMBER "56" AS $num    # 56
    ```

<br>

**FLOAT**

- **Sintaxis:** `FLOAT {ANY} [AS {VAR}]`
- **Descripción:** Convierte un valor a flotante. Si el valor no puede ser convertido, se lanzará un error.
- **Ejemplo:**
    ```bash
    FLOAT "56.25" AS $float    # 56.25
    ```

<br>

**BOOL**

- **Sintaxis:** `BOOL {ANY} [AS {VAR}]`
- **Descripción:** Convierte un valor a booleano.
- **Ejemplo:**
    ```bash
    BOOL "true" AS $bool    # TRUE
    ```

<br>

**ARRAY**

- **Sintaxis:** `ARRAY {ANY} [AS {VAR}]`
- **Descripción:** Convierte un valor a array.
- **Ejemplo:**
    ```bash
    ARRAY {"name": "Martin", "age": 23} AS $array
    ```

<br>

**MAP**

- **Sintaxis:** `MAP {ANY} [AS {VAR}]`
- **Descripción:** Convierte un valor a mapa.
- **Ejemplo:**
    ```bash
    MAP [["name", "Martin"], ["age", 23]] AS $map
    ```

<br>

### Conversión de Caracteres y Números

**CHAR**

- **Sintaxis:** `CHAR {NUMBER} [AS {VAR}]`

- **Descripción:** Devuelve el carácter que se relaciona con el código Unicode de entrada para la visualización.

- **Ejemplo:**

  ```bash
  CHAR 65 AS $char_value  # A
  ```

<br>

**ORD**

- **Sintaxis:** `ORD {CHAR} [AS {VAR}]`

- **Descripción:** Devuelve el valor Unicode (UTF8) para el carácter.

- **Ejemplo:**

  ```bash
  ORD "A" AS $ord_value   # 65
  ```

<br>

**HEX**

- **Sintaxis:** `HEX {NUMBER} [AS {VAR}]`

- **Descripción:** Convierte un número decimal en su representación hexadecimal.

- **Ejemplo:**

  ```bash
  HEX 255 AS $hex_value
  ```

<br>

**BIN**

- **Sintaxis:** `BIN {NUMBER} [AS {VAR}]`

- **Descripción:** Convierte un número decimal en su representación binaria.

- **Ejemplo:**

  ```bash
  BIN 10 AS $bin_value
  ```

<br>

**OCT**

- **Sintaxis:** `OCT {NUMBER} [AS {VAR}]`

- **Descripción:** Convierte un número decimal en su representación octal.

- **Ejemplo:**

  ```bash
  OCT 64 AS $oct_value
  ```

<br>

**HEX_TO_DEC**

- **Sintaxis:** `HEX_TO_DEC {STRING} [AS {VAR}]`

- **Descripción:** Convierte un valor hexadecimal en su representación decimal.

- **Ejemplo:**

  ```bash
  HEX_TO_DEC "FF" AS $decimal_value
  ```

<br>

**BIN_TO_DEC**

- **Sintaxis:** `BIN_TO_DEC {STRING} [AS {VAR}]`

- **Descripción:** Convierte un valor binario en su representación decimal.

- **Ejemplo:**

  ```bash
  BIN_TO_DEC "1010" AS $decimal_value
  ```

<br>

**OCT_TO_DEC**

- **Sintaxis:** `OCT_TO_DEC {STRING} [AS {VAR}]`

- **Descripción:** Convierte un valor octal en su representación decimal.

- **Ejemplo:**

  ```bash
  OCT_TO_DEC "100" AS $decimal_value
  ```

<br>

## Operaciones Matemáticas

**SUM**

- **Sintaxis:** 
    1. `SUM {NUMBER} TO {NUMBER} [AS {VAR}]`
    2. `SUM {ARRAY} [AS {VAR}]`
- **Descripción:**
    1. Suma dos números.
    2. Suma todos los números en un array.
- **Ejemplos:**
    ```bash
    SUM 2 TO 3 AS $result          # Retorna 5
    SUM [1, 2, 3, 4] AS $result    # Retorna 10
    ```

<br>

**SUBTRACT**

- **Sintaxis:** 
    1. `SUBTRACT {X: NUMBER} WITH {Y: NUMBER} [AS {VAR}]`
    2. `SUBTRACT {ARRAY} [AS {VAR}]`
- **Descripción:** 
    1. Resta dos números.
    2. Resta todos los números en un array secuencialmente.
- **Ejemplos**
    ```bash
    SUBTRACT 5 WITH 3 AS $result      # Retorna 2
    SUBTRACT [10, 3, 2] AS $result    # Retorna 5
    ```

<br>

**MULTIPLY**

- **Sintaxis:** 
    1. `MULTIPLY {X: NUMBER} BY {Y: NUMBER} [AS {VAR}]`
    2. `MULTIPLY {ARRAY} [AS {VAR}]`
- **Descripción:**
    1. Multiplica dos números.
    2. Multiplica todos los números en un array.
- **Ejemplos:**
    ```bash
    MULTIPLY 4 BY 5 AS $result          # Retorna 20
    MULTIPLY [1, 2, 3, 4] AS $result    # Retorna 24
    ```
    <br>

**DIVIDE**

- **Sintaxis:** 
    1. `DIVIDE {X: NUMBER} BY {Y: NUMBER} [AS {VAR}]`
    2. `DIVIDE {ARRAY} [AS {VAR}]`
- **Descripción:** 
    1. Divide dos números.
    2. Divide secuencialmente todos los números en un array.
- **Ejemplo 1:**
    ```bash
    DIVIDE 10 BY 2 AS $result        # Retorna 5
    DIVIDE [100, 2, 5] AS $result    # Retorna 10
    ```

<br>

**MIN**

- **Sintaxis:** `MIN {ARRAY} [AS {VAR}]`
- **Descripción:** Encuentra el valor mínimo en un array.
- **Ejemplo:**
    ```bash
    MIN [3, 1, 4, 1, 5] AS $min    # Retorna 1
    ```

<br>

**MAX**

- **Sintaxis:** `MAX {ARRAY} [AS {VAR}]`
- **Descripción:** Encuentra el valor máximo en un array.
- **Ejemplo:**
    ```bash
    MAX [3, 1, 4, 1, 5] AS $max    # Retorna 5
    ```

<br>

**BOUND**

- **Sintaxis:** `BOUND {NUMBER} WITH {MIN: NUMBER} AND {MAX: NUMBER} [AS {VAR}]`
- **Descripción:** Limita un número a un rango específico.
- **Ejemplo:**
    ```bash
    BOUND 10 WITH 1 AND 5 AS $bounded    # Retorna 5
    ```

<br>

**ROUND**

- **Sintaxis:** `ROUND {NUMBER} [AS {VAR}]`
- **Descripción:** Redondea un número al entero más cercano.
- **Ejemplo:**
    ```bash
    ROUND 3.14 AS $rounded    # Retorna 3
    ```

<br>

**ROUND_UP**

- **Sintaxis:** 
    1. `ROUND_UP {NUMBER} [AS {VAR}]`
    2. `ROUND_UP {NUMBER} TO {DEC} [AS {VAR}]`
- **Descripción:**
    1. Redondea un número hacia arriba al entero más cercano.
    2. Redondea un número hacia arriba con un número específico de decimales.

- **Ejemplos:**
    ```bash
    ROUND_UP 3.14 AS $rounded    # Retorna 4
    ROUND_UP 23.34121215 TO 3 AS $rounded    # Retorna 23.342
    ```

<br>

**ROUND_DOWN**

- **Sintaxis:** 
    1. `ROUND_DOWN {NUMBER} [AS {VAR}]`
    2. `ROUND_DOWN {NUMBER} TO {DEC} [AS {VAR}]`
- **Descripción:**
    1. Redondea un número hacia abajo al entero más cercano.
    2. Redondea un número hacia abajo con un número específico de decimales.
- **Ejemplo:**
    ```bash
    ROUND_DOWN 3.14 AS $rounded    # Retorna 3
    ROUND_DOWN 23.34121215 TO 3 AS $rounded    # Retorna 23.341
    ```

<br>

**FRAC**

- **Sintaxis:** `FRAC {NUMBER} [AS {VAR}]`
- **Descripción:** Retorna la parte decimal de un número. Ideal para obtener la fracción exacta después del punto decimal.
- **Ejemplo:**
    ```bash
    FRAC 3.14 AS $frac    # Retorna 0.14
    ```

<br>

**FRACTIONAL**

- **Sintaxis:** `FRACTIONAL {NUMBER} [AS {VAR}]`
- **Descripción:** Retorna la parte decimal de un número con mayor precisión. Ideal para cálculos que requieren exactitud en la parte decimal.
- **Ejemplo:**
    ```bash
    FRACTIONAL 3.14 AS $frac    # Retorna 0.14000000000000012
    ```

<br>

**POWER / POW**

- **Sintaxis:** `POWER {BASE: NUMBER} TO {EXPONENT: NUMBER} [AS {VAR}]`
- **Descripción:** Eleva un número a la potencia de otro número.
- **Ejemplo:**
    ```bash
    POWER 2 TO 3 AS $result    # Retorna 8
    ```

<br>

**SQRT**

- **Sintaxis:** `SQRT {NUMBER} [AS {VAR}]`
- **Descripción:** Calcula la raíz cuadrada de un número.
- **Ejemplo:**
    ```bash
    SQRT 16 AS $sqrt    # Retorna 4.0
    ```

<br>

**MOD**

- **Sintaxis:** `MOD {DIVIDEND: NUMBER} BY {DIVISOR: NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el módulo (resto) de una división.
- **Ejemplo:**
    ```bash
    MOD 10 BY 3 AS $mod    # Retorna 1
    ```

<br>

**ABS**

- **Sintaxis:** `ABS {NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el valor absoluto de un número.
- **Ejemplo:**
    ```bash
    ABS -5 AS $abs    # Retorna 5
    ```

<br>

**SIN**

- **Sintaxis:** `SIN {RADIANS: NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el seno de un ángulo en radianes.
- **Ejemplo:**
    ```bash
    SIN 1.5708 AS $sin    # Retorna 0.9999999999932537
    ```

<br>

**COS**

- **Sintaxis:** `COS {RADIANS: NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el coseno de un ángulo en radianes.
- **Ejemplo:**
    ```bash
    COS 0 AS $cos    # Retorna 1.0
    ```

<br>

**TAN**

- **Sintaxis:** `TAN {RADIANS: NUMBER} [AS {VAR}]`
- **Descripción:** Calcula la tangente de un ángulo en radianes.
- **Ejemplo:**
    ```bash
    TAN 0.7854 AS $tan    # Retorna 1.0000036732118496
    ```

<br>

**LOG**

- **Sintaxis:** `LOG {NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el logaritmo natural de un número.
- **Ejemplo:**
    ```bash
    LOG 2.7183 AS $log    # Retorna 1
    ```

<br>

**LOGN**

- **Sintaxis:** `LOGN {BASE: NUMBER} OF {NUMBER: NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el logaritmo de un número con una base específica.
- **Ejemplo:**
    ```bash
    LOGN 2 OF 8 AS $logn    # Retorna 3
    ```

<br>

**LOG10**

- **Sintaxis:** `LOG10 {NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el logaritmo base 10 de un número.
- **Ejemplo:**
    ```bash
    LOG10 100 AS $log10    # Retorna 2
    ```

<br>

**FACTORIAL**

- **Sintaxis:** `FACTORIAL {NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el factorial de un número.
- **Ejemplo:**
    ```bash
    FACTORIAL 5 AS $fact    # Retorna 120
    ```

<br>

**EXP**

- **Sintaxis:** `EXP {NUMBER} [AS {VAR}]`
- **Descripción:** Calcula el valor de e elevado a la potencia de un número.
- **Ejemplo:**
    ```bash
    EXP 1 AS $exp    # Retorna 2.7183
    ```

<br>

### Estadísticas

**STAT_MEDIAN**

- **Sintaxis:** `STAT_MEDIAN {ARRAY} [AS {VAR}]`
- **Descripción:** Calcula la mediana de un array de números.
- **Ejemplo:**
    ```bash
    STAT_MEDIAN [1, 3, 3, 6, 7, 8, 9] AS $median    # Retorna 6
    ```

<br>

**STAT_AVERAGE**

- **Sintaxis:** `STAT_AVERAGE {ARRAY} [AS {VAR}]`
- **Descripción:** Calcula el promedio de un array de números.
- **Ejemplo:**
    ```bash
    STAT_AVERAGE [1, 2, 3, 4, 5] AS $avg    # Retorna 3
    ```

<br>

**STAT_VARIANCE**

- **Sintaxis:** `STAT_VARIANCE {ARRAY} [AS {VAR}]`
- **Descripción:** Calcula la varianza de un array de números.
- **Ejemplo:**
    ```bash
    STAT_VARIANCE [1, 2, 3, 4, 5] AS $variance    # Retorna 2.5
    ```

<br>

**STAT_STDEV**

- **Sintaxis:** `STAT_STDEV {ARRAY} [AS {VAR}]`
- **Descripción:** Calcula la desviación estándar de un array de números.
- **Ejemplo:**
    ```bash
    STAT_STDEV [1, 2, 3, 4, 5] AS $stdev    # Retorna 1.5811
    ```

<br>

**STAT_MODE**

- **Sintaxis:** `STAT_MODE {ARRAY} [AS {VAR}]`
- **Descripción:** Encuentra el valor que más se repite en un array.
- **Ejemplo:**
    ```bash
    STAT_MODE [1, 2, 2, 3, 4] AS $mode    # Retorna 2
    ```
    <br>

## Patrones y Expresiones Regulares

**PATTERN**

- **Sintaxis:** 
    1. `PATTERN {STRING} [AS {VAR}]`
    2. `PATTERN {STRING} WITH {FLAGS} [AS {VAR}]`
- **Descripción:** 
    1. Crea un patrón (expresión regular) a partir de un string.
    2. Crea un patrón (expresión regular) a partir de un string con un array de flags.
- **Ejemplo:**
    ```bash
    PATTERN "[0-9]+" AS $pattern
    PATTERN "[0-9]+" WITH [IGNORECASE, MULTILINE] AS $pattern
    PATTERN "[0-9]+" WITH ["i", "m"] AS $pattern
    ```

<br>

**PATTERN_INFO**

- **Sintaxis:** `PATTERN_INFO {PATTERN} [AS {VAR}]`
- **Descripción:** Retorna un mapa con información acerca del patrón.
- **Ejemplo:**
    ```bash
    PATTERN_INFO $pattern AS $details
    ```

<br>

**MATCH**

- **Sintaxis:** `MATCH {PATTERN} IN {STRING} [AS {VAR}]`
- **Descripción:** Encuentra todas las coincidencias del patrón en el texto, similar a `re.findall` en Python o `match` en JavaScript.
- **Ejemplo:**
    ```bash
    MATCH $pattern IN "abc123xyz" AS $matches    # Retorna ["123"]
    ```

<br>

**MATCH_FIRST**

- **Sintaxis:** `MATCH_FIRST {PATTERN} IN {STRING} [AS {VAR}]`
- **Descripción:** Encuentra la primera coincidencia del patrón en el texto.
- **Ejemplo:**
    ```bash
    MATCH_FIRST $pattern IN "abc123xyz456" AS $first_match
    ```

<br>

**MATCH_ALL**

- **Sintaxis:** `MATCH_ALL {PATTERN} IN {STRING} [AS {VAR}]`
- **Descripción:** Encuentra todas las coincidencias del patrón en el texto y devuelve un mapa de resultados con sus posiciones, similar a `re.finditer` en Python.
- **Ejemplo:**
    ```bash
    MATCH_ALL $pattern IN "abc123xyz456" AS $all_matches 
    ```

<br>

**MATCH_FULL**

- **Sintaxis:** `MATCH_FULL {PATTERN} IN {STRING} [AS {VAR}]`
- **Descripción:** Verifica si el texto completo coincide con el patrón.
- **Ejemplo:**
    ```bash
    MATCH_FULL $pattern IN "123" AS $is_full_match    # Retorna TRUE si coincide todo el texto, de lo contrario FALSE
    ```

<br>

**COUNT**

- **Sintaxis:** `COUNT {PATTERN} IN {STRING} [AS {VAR}]`
- **Descripción:** Cuenta cuántas veces el patrón coincide en el texto.
- **Ejemplo:**
    ```bash
    PATTERN "\d+" AS $pattern
    COUNT $pattern IN "abc123xyz456" AS $count    # Retorna 2
    ```

<br>

**SPLIT**

- **Sintaxis:** `SPLIT {STRING} BY {PATTERN} [AS {VAR}]`
- **Descripción:** Divide un string utilizando un patrón como delimitador.
- **Ejemplo:**
    ```bash
    PATTERN "\d+" AS $pattern 
    SPLIT "hello45world77world22hello" BY $pattern AS $parts    # Retorna ["hello", "world", "world", "hello"]
    ```

<br>

**FIND**

- **Sintaxis:** `FIND {PATTERN} IN {STRING} [AS {VAR}]`
- **Descripción:** Encuentra la posición de la primera coincidencia del patrón en el string.
- **Ejemplo:**
    ```bash
    PATTERN "\d+" AS $pattern 
    FIND $pattern IN "abc123xyz" AS $position    # Retorna 4
    ```

<br>

**REPLACE**

- **Sintaxis:** `REPLACE {PATTERN} WITH {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Reemplaza la primera coincidencia del patrón en el texto con otro string.
- **Ejemplo:**
    ```bash
    PATTERN "[0-9]+" AS $pattern
    REPLACE $pattern WITH "X" IN "abc123xyz456" AS $new_text    # Retorna abcXxyz456
    ```
    <br>

**REPLACE_ALL**

- **Sintaxis:** `REPLACE_ALL {PATTERN} WITH {SUBSTR} IN {STRING} [AS {VAR}]`
- **Descripción:** Reemplaza todas las coincidencias del patrón en el texto con otro string.
- **Ejemplo:**
    ```bash
    PATTERN "[0-9]+" AS $pattern
    REPLACE_ALL $pattern WITH "X" IN "abc123xyz456" AS $new_text    # Retorna abcXxyzX
    ```

<br>


## Archivos y Directorios

### Manipulación de Archivos

**FILE_EXISTS**

- **Sintaxis:** `FILE_EXISTS {FILE: STRING} [AS {VAR}]`
- **Descripción:** Verifica si un archivo existe en la ruta especificada.
- **Ejemplo:**
    ```bash
    FILE_EXISTS "example.txt" AS $exists
    ```

<br>

**FILE_DELETE**

- **Sintaxis:** `FILE_DELETE {FILE: STRING}`
- **Descripción:** Elimina un archivo especificado.
- **Ejemplo:**
    ```bash
    FILE_DELETE "example.txt"
    ```

<br>

**FILE_MOVE**

- **Sintaxis:** `FILE_MOVE {FILE: STRING} TO {DEST}`
- **Descripción:** Mueve un archivo (`FILE`) a una nueva ubicación (`DEST`).
- **Ejemplo:**
    ```bash
    FILE_MOVE "example.txt" TO "new_folder/example.txt"
    ```

<br>

**FILE_RENAME**

- **Sintaxis:** `FILE_RENAME {OLD: STRING} TO {NEW: STRING}`
- **Descripción:** Renombra un archivo especificado.
- **Ejemplo:**
    ```bash
    FILE_RENAME "old_name.txt" TO "new_name.txt"
    ```

<br>

**FILE_SIZE**

- **Sintaxis:** `FILE_SIZE {FILE: STRING} [AS {VAR}]`
- **Descripción:** Obtiene el tamaño de un archivo en bytes.
- **Ejemplo:**
    ```bash
    FILE_SIZE "example.txt" AS $size
    ```

<br>

**FILE_OPEN**

- **Sintaxis:** `FILE_OPEN {FILE: STRING}`
- **Descripción:** Abre un archivo para su lectura o escritura, retornando el ID del archivo.
- **Ejemplo:**
    ```bash
    FILE_OPEN "example.txt" AS $file_id
    ```

<br>

**FILE_WRITE**

- **Sintaxis:** `FILE_WRITE {ID: NUMBER} WITH {STRING}`
- **Descripción:** Escribe texto en un archivo abierto.
- **Ejemplo:**
    ```bash
    FILE_WRITE $file_id WITH "Hello, world!"
    ```

<br>

**FILE_READ**

- **Sintaxis:** `FILE_READ {ID: NUMBER} [AS {VAR}]`
- **Descripción:** Lee el contenido de un archivo abierto.
- **Ejemplo:**
    ```bash
    FILE_READ $file_id AS $content
    ```

<br>

**FILE_APPEND**

- **Sintaxis:** `FILE_APPEND {ID: NUMBER} WITH {STRING}`
- **Descripción:** Añade texto al final de un archivo abierto.
- **Ejemplo:**
    ```bash
    FILE_APPEND $file_id WITH "More text."
    ```

<br>

**FILE_CLOSE**

- **Sintaxis:** `FILE_CLOSE {ID: NUMBER}`
- **Descripción:** Cierra un archivo abierto.
- **Ejemplo:**
    ```bash
    FILE_CLOSE $file_id
    ```

<br>



### Manipulación de Directorios

**DIRECTORY_CREATE**

- **Sintaxis:** `DIRECTORY_CREATE {DIR: STRING}`
- **Descripción:** Crea un nuevo directorio.
- **Ejemplo:**
    ```bash
    DIRECTORY_CREATE "new_folder"
    ```

<br>

**DIRECTORY_DELETE**

- **Sintaxis:** `DIRECTORY_DELETE {DIR: STRING}`
- **Descripción:** Elimina un directorio especificado.
- **Ejemplo:**
    ```bash
    DIRECTORY_DELETE "old_folder"
    ```

<br>

**DIRECTORY_RENAME**

- **Sintaxis:** `DIRECTORY_RENAME {OLD: STRING} TO {NEW: STRING}`
- **Descripción:** Renombra un directorio especificado.
- **Ejemplo:**
    ```bash
    DIRECTORY_RENAME "old_folder" TO "new_folder"
    ```

<br>

**DIRECTORY_EXISTS**

- **Sintaxis:** `DIRECTORY_EXISTS {DIR: STRING} [AS {VAR}]`
- **Descripción:** Verifica si un directorio existe en la ruta especificada.
- **Ejemplo:**
    ```bash
    DIRECTORY_EXISTS "example_folder" AS $exists
    ```

<br>

**DIRECTORY_COPY**

- **Sintaxis:** `DIRECTORY_COPY {DIR: STRING} TO {DEST}`
- **Descripción:** Copia un directorio a una nueva ubicación.
- **Ejemplo:**
    ```bash
    DIRECTORY_COPY "example_folder" TO "backup_folder"
    ```

<br>

**DIRECTORY_MOVE**

- **Sintaxis:** `DIRECTORY_MOVE {DIR: STRING} TO {DESC}`
- **Descripción:** Mueve un directorio a una nueva ubicación.
- **Ejemplo:**
    ```bash
    DIRECTORY_MOVE "example_folder" TO "new_location"
    ```

<br>

**DIRECTORY_SIZE**

- **Sintaxis:** `DIRECTORY_SIZE {DIR: STRING} [AS {VAR}]`
- **Descripción:** Obtiene el tamaño total de un directorio y su contenido.
- **Ejemplo:**
    ```bash
    DIRECTORY_SIZE "example_folder" AS $size
    ```

<br>

**DIRECTORY_LIST**

- **Sintaxis:** `DIRECTORY_LIST {DIR: STRING} [AS {VAR}]`
- **Descripción:** Lista todos los archivos y directorios en un directorio.
- **Ejemplo:**
    ```bash
    DIRECTORY_LIST "example_folder" AS $items
    ```

<br>

**DIRECTORY_LIST_FILES**

- **Sintaxis:** `DIRECTORY_LIST_FILES {DIR: STRING} [AS {VAR}]`
- **Descripción:** Lista solo los archivos en un directorio.
- **Ejemplo:**
    ```bash
    DIRECTORY_LIST_FILES "example_folder" AS $files
    ```

<br>

**DIRECTORY_LIST_DIRS**

- **Sintaxis:** `DIRECTORY_LIST_DIRS {DIR: STRING} [AS {VAR}]`
- **Descripción:** Lista solo los subdirectorios en un directorio.
- **Ejemplo:**
    ```bash
    DIRECTORY_LIST_DIRS "example_folder" AS $dirs
    ```

<br>

### Manipulación de Rutas de Archivos

**PATH_FILENAME**

- **Sintaxis:** `PATH_FILENAME {PATH: STRING} [AS {VAR}]`
- **Descripción:** Obtiene el nombre del archivo de una ruta completa.
- **Ejemplo:**
    ```bash
    PATH_FILENAME "/path/to/file.txt" AS $filename  # file.txt
    ```

<br>

**PATH_EXT**

- **Sintaxis:** `PATH_EXT {PATH: STRING} [AS {VAR}]`
- **Descripción:** Obtiene la extensión del archivo de una ruta completa.
- **Ejemplo:**
    ```bash
    PATH_EXT "/path/to/file.txt" AS $extension  # .txt
    ```

<br>

**PATH_DIR**

- **Sintaxis:** `PATH_DIR {PATH: STRING} [AS {VAR}]`
- **Descripción:** Obtiene el directorio de una ruta completa.
- **Ejemplo:**
    ```bash
    PATH_DIR "/path/to/file.txt" AS $directory  # /path/to
    ```

<br>

**PATH_JOIN**

- **Sintaxis:** `PATH_JOIN {ARRAY} [AS {VAR}]`
- **Descripción:** Combina elementos de una ruta en un solo string.
- **Ejemplo:**
    ```bash
    PATH_JOIN ["/path", "to", "file.txt"] AS $full_path # path/to/file.txt
    ```

<br>

**PATH_ABSOLUTE**

- **Sintaxis:** `PATH_ABSOLUTE {PATH: STRING} [AS {VAR}]`
- **Descripción:** Convierte una ruta relativa en una ruta absoluta.
- **Ejemplo:**
    ```bash
    PATH_ABSOLUTE "relative/path" AS $absolute_path # C:\Users\PC\example\relative\path
    ```

<br>

**PATH_RELATIVE**

- **Sintaxis:** 
    1. `PATH_RELATIVE {PATH: STRING} [AS {VAR}]`
    2. `PATH_RELATIVE {PATH: STRING} FROM {STARTDIR: STRING} [AS {VAR}]`
- **Descripción:** 
    1. Convierte una ruta absoluta en una ruta relativa.
    2. Convierte una ruta absoluta en una ruta relativa basada en un directorio de inicio específico.
- **Ejemplos:**
    ```bash
    PATH_RELATIVE "/absolute/path" AS $relative_path
    PATH_RELATIVE "/absolute/path" FROM "/start/dir" AS $relative_path
    ```

<br>

**PATH_PARENT**

- **Sintaxis:** `PATH_PARENT {PATH: STRING} [AS {VAR}]`
- **Descripción:** Obtiene el directorio padre de una ruta especificada.
- **Ejemplo:**
    ```bash
    PATH_PARENT "/path/to/file.txt" AS $parent_dir
    ```

<br>

**PATH_NORMALIZE**

- **Sintaxis:** `PATH_NORMALIZE {PATH: STRING} [AS {VAR}]`
- **Descripción:** Normaliza una ruta de archivo eliminando los componentes redundantes.
- **Ejemplo:**
    ```bash
    PATH_NORMALIZE "/path/./to/../file.txt" AS $normalized_path
    ```

<br>

**PATH_SPLIT**

- **Sintaxis:** `PATH_SPLIT {PATH: STRING} [AS {VAR}]`
- **Descripción:** Divide una ruta de archivo en componentes (nombre de archivo, directorio, extensión).
- **Ejemplo:**
    ```bash
    PATH_SPLIT "/path/to/file.txt" AS $components   # ["file", "/path/to", ".txt"]
    ```

<br>

### Manipulación de JSON

**JSON_PARSE**

- **Sintaxis:** `JSON_PARSE {STRING} [AS {VAR}]`
- **Descripción:** Convierte un string (en formato JSON) en un mapa.
- **Ejemplo:**
    ```bash
    JSON_PARSE '{"key": "value"}' AS $parsed_map
    PRINTF $parsed_map
    ```

<br>

**JSON_TO_STRING**

- **Sintaxis:** `JSON_TO_STRING {MAP} [AS {VAR}]`
- **Descripción:** Convierte un mapa en un string en formato JSON.
- **Ejemplo:**
    ```bash
    JSON_TO_STRING {"key": "value"} AS $json_string
    ```

<br>

**JSON_TO_PRETTY**

- **Sintaxis:** `JSON_TO_PRETTY {MAP} WITH {SPACES: STRING} [AS {VAR}]`
- **Descripción:** Convierte un mapa en un string en formato JSON definiendo los espacios.
- **Ejemplo:**
    ```bash
    JSON_TO_PRETTY {"key": "value"} WITH 4 AS $json_string
    ```

<br>

**JSON_LOAD**

- **Sintaxis:** `JSON_LOAD {FILE: STRING} [AS {VAR}]`
- **Descripción:** Carga un archivo JSON y lo convierte en un mapa.
- **Ejemplo:**
    ```bash
    JSON_LOAD "data.json" AS $data_map
    ```

<br>

**JSON_SAVE**

- **Sintaxis:** `JSON_SAVE {MAP} TO {FILE: STRING}`
- **Descripción:** Guarda un mapa en un archivo en formato JSON.
- **Ejemplo:**
    ```bash
    JSON_SAVE {"key": "value"} TO "output.json"
    ```
    

<br>

## Valores Aleatorios

**RANDOM**

- **Sintaxis:**
    1. `RANDOM {N: STRING} [AS {VAR}]`
    2. `RANDOM FROM {X: NUMBER} TO {Y: NUMBER} [AS {VAR}]`
- **Descripción:**
    1. Genera un número aleatorio entre 0 y el valor especificado.
    2. Genera un número aleatorio dentro del rango especificado entre `X` y `Y`.
- **Ejemplos:**
    ```bash
    RANDOM 100 AS $random_number
    RANDOM FROM 1 TO 10 AS $random_range
    ```

<br>


**RANDOM_CHOICE**

- **Sintaxis:** `RANDOM_CHOICE {ARRAY} [AS {VAR}]`
- **Descripción:** Selecciona un elemento aleatorio de un array.
- **Ejemplo:**
    ```bash
    RANDOM_CHOICE [1, 2, 3, 4] AS $random_element
    ```


<br>

## Control de Flujo

### Condicionales

**WHEN**

- **Sintaxis:** `WHEN {CONDITION: ANY}`

- **Descripción:** Ejecuta un bloque de código si la condición especificada es verdadera.

- **Ejemplo:**

  ```bash
  WHEN $count > 5
      PRINT "El valor es mayor que 5"
  ```

<br>

**ORWHEN**

- **Sintaxis:** `ORWHEN {CONDITION: ANY}`

- **Descripción:** Se utiliza para añadir una condición alternativa en una estructura de control `WHEN`, de modo que si alguna de las condiciones especificadas es verdadera, se ejecutará el bloque de código correspondiente.

- **Ejemplo:**

  ```bash
  WHEN $count > 10
      PRINT "El valor es mayor que 10"
  ORWHEN $count == 10
      PRINT "El valor es exactamente 10"
  ```

<br>

**AND**

- **Sintaxis:** `AND {CONDITION: ANY}`

- **Descripción:** Se utiliza junto con `WHEN` o `ORWHEN` para combinar múltiples condiciones con una lógica `AND`. 

- **Ejemplo:**

  ```bash
  WHEN $count > 5
  AND $count < 10
      PRINT "El valor está entre 5 y 10"
  ```

<br>

**OR**

- **Sintaxis:** `OR {CONDITION: ANY}`

- **Descripción:** Se utiliza junto con `WHEN` o `ORWHEN` para combinar múltiples condiciones con una lógica `OR`.

- **Ejemplo:**

  ```bash
  WHEN $count < 5
  OR $count > 10
      PRINT "El valor está fuera del rango de 5 a 10"
  ```

<br>

**ELSE**

- **Sintaxis:** `ELSE`

- **Descripción:** Se usa para especificar un bloque de código que se ejecutará si ninguna de las condiciones anteriores con `WHEN` o `ORWHEN` es verdadera.

- **Ejemplo:**

  ```bash
  WHEN $count > 10
      PRINT "El valor es mayor que 10"
  ELSE
      PRINT "El valor es 10 o menor"
  ```

<br>



### Bucles

**WHILE**

- **Sintaxis:** `WHILE {CONDITION: ANY}`
- **Descripción:** Ejecuta un bloque de código mientras se cumpla la condición especificada.
- **Tipo:** Comando compuesto.
- **Ejemplo:**
    ```bash
    WHILE $count < 10
        PRINT $count
        INCREMENT $count
    ```

<br>

**ITERATE**

- **Sintaxis:** 
    1. `ITERATE {STRING|ARRAY} [AS {VAR}]`
    2. `ITERATE FROM {X: NUMBER} TO {Y: NUMBER} [AS {VAR}]`
- **Descripción:** Itera sobre los elementos de un string o array, o sobre un rango de índices. Para arrays y strings, el valor actual se almacena en la variable especificada.
- **Ejemplo:**
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

- **Sintaxis:** `REPEAT {NUMBER}`
- **Descripción:** Ejecuta un bloque de código un número específico de veces.
- **Ejemplo:**
    ```bash
    REPEAT 5
        PRINT "Repetición"
    ```

<br>

**BREAK**

- **Sintaxis:** `BREAK`
- **Descripción:** Sale del bucle o del bloque `ITERATE` o `WHILE` en el que se encuentra.
- **Ejemplo:**
    ```bash
    WHILE $count < 10
        PRINT $count
        WHEN $count == 5
            BREAK
        INCREMENT $count
    ```

<br>

**CONTINUE**

- **Sintaxis:** `CONTINUE`
- **Descripción:** Salta a la siguiente iteración del bucle o del bloque `ITERATE` o `WHILE`.
- **Ejemplo:**
    ```bash
    WHILE $count < 10
        WHEN $count % 2 == 0
            INCREMENT $count
            CONTINUE
        PRINT $count
        INCREMENT $count
    ```

<br>

### Definiciones

**DEFINE**

- **Sintaxis:** `DEFINE {NAME: STRING}`

- **Descripción:** Define un comando personalizado con el nombre especificado.

- **Ejemplo:**

  ```bash
  DEFINE GREET
      PRINT "Hello, World!"
  ```

<br>

**PARAMS**

- **Sintaxis:** `PARAMS {ARRAY}`

- **Descripción:** Especifica los parámetros que el comando `DEFINE` recibirá. Los parámetros deben estar en un array.

- **Ejemplo:**

  ```bash
  DEFINE GREET
      PARAMS [STRING, NUMBER]
      PRINT "Hello, " + @PARAMS.1 + ". You have " + @PARAMS.2 +" new messages."
  
  GREET "Martin" 32   # Hello, Martin. You have 32 new messages.
  ```

<br>

**RETURN**

- **Sintaxis:** `RETURN {ANY}`

- **Descripción:** Retorna un valor desde un comando definido con `DEFINE`.

- **Ejemplo:**

  ```bash
  DEFINE ADD
      PARAMS [NUMBER, NUMBER]
      SET $result AS @PARAMS.1 + @PARAMS.2
      RETURN $result
  ```

<br>

**ON**

- **Sintaxis:** `ON {EVENT}`

- **Descripción:** Ejecuta un bloque de código en un determinado evento. Visite la sección [Eventos](Guide_ES.md#eventos) para más información.

- **Ejemplo:**

  ```bash
  ON START
      PRINT "Hello"
  
  ON END
      PRINT "Bye"
  ```

<br>

### Módulos

**IMPORT**

- **Sintaxis:** `IMPORT {FILE: STRING}`
- **Descripción:** Importa un archivo que contiene definiciones de comandos, variables o datos que pueden ser utilizados en el código actual. Este comando permite la reutilización de código y la modularización en tu proyecto. También puedes importar librerías.
- **Ejemplos:**
    ```bash
    IMPORT "myfunctions.stal"
    IMPORT "myfunctions"    # The extension can be omitted
    ```

<br>

**EXPORT**

- **Sintaxis:** 
    1. `EXPORT {VAR}`
    3. `EXPORT {CMD: STRING}`
    
- **Descripción:** Exporta una variable o un comando (como string) para que puedan ser utilizados en otros archivos o contextos. Este comando es útil para compartir datos o comandos entre diferentes partes del proyecto o con librerías.

- **Ejemplo:**
    ```bash
    EXPORT $my_variable      # Exportar una variable
    EXPORT "MYFUNCTION"     # Exportar un comando
    ```
    <br>



### Manejo de Errores

**TRY**

- **Sintaxis:** `TRY`
- **Descripción:** Inicia un bloque de código que puede generar errores, que se manejarán con `CATCH`.
- **Ejemplo:**
    ```bash
    TRY
        PRINT 1 / 0
    ```

<br>

**CATCH**

- **Sintaxis:** `CATCH [AS {VAR}]`
- **Descripción:** Captura y maneja errores generados en el bloque `TRY`. El error capturado se almacena en la variable especificada.
- **Ejemplo:**
    ```bash
    TRY
        PRINT 1 / 0
    CATCH AS $error
        PRINTF $error
    ```

<br>

**ERROR**

- **Sintaxis:** `ERROR {STRING}`
- **Descripción:** Lanza un error de tipo genérico.
- **Ejemplo:**
    ```bash
    SET $n AS 0
    WHEN $n IS LEQ 0
        ERROR "El número debe ser mayor que 0."
    ```

<br>

**THROW**

- **Sintaxis:** `THROW {ARRAY}`
- **Descripción:** Lanza un error de tipo personalizado. Normalmente usado con comandos como `SYNTAX_ERROR`, `VALUE_ERROR`, `RUNTIME_ERROR`, etc.
- **Ejemplo:**
    ```bash
    THROW VALUE_ERROR ["El valor es inváldo"];
    ```

<br>

**SYNTAX_ERROR** — **VALUE_ERROR** — **TYPE_ERROR** — **RUNTIME_ERROR** — **NOT_FOUND_ERROR** — **FILESYSTEM_ERROR**

- **Sintaxis:** `COMMAND {ARRAY}`
- **Descripción:** Reciben un array como parámetro, donde el primer elemento representa la descripción del error y el segundo índice el nombre del error. Devuelve un mapa con un error de tipo `SyntaxError`, `ValueError`, `TypeError`, `RuntimeError`, `NotFoundError` y `FilesystemError` respectivamente. 
- **Ejemplo:**
    ```bash
    THROW SYNTAX_ERROR ["Invalid Syntax"];
    THROW VALUE_ERROR ["Invalid value"];
    THROW TYPE_ERROR ["Unsupported Type"];
    THROW RUNTIME_ERROR ["Missing values", "MISSING_PARAMETERS"];
    ```
    <br>

## Entrada/Salida

### Contenido 

**INPUT**

- **Sintaxis:**
    1. `INPUT {STRING}`
    2. `INPUT FROM {FILENAME}`
- **Descripción:**  Carga un string de entrada desde un string o un archivo.
- **Ejemplo:**
    ```bash
    INPUT "Hello, world!"
    INPUT FROM "data.txt"
    ```

<br>

**STORE**

- **Sintaxis:** `STORE {STRING}`
- **Descripción:** Almacena un string como una línea, para luego ser exportado como un archivo utilizando el comando `OUTPUT`.
- **Ejemplo:**
    ```bash
    STORE "Este es el contenido a guardar."
    ```

<br>

**CLEAR_STORAGE**

- **Sintaxis:** `CLEAR_STORAGE`
- **Descripción:** Vacía el almacenamiento global de líneas, elimina todas las cadenas almacenadas y restablece el almacenamiento a un estado vacío.
- **Ejemplo:**
    ```bash
    CLEAR_STORAGE
    ```

<br>

**OUTPUT**

- **Sintaxis:** `OUTPUT {FILE: STRING}`
- **Descripción:** Exporta todo contenido almacenado a un archivo con el nombre especificado.
- **Ejemplo:**
    ```bash
    INPUT "file.txt"
    
    ON EACH_LINE
        STORE @LINE
    
    OUTPUT "resultado.txt"
    ```

<br>



### Misc

**PRINT**

- **Sintaxis:** `PRINT {ANY}`
- **Descripción:** Imprime el valor de la variable o texto especificado en la salida estándar.
- **Ejemplo:**
    ```bash
    PRINT "Hello, world!"
    ```

<br>

**PRINTF**

- **Sintaxis:** `PRINTF {ANY}`
- **Descripción:** Imprime el valor especificado con formato, con una presentación más estructurada y legible.
- **Ejemplo:**
    ```bash
    PRINTF {"name": "John", "age": 30}
    ```

<br>

**PRINTL**

- **Sintaxis:** `PRINTL {ARRAY}`
- **Descripción:** Imprime los elementos de un array en una sola línea, separados por espacios.
- **Ejemplo:**
    ```bash
    PRINTL ["hola", "como", "estas"]  # Output: hola como estas
    ```



<br>

## Fechas y Tiempos

### Generar y Formatear Fechas

**DATE_CREATE**

- **Sintaxis:** `DATE_CREATE {STRING|ARRAY} [AS {VAR}]`
- **Descripción:** Crea una nueva fecha a partir de un string o array. Retorna un mapa de fecha.
    
    Si se usa un array, este debe contener 7 elementos: año, mes, día, hora, minuto, segundo y microsegundo. 

    Si se usa un string, este debe estar estructurado con un formato válido como: `YYYY-MM-DD`, `MM-DD-YYYY`, `MM/DD/YYYY`, `YYYY-MM-DD HH:mm:ss`, etc.
    
- **Ejemplo:**
    ```bash
    DATE_CREATE "2020-10-10"
    DATE_CREATE [2024, 8, 12, 12, 0, 0, 0] AS $date
    ```

<br>

**DATE_NOW**

- **Sintaxis:** `DATE_NOW [AS {VAR}]`
- **Descripción:** Obtiene la fecha y hora actuales. Retorna un mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_NOW AS $current_date
    ```

<br>

**DATE_FORMAT**

- **Sintaxis:** 
    1. `DATE_FORMAT {DATE: MAP} [AS {VAR}]`
    2. `DATE_FORMAT {DATE: MAP} TO {FORMAT} [AS {VAR}]`
- **Descripción:** Formatea un mapa de fecha según el formato global o un formato especificado.
- **Ejemplo:**
    ```bash
    DATE_CREATE "2024-06-03" AS $date
    DATE_FORMAT $date # Formato global
    DATE_FORMAT $date TO "MM/DD/YYYY" AS $formatted_date 
    ```

<br>

**DATE_FORMAT_NOW**

- **Sintaxis:** `DATE_FORMAT_NOW [AS {VAR}]`
- **Descripción:** Formatea la fecha y hora actuales según el formato global.
- **Ejemplo:**
    ```bash
    DATE_FORMAT_NOW AS $formatted_now
    ```

<br>


### Comparaciones y Verificaciones

**IS_DATE**

- **Sintaxis:** `IS_DATE {ANY} [AS {VAR}]`
- **Descripción:** Verifica si el valor especificado es una mapa de fecha válido.
- **Ejemplo:**
    ```bash
    IS_DATE $date AS $is_valid_date
    ```

<br>

**DATE_IS_LEAP_YEAR**

- **Sintaxis:** `DATE_IS_LEAP_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Verifica si el año especificado es un año bisiesto.
- **Ejemplo:**
    ```bash
    DATE_IS_LEAP_YEAR $date
    ```

<br>

**DATE_COMPARE**

- **Sintaxis:** `DATE_COMPARE {DATE: MAP} AND {DATE: MAP} [AS {VAR}]`
- **Descripción:** Compara dos fechas y devuelve 1 si la primera es mayor, -1 si es menor y 0 si son iguales.
- **Ejemplo:**
    ```bash
    DATE_COMPARE $date1 AND $date2
    ```

<br>

**DATE_IS_BEFORE**

- **Sintaxis:** `DATE_IS_BEFORE {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Verifica si `DATE1` es anterior a `DATE2`.
- **Ejemplo:**
    ```bash
    DATE_IS_BEFORE $date1 AND $date2
    ```

<br>

**DATE_IS_AFTER**

- **Sintaxis:** `DATE_IS_AFTER {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Verifica si `DATE1` es posterior a `DATE2`.
- **Ejemplo:**
    ```bash
    DATE_IS_AFTER $date1 AND $date2
    ```

<br>

**DATE_IS_SAME**

- **Sintaxis:** `DATE_IS_SAME {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Verifica si `DATE1` y `DATE2` son iguales.
- **Ejemplo:**
    ```bash
    DATE_IS_SAME $date1 AND $date2
    ```

<br>

**DATE_SINCE**

- **Sintaxis:** `DATE_SINCE {DATE: MAP} [AS {VAR}]`

- **Descripción:** Obtiene el tiempo transcurrido desde la fecha especificada hasta ahora.

- **Ejemplo:**

  ```bash
  DATE_CREATE "2024-06-03" AS $date
  DATE_SINCE $date AS $elapsed_time    # 2 months ago 
  ```

<br>

**DATE_DISTANCE**

- **Sintaxis:** 

  1. `DATE_DISTANCE {DATE: MAP} [AS {VAR}]`
  2. `DATE_DISTANCE {DATE: MAP} TO {DATE: MAP} [AS {VAR}]`

- **Descripción:**

  1. Obtiene la distancia de tiempo transcurrido desde la fecha especificada hast ahora.
  2. Obtiene la distancia de tiempo entre dos fechas.

- **Ejemplo:**

  ```bash
  DATE_DISTANCE $date1 AS $distance    # 1 year
  DATE_DISTANCE $date1 TO $date2 AS $time_distance    # 4 years
  ```

<br>

### Diferencias de Fechas

**DATE_DIFF_YEARS**

- **Sintaxis:** `DATE_DIFF_YEARS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en años entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_YEARS $date1 AND $date2
    ```

<br>

**DATE_DIFF_MONTHS**

- **Sintaxis:** `DATE_DIFF_MONTHS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en meses entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_MONTHS $date1 AND $date2
    ```

<br>

**DATE_DIFF_WEEKS**

- **Sintaxis:** `DATE_DIFF_WEEKS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en semanas entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_WEEKS $date1 AND $date2
    ```

<br>

**DATE_DIFF_DAYS**

- **Sintaxis:** `DATE_DIFF_DAYS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en días entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_DAYS $date1 AND $date2
    ```

<br>

**DATE_DIFF_HOURS**

- **Sintaxis:** `DATE_DIFF_HOURS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en horas entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_CREATE "2024-01-01T12:00:00" AS $date1
    DATE_CREATE "2020-01-01T12:00:00" AS $date2
    DATE_DIFF_HOURS $date1 AND $date2 AS $diff
    ```

<br>

**DATE_DIFF_MINUTES**

- **Sintaxis:** `DATE_DIFF_MINUTES {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en minutos entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_MINUTES $date1 AND $date2
    ```

<br>

**DATE_DIFF_SECONDS**

- **Sintaxis:** `DATE_DIFF_SECONDS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en segundos entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_SECONDS $date1 AND $date2
    ```

<br>

**DATE_DIFF_MICROSECONDS**

- **Sintaxis:** `DATE_DIFF_MICROSECONDS {DATE1: MAP} AND {DATE2: MAP} [AS {VAR}]`
- **Descripción:** Calcula la diferencia en microsegundos entre dos fechas.
- **Ejemplo:**
    ```bash
    DATE_DIFF_MICROSECONDS $date1 AND $date2
    ```

<br>

### Componentes de Fechas

**DATE_GET_YEAR**

- **Sintaxis:** `DATE_GET_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el año de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_YEAR $date AS $year
    ```

<br>

**DATE_GET_MONTH**

- **Sintaxis:** `DATE_GET_MONTH {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el mes de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_MONTH $date AS $month
    ```

<br>

**DATE_GET_WEEK**

- **Sintaxis:** `DATE_GET_WEEK {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene la semana del año de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_WEEK $date AS $week
    ```

<br>

**DATE_GET_DAY**

- **Sintaxis:** `DATE_GET_DAY {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el día del mes de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_DAY $date AS $day
    ```

<br>

**DATE_GET_HOUR**

- **Sintaxis:** `DATE_GET_HOUR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene la hora de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_HOUR $date AS $hour
    ```

<br>

**DATE_GET_MINUTE**

- **Sintaxis:** `DATE_GET_MINUTE {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el minuto de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_MINUTE $date AS $minute
    ```

<br>

**DATE_GET_SECOND**

- **Sintaxis:** `DATE_GET_SECOND {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el segundo de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_SECOND $date" AS $second
    ```

<br>

**DATE_GET_MICROSECOND**

- **Sintaxis:** `DATE_GET_MICROSECOND {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el microsegundo de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_MICROSECOND $date AS $microsecond
    ```

<br>

**DATE_GET_WEEKDAY**

- **Sintaxis:** `DATE_GET_WEEKDAY {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el número del día de la semana de la fecha especificada (1 a 7).
- **Ejemplo:**
    ```bash
    DATE_NOW AS $date
    DATE_GET_WEEKDAY $date AS $weekday  # 5
    ```

<br>

**DATE_GET_WEEKDAY_NAME**

- **Sintaxis:** `DATE_GET_WEEKDAY_NAME {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el nombre del día de la semana de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_NOW AS $date
    DATE_GET_WEEKDAY_NAME $date AS $weekday_name    # Friday
    ```

<br>

**DATE_GET_DAYS_IN_MONTH**

- **Sintaxis:** `DATE_GET_DAYS_IN_MONTH {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el número de días en el mes de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_DAYS_IN_MONTH $date AS $days_in_month  # 31
    ```

<br>

**DATE_GET_DAYS_IN_YEAR**

- **Sintaxis:** `DATE_GET_DAYS_IN_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el número de días en el año de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_DAYS_IN_YEAR $date AS $days_in_year    # 365 or 366
    ```

<br>

**DATE_GET_TIMESTAMP**

- **Sintaxis:** `DATE_GET_TIMESTAMP {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el timestamp Unix de la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_TIMESTAMP $date AS $timestamp
    ```

<br>

### Contexto Anual

**DATE_GET_WEEK_OF_YEAR**

- **Sintaxis:** `DATE_GET_WEEK_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el número de la semana del año para la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_WEEK_OF_YEAR $date AS $week_of_year
    ```

<br>

**DATE_GET_DAY_OF_YEAR**

- **Sintaxis:** `DATE_GET_DAY_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el número del día del año para la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_DAY_OF_YEAR $date AS $day_of_year
    ```

<br>

**DATE_GET_HOUR_OF_YEAR**

- **Sintaxis:** `DATE_GET_HOUR_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene la hora del año para la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_HOUR_OF_YEAR $date AS $hour_of_year
    ```

<br>

**DATE_GET_MINUTE_OF_YEAR**

- **Sintaxis:** `DATE_GET_MINUTE_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el minuto del año para la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_MINUTE_OF_YEAR $date AS $minute_of_year
    ```

<br>

**DATE_GET_SECOND_OF_YEAR**

- **Sintaxis:** `DATE_GET_SECOND_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el segundo del año para la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_SECOND_OF_YEAR $date AS $second_of_year
    ```

<br>

**DATE_GET_MICROSECOND_OF_YEAR**

- **Sintaxis:** `DATE_GET_MICROSECOND_OF_YEAR {DATE: MAP} [AS {VAR}]`
- **Descripción:** Obtiene el microsegundo del año para la fecha especificada.
- **Ejemplo:**
    ```bash
    DATE_GET_MICROSECOND_OF_YEAR $date AS $microsecond_of_year
    ```

<br>

### Modificadores

**DATE_INCREMENT_YEAR**

- **Sintaxis:** `DATE_INCREMENT_YEAR {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa el año de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_YEAR $date WITH 5 AS $new_date
    ```

<br>

**DATE_INCREMENT_MONTH**

- **Sintaxis:** `DATE_INCREMENT_MONTH {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa el mes de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_MONTH $date WITH -3 AS $new_date
    ```

<br>

**DATE_INCREMENT_WEEK**

- **Sintaxis:** `DATE_INCREMENT_WEEK {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa la semana de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_WEEK $date WITH -3 AS $new_date
    ```

<br>

**DATE_INCREMENT_DAY**

- **Sintaxis:** `DATE_INCREMENT_DAY {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa el día de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_DAY $date WITH 10 AS $new_date
    ```

<br>

**DATE_INCREMENT_HOUR**

- **Sintaxis:** `DATE_INCREMENT_HOUR {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa la hora de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_HOUR $date WITH 5 AS $new_date
    ```

<br>

**DATE_INCREMENT_MINUTE**

- **Sintaxis:** `DATE_INCREMENT_MINUTE {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa el minuto de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_MINUTE $date WITH 30 AS $new_date
    ```

<br>

**DATE_INCREMENT_SECOND**

- **Sintaxis:** `DATE_INCREMENT_SECOND {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa el segundo de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_SECOND $date WITH 15 AS $new_date
    ```

<br>

**DATE_INCREMENT_MICROSECOND**

- **Sintaxis:** `DATE_INCREMENT_MICROSECOND {DATE: MAP} WITH {NUMBER} [AS {VAR}]`
- **Descripción:** Incrementa el microsegundo de la fecha especificada por el número dado. Retorna un nuevo mapa de fecha.
- **Ejemplo:**
    ```bash
    DATE_INCREMENT_MICROSECOND $date WITH 500 AS $new_date
    ```

<br>

### Configuración Global de Fechas


**DATE_SET_LOCALE**

- **Sintaxis:** `DATE_SET_LOCALE {LOCAL: STRING}`
- **Descripción:** Establece la configuración regional. Recibe un código de idioma como parámetro.
- **Ejemplo:**
    ```bash
    DATE_GET_WEEKDAY_NAME $result # Friday
    DATE_SET_LOCALE "es_ES"
    DATE_GET_WEEKDAY_NAME $result2 # viernes
    ```

<br>

**DATE_SET_TIMEZONE**

- **Sintaxis:** `DATE_SET_TIMEZONE {TIMEZONE: STRING}`
- **Descripción:** Establece la zona horaria.
- **Ejemplo:**
    ```bash
    DATE_SET_TIMEZONE "Europe/Berlin"
    DATE_SET_TIMEZONE "LOCAL"
    ```

<br>

**DATE_SET_FORMAT**

- **Sintaxis:** `DATE_SET_FORMAT {FORMAT: STRING} AS {format}`
- **Descripción:** Establece el formato global.
- **Ejemplo:**
    ```bash
    DATE_SET_FORMAT "YYYY-MM-DD"
    PRINT DATE_FORMAT_NOW;
    ```

<br>

**DATE_GET_LOCALE**

- **Sintaxis:** `DATE_GET_LOCALE [AS {VAR}]`
- **Descripción:** Devuelve el código de localización regional para el formato de fecha, como `en-us`.
- **Ejemplo:**
    ```bash
    DATE_GET_LOCALE AS $locale # en-us
    ```

<br>

**DATE_GET_TIMEZONE**

- **Sintaxis:** `DATE_GET_TIMEZONE [AS {VAR}]`
- **Descripción:** Devuelve la zona horaria establecida.
- **Ejemplo:**
    ```bash
    PRINT DATE_GET_TIMEZONE;
    ```

<br>

**DATE_GET_FORMAT**

- **Sintaxis:** `DATE_GET_FORMAT [AS {VAR}]`
- **Descripción:** Devuelve el formato global.
- **Ejemplo:**
    ```bash
    PRINT DATE_GET_FORMAT;
    ```

<br>

<br>

## Gestión de sistemas y procesos

**RUN**  

- **Sintaxis:** `RUN {STRING}`  
- **Descripción:** Ejecuta el programa o comando especificado sin esperar a que finalice. Esto permite que el script continúe ejecutándose mientras el proceso se ejecuta en segundo plano.  

- **Ejemplo:**  
    ```bash
    RUN "notepad.exe"
    ```
<br>

**RUNW**  

- **Sintaxis:** `RUNW {STRING}`  
- **Descripción:** Ejecuta el programa o comando especificado y espera a que termine antes de continuar con la ejecución del script. Esto garantiza que los comandos siguientes solo se ejecuten después de que el proceso haya finalizado.  

- **Ejemplo:**  
    ```bash
    RUNW "installer.exe"
    ```

<br>

**RUN_FILE**  

- **Sintaxis:** `RUN_FILE {STRING}`  
- **Descripción:** Abre o ejecuta un archivo utilizando la aplicación predeterminada asociada en el sistema operativo. Si el archivo no existe, se genera un error.  

- **Ejemplo:**  
    ```bash
    RUN_FILE "document.pdf"
    ```

<br>

**TERMINATE**  

- **Sintaxis:** `TERMINATE {STRING | NUMBER}`  
- **Descripción:** Termina un proceso en ejecución por su ID de proceso (PID) o por su nombre de ejecutable. Si el proceso especificado existe, se detiene forzosamente. Retorna `TRUE` si el proceso fue terminado con éxito, de lo contrario, retorna `FALSE`.  

- **Ejemplo:**  
    ```bash
    TERMINATE 12345  # Por PID
    TERMINATE "notepad.exe"
    ```

<br>

**WAIT**  

- **Sintaxis:**  
    1. `WAIT {MILLISECONDS: NUMBER}`  
    2. `WAIT FOR {SECONDS: NUMBER}`  

- **Descripción:** Pausa la ejecución del script durante la duración especificada. La demora puede indicarse en milisegundos o segundos.  

- **Ejemplo:**  
    ```bash
    WAIT 500  # Espera 500 milisegundos (0.5 segundos)
    PRINT "Hola"
    
    WAIT FOR 3  # Espera 3 segundos
    PRINT "Mundo"
    ```

<br>

## Solicitudes HTTP

**HTTP_SET_PARAMS**

- **Sintaxis:** `HTTP_SET_PARAMS {MAP}`
- **Descripción:** Establece los parámetros de la solicitud HTTP.
- **Ejemplo:**
    ```bash
    HTTP_SET_PARAMS {"key": "value"}
    ```

<br>

**HTTP_SET_HEADER**

- **Sintaxis:** `HTTP_SET_HEADER {MAP}`
- **Descripción:** Establece los encabezados de la solicitud HTTP.
- **Ejemplo:**
    ```bash
    HTTP_SET_HEADER {"Content-Type": "application/json"}
    ```

<br>

**HTTP_SET_METHOD**

- **Sintaxis:** `HTTP_SET_METHOD {STRING}`
- **Descripción:** Define el método HTTP a utilizar (ejemplo: `POST`, `GET`, etc.).
- **Ejemplo:**
    ```bash
    HTTP_SET_METHOD "POST"
    ```

<br>

**HTTP_SET_DATA**

- **Sintaxis:** `HTTP_SET_DATA {MAP}`
- **Descripción:** Establece los datos que se enviarán en la solicitud HTTP.
- **Ejemplo:**
    ```bash
    HTTP_SET_DATA {"name": "John", "age": 30}
    ```

<br>

**HTTP_SET_TIMEOUT**

- **Sintaxis:** `HTTP_SET_TIMEOUT {TIME: NUMBER}`
- **Descripción:** Establece el tiempo de espera (en segundos) para la solicitud HTTP. Por defecto, 30 segundos.
- **Ejemplo:**
    ```bash
    HTTP_SET_TIMEOUT 10
    ```

<br>

**HTTP_RESET**

- **Sintaxis:** `HTTP_RESET`
- **Descripción:** Reinicia todos los parámetros configurados para la solicitud HTTP.
- **Ejemplo:**
    ```bash
    HTTP_RESET
    ```

<br>

**HTTP_REQUEST**

- **Sintaxis:** `HTTP_REQUEST {URL: STRING} [AS {VAR}]`
- **Descripción:** Realiza una solicitud HTTP a la URL especificada y almacena la respuesta en una variable.
- **Ejemplo:**
    ```bash
    HTTP_REQUEST "https://example.com/api" AS $response
    ```

<br>

**HTTP_REQUEST_EXT**

- **Sintaxis:** `HTTP_REQUEST_EXT {URL: STRING} [AS {VAR}]`
- **Descripción:** Realiza una solicitud HTTP a la URL especificada y almacena información extendida de la respuesta (redirecciones, codificación, historial, texto, etc.) en una variable.
- **Ejemplo:**
    ```bash
    HTTP_REQUEST_EXT "https://example.com/api" AS $extended_response
    ```

<br>

**HTTP_GET**

- **Sintaxis:** `HTTP_GET {URL: STRING} [AS {VAR}]`
- **Descripción:** Realiza una solicitud GET a la URL especificada y almacena la respuesta en una variable.
- **Ejemplo:**
    ```bash
    HTTP_GET "https://example.com/api" AS $get_response
    ```

<br>

**HTTP_POST**

- **Sintaxis:** `HTTP_POST {URL: STRING} [AS {VAR}]`
- **Descripción:** Realiza una solicitud POST a la URL especificada y almacena la respuesta en una variable.
- **Ejemplo:**
    ```bash
    HTTP_POST "https://example.com/api" AS $post_response
    ```

<br>

**HTTP_DOWNLOAD**

- **Sintaxis:** `HTTP_DOWNLOAD {URL: STRING} TO {FILE: STRING}`
- **Descripción:** Descarga un archivo desde la URL especificada y lo guarda en la ubicación indicada.
- **Ejemplo:**
    ```bash
    HTTP_DOWNLOAD "https://example.com/file.zip" TO "downloads/file.zip"
    ```

<br>

**HTTP_UPLOAD**

- **Sintaxis:** `HTTP_UPLOAD {FILE: STRING} TO {URL: STRING} [AS {VAR}]`
- **Descripción:** Sube un archivo a la URL especificada y almacena la respuesta en una variable.
- **Ejemplo:**
    ```bash
    HTTP_UPLOAD "path/to/file.zip" TO "https://example.com/upload" AS $upload_response
    ```

<br>

**HTTP_GET_SETTINGS**

- **Sintaxis:** `HTTP_GET_SETTINGS [AS {VAR}]`
- **Descripción:** Devuelve los parámetros, encabezados y otras configuraciones actuales de la solicitud HTTP.
- **Ejemplo:**
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

## Traducción

Scriptal proporciona funciones integradas para traducir textos entre diversos idiomas utilizando la API de Google Translator. Sin embargo, es importante tener en cuenta que la disponibilidad y precisión del servicio de traducción están sujetas a las condiciones y posibles interrupciones de Google. Esto podría resultar en fallas, limitaciones en el uso o bloqueos ocasionales.

Puedes usar tanto el código del idioma (`en`) como el nombre de este (`english`). Puedes obtener todos los idiomas disponibles para traducir, usando el comando `TRANSLATE_SUPPORTED_CODES`.

<br>

**TRANSLATE FROM**

- **Sintaxis:** `TRANSLATE FROM {SOURCE_LANG: STRING} TO {TARGET_LANG: STRING}`
- **Descripción:** Configura el idioma de origen y el idioma de destino de forma global.
- **Ejemplo:**
    ```bash
    TRANSLATE FROM "en" TO "it"
    ```

<br>

**TRANSLATE**

- **Sintaxis:** `TRANSLATE {STRING} [AS {VAR}]`
- **Descripción:** Traduce el texto especificado al idioma de destino predeterminado. Si la traducción falla, lanzará un error.
- **Ejemplo:**
    ```bash
    TRANSLATE "Hello, world!" AS $translated_text
    ```

<br>

**TRANSLATES**

- **Sintaxis:** `TRANSLATES {STRING} [AS {VAR}]`
- **Descripción:** Traduce el texto especificado al idioma de destino predeterminado. Si la traducción falla, el resultado será el mismo texto de entrada.
- **Ejemplo:**
    ```bash
    TRANSLATES "How are you?" AS $translated_text   # Si falla la traducción, el resultado será "How are you?"
    ```

<br>

**TRANSLATE_ATTEMPTS**

- **Sintaxis:** `TRANSLATE_ATTEMPTS {NUMBER}`
- **Descripción:** Establece el número de intentos para realizar la traducción en caso de fallos.
- **Ejemplo:**
    ```bash
    TRANSLATE_ATTEMPTS 3
    TRANSLATE ""    # Un string vacío normalmente produce errores.
    ```

<br>

**TRANSLATE_FROM**

- **Sintaxis:** `TRANSLATE_FROM {SOURCE_LANG: STRING}`
- **Descripción:** Establece el idioma de origen para futuras traducciones.
- **Ejemplo:**
    ```bash
    TRANSLATE_FROM "en"
    ```

<br>

**TRANSLATE_TO**

- **Sintaxis:** `TRANSLATE_TO {TARGET_LANG: STRING}`
- **Descripción:** Establece el idioma de destino futuras traducciones.
- **Ejemplo:**
    ```bash
    TRANSLATE_TO "es"
    ```

<br>

**TRANSLATE_SUPPORTED_CODES**

- **Sintaxis:** `TRANSLATE_SUPPORTED_CODES [AS {VAR}]`
- **Descripción:** Devuelve los códigos de idiomas soportados para traducción.
- **Ejemplo:**
    ```bash
    TRANSLATE_SUPPORTED_CODES AS $supported_codes
    ```

<br>

**TRANSLATE_SUPPORTED_LANGS**

- **Sintaxis:** `TRANSLATE_SUPPORTED_LANGS [AS {VAR}]`
- **Descripción:** Devuelve los idiomas soportados para traducción.
- **Ejemplo:**
    ```bash
    TRANSLATE_SUPPORTED_LANGS AS $supported_langs
    ```
