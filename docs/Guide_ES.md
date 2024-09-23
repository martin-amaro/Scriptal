# Scriptal: Lenguaje de Procesamiento de Texto

### Tabla de Contenido

1. [ Introducción. ](#introducción)
2. [ Sintaxis. ](#sintaxis)
3. [ Comandos. ](#comandos)
   1. [ Parámetros. ](#parámetros)
   2. [ Múltiples parámetros. ](#múltiples-parámetros)
   3. [ Conectores. ](#conectores)

4. [ Comandos Internos. ](#comandos-internos)
5. [ Comandos Compuestos. ](#comandos-compuestos)
6. [ Comentarios. ](#comentarios)
7. [ Variables. ](#variables)
8. [ Tipo de Datos. ](#tipo-de-datos)
   1. [ Obtener Tipo de Datos. ](#obtener-tipo-de-datos)
   2. [ Strings. ](#strings)
   3. [ Números. ](#números)
   4. [ Arrays. ](#arrays)
   5. [ Booleanos. ](#booleanos)
   6. [ Patrones. ](#patrones)
9. [ Conector AS. ](#conector-as)
10. [ Operadores. ](#operadores)
    1. [ Aritméticos. ](#operadores-aritméticos)
    2. [ De Comparación. ](#operadores-de-comparación)
    3. [ Lógicos. ](#operadores-lógicos)
    4. [ De Pertenencia. ](#operadores-de-pertenencia)
    5. [ Combinación de Datos. ](#combinación-de-datos)
    6. [ Operadores Especiales vs Conectores. ](#operadores-especiales-vs-conectores)
11. [ Condiciones. ](#condiciones)
12. [ Palabras Claves. ](#palabras-claves)
13. [ Comparación de Datos. ](#comparación-de-datos)
14. [ Procesamiento de Texto. ](#procesamiento-de-texto)
15. [ Eventos. ](#eventos)
16. [ Variables de entorno. ](#variables-de-entorno)
17. [ Bucles. ](#bucles)
18. [ Definir Comandos. ](#definir-comandos)
19. [ Manejo de Errores. ](#manejo-de-errores)
20. [ Módulos. ](#módulos)
21. [ Librerías. ](#librerías)
22. [ Convenciones. ](#convenciones)
23. [ Recursos Adicionales. ](#recursos-adicionales)


<br>

## Introducción

Scriptal es un lenguaje liviano diseñado específicamente para el procesamiento de texto, permitiendo al usuario modificar la estructura de un texto según su conveniencia con el fin de agilizar procesos.

Fue creado para facilitar el aprendizaje de lenguajes de programación, basándose en la estructura del inglés y utilizando palabras más comunes, como `ROUND_UP` en lugar de `CEIL`.

### Sintaxis de Scriptal comparada con otros lenguajes de programación

Scriptal fue diseñado para facilitar la lectura y tiene algunas similitudes con el idioma inglés. A continuación, se destacan algunas de las diferencias y similitudes entre Scriptal y otros lenguajes de programación:

1. **Nuevas Líneas en Lugar de Punto y Coma**

    Scriptal utiliza nuevas líneas para completar un comando, a diferencia de otros lenguajes de programación que suelen usar punto y coma o paréntesis. Esta característica hace que el código sea más limpio y legible.

2. **Sangría para Definir Alcance**

    Scriptal se basa en la sangría, utilizando espacios en blanco, para definir el alcance de bloques de código como bucles, funciones y condiciones. Otros lenguajes de programación a menudo usan corchetes rizados {} para este propósito.

<br>

## Sintaxis

El uso de Scriptal es muy sencillo una vez que entiendes sus reglas y cómo funciona su sintaxis. A continuación, se presentan algunas de las reglas más importantes:

### Secuencias de comandos

Un archivo Scriptal es una secuencia de comandos o instrucciones. No es posible realizar acciones como operaciones matemáticas o asignación de variables de forma libre; siempre debes usar un comando específico para estas tareas. Por lo tanto, cada línea debe comenzar con un comando.


### Una Línea: Un Comando

Como convención, se recomienda utilizar un comando por línea. Si necesitas usar varios comandos, sepáralos en distintas líneas. Esto mantiene el código más claro y fácil de entender. Aunque en algunos casos tendrás que usar comandos internos, intenta siempre usar un comando por línea cuando sea posible.


<br>

## Comandos

Un "comando" es una instrucción que se escribe al principio de la línea y realiza una acción específica en el contexto del procesamiento de texto. Cada comando está diseñado para ser claro y directo, utilizando una sintaxis en mayúsculas para indicar la operación que se va a ejecutar. Los comandos pueden modificar, analizar, transformar y manipular texto, así como realizar operaciones lógicas y de control de flujo.


### Parámetros

En Scriptal, los comandos pueden recibir información adicional conocida como parámetros. A diferencia de otros lenguajes, no se utilizan paréntesis para pasar parámetros; se escriben directamente después del comando.

```bash
# ¡Pseudocódigo!
COMANDO PARÁMETRO
```
<br>

Por ejemplo, el comando `PRINT` muestra un mensaje en la consola. Éste necesita un parámetro para funcionar:

```bash
PRINT "He escrito algo para ti"
```

<br>

En Scriptal no es posible utilizar múltiples comandos de forma independiente en una misma línea. Por ejemplo, la siguiente sintaxis es incorrecta:

```bash
PRINT "Hello" PRINT "World"     # ERROR! En realidad, estás pasando 3 parámetros.
```

<br>

En lugar de esto, debes escribir cada comando en una línea separada:

```bash
PRINT "Hello"
PRINT "World"
```

<br>

### Múltiples parámetros

A diferencia de otros lenguajes de programación, en Scriptal no se utilizan comas para separar los parámetros. En su lugar, los parámetros se separan por tipos de datos o instancias. Cada cadena de texto, cifra, operación matemática, array, etc., se considera una instancia separada.

```bash
# ¡Pseudocódigo!
COMANDO "Parámetro #1" "Parámetro #2" "Parámetro #3"
```
<br>

Los espacios entre parámetros no siempre indican separación. Por ejemplo, en las operaciones matemáticas, los espacios pueden estar presentes y el conjunto aún se considerará un solo parámetro:

```bash
# ¡Pseudocódigo!
COMANDO "Parámetro #1" 32.4564 + 10.2103 "Parámetro #3"
```

**Parámetros obtenidos:**

* "Parámetro #1"
* 42.6667
* "Parámetro #3"

<br>

> [!NOTE]  
> Para evitar confusiones, se utilizan conectores.

<br>

### Conectores

Para evitar confusiones y mejorar la legibilidad del código, los comandos en Scriptal utilizan palabras clave llamadas **conectores**. Estas palabras clave, siempre en mayúsculas, separan los parámetros entre sí y también se consideran parámetros. Algunos conectores son obligatorios en ciertos comandos.

<br>

**Lista de conectores**:

`AND`, `AS`, `AT`, `BETWEEN`, `BY`, `FROM`, `IN`, `IS`, `INTO`, `OR`, `TO`, `WITH`.

<br>

Por ejemplo, para dividir un texto, podemos usar el comando `SPLIT`, que requiere tres parámetros: el texto, el conector `BY`, y el separador.

```bash
SPLIT "Hola Mundo desde Scriptal" BY " " # Resultado: ["Hola", "Mundo", "desde", "Scriptal"]
```

En este caso, `BY` es un conector obligatorio que también cuenta como un parámetro. El conector asegura que el código sea más claro y fácil de leer, especificando claramente la relación entre los distintos parámetros del comando.

<br>

## Comandos Internos

En algunas situaciones, es necesario usar un comando dentro de otro. Estos comandos se conocen como comandos internos, ya que proporcionan información al comando principal. Para que un comando interno sea válido, debe retornar un valor que el comando externo pueda utilizar. Dicho valor no puede ser `NULL`. 

Para utilizar un comando interno, debes llamarlo desde otro comando y cerrar su apertura con un punto y coma (;).

<br>

**Ejemplo de Comando Interno**

Si deseas convertir un texto a mayúsculas y luego imprimirlo, puedes usar `UPPERCASE` como un comando interno dentro de `PRINT`:

```bash
PRINT UPPERCASE "texto en minúsculas";
```

<br>

En este caso, `UPPERCASE` es un comando interno que retorna un valor que `PRINT` usa para mostrar el texto en mayúsculas.

<br>

**IMPORTANTE**

Recomendamos no usar más de un comando interno por línea. Esto ayuda a mantener el código claro y evita confusiones en la ejecución de los comandos.

Sin embargo, si necesitas usar más de un comando interno en una línea, debes encapsular los comandos internos dentro de paréntesis para que Scriptal los interprete correctamente.

```bash
PRINT MATCH (PATTERN "\d+";) IN $text;
PRINT (UPPERCASE (REVERSE "Hola Mundo";);)
```

Nuevamente, **EVITA** códigos como ese.

<br>

> [!NOTE]  
> Como excepción, sólo el comando `RETURN` puede utilizar comandos internos que devuelvan `NULL`.


<br>

## Comandos Compuestos

En Scriptal, los comandos compuestos permiten anidar código dentro de ellos para manejar estructuras de control como condiciones, bucles y definiciones. Estos comandos determinan cuándo y cómo se ejecutará el código anidado.

### Bloques de código e Indentación

La indentación es crucial en Scriptal para definir bloques de código. A diferencia de otros lenguajes donde la indentación se usa principalmente para mejorar la legibilidad, en Scriptal se utiliza para estructurar el código y definir la jerarquía de los bloques.

**Ejemplo de Pseudocódigo**
```bash
PADRE
    HIJO
        NIETO
```

La indentación debe ser un múltiplo de 4 espacios.

<br>

Si un bloque de código dentro de un comando compuesto está vacío, se producirá un error de indentación. Para evitar este problema, utiliza el comando `SKIP`, que funciona de manera similar a `pass` en Python.

```bash
WHEN TRUE
    SKIP
```

<br>

## Comentarios

los comentarios se utilizan para explicar el código, hacer que sea más legible, o simplemente para anotar secciones del código. Los comentarios comienzan con un `#` y son ignorados por Scriptal durante la ejecución.

```bash
# Esto es un comentario
PRINT "Hello World"
```

<br>

Los comentarios también se pueden colocar al final de una línea y Scriptal ignorará el resto de la línea:

```bash
PRINT "Hello World"     # Esto es un comentario
```

<br>


### Comentarios de varias líneas

Para comentarios de varias líneas, enciérralos entre ``###``:

```coffeescript
###
    Yes!
    Soy un comentario
    de varias lineas
###
```

<br>


## Variables

En Scriptal, las variables son contenedores para almacenar valores de datos. Cada variable debe ser precedida por un signo `$`, similar a cómo se hace en PHP. Por ejemplo: `$nombre`.

Una variable puede tener un nombre corto (como x o y) o un nombre más descriptivo (como edad, nombre_del_auto, volumen_total). Las reglas para los nombres de variables en Scriptal son:

- Un nombre de variable debe empezar con una letra o el carácter guion bajo (_).
- Un nombre de variable no puede empezar con un número.
- Un nombre de variable solo puede contener caracteres alfanuméricos y guiones bajos (A-Z, 0-9 y _).
- Los nombres de las variables distinguen entre mayúsculas y minúsculas (edad, Edad y EDAD son tres variables diferentes).

<br>

### Declarar o Modificar Variables

Para declarar o modificar una variable, se utiliza el comando `SET`. A diferencia de otros lenguajes de programación, en Scriptal solo puedes realizar estas tareas mediante comandos.

**Sintaxis**
```
SET {variable} AS {value}
```

**Ejemplos**

```bash
SET $name AS "Martin"
SET $age AS 22

PRINT $age      # 22
PRINT $name     # Martin
```

<br>

No es necesario declarar variables con ningún tipo en particular, e incluso pueden cambiar de tipo después de que se hayan sido establecidas.

```bash
SET $x AS 4
SET $x AS "Mike"
```

<br>

### Alcance (Scope) de las Variables

En Scriptal, el alcance (scope) de una variable determina dónde puede ser accedida dentro del código. Existen dos tipos principales de alcance:

- **Alcance Local**: Cuando una variable se declara dentro de un bloque de código o función, su alcance está limitado a ese bloque o función. Estas variables no están disponibles fuera de su contexto específico.

    ```bash
    WHEN 34
        SET $name AS "John" # Declaramos una variable en un bloque

    PRINT $name # Variable no definida
    ```
<br>

- **Alcance Global**: Si una variable se declara fuera de cualquier bloque de código o función, dentro de un evento, o si no hay un alcance específico establecido, la variable tiene un alcance global y puede ser accedida desde cualquier parte del código. Para declarar una variable global desde dentro de un bloque o función, se utiliza el comando `GLOBAL_SET`.

    ```bash
    WHEN 34
        GLOBAL_SET $name AS "John" # Declaramos una variable global
    
    PRINT $name # John
    ```


<br>


## Tipo de Datos

Las variables pueden almacenar diferentes tipos de datos. Cada tipo de dato tiene características y comportamientos distintos.

Scriptal incluye los siguientes tipos de datos integrados:

- **`STRING`**: Para almacenar texto.
- **`NUMBER`**: Para almacenar valores numéricos.
- **`ARRAY`**: Para almacenar secuencias de elementos.
- **`MAP`**: Para almacenar pares clave-valor.
- **`BOOL`**: Para valores booleanos (verdadero o falso).
- **`NULL`**: Para representar la ausencia de valor.
- **`PATTERN`**: Para patrones de texto o expresiones regulares.

<br>

### Obtener Tipo de Datos

Puede obtener el tipo de datos de cualquier objeto mediante el comando `TYPE`.

```bash
SET $x AS 32
PRINT TYPE $x;      	# Devolverá: "NUMBER"
PRINT TYPE "Hello";     # Devolverá: "STRING"
```

<br>

### Strings

Las cadenas de texto se encierran entre comillas simples (') o dobles ("). Ambos tipos de comillas se consideran equivalentes y puedes usar el que prefieras.

`"hola"` es lo mismo que `'hola'`

```bash
PRINT "I'm a string"
PRINT 'También se usan comillas simples'
```

<br>

#### Caracteres Especiales
Dentro de las cadenas, puedes incluir caracteres especiales como `\n` para nuevas líneas o `\t` para tabulaciones.

**Ejemplo**

```bash
PRINT "Line #1\nLine #2 \u0052 \tTEXT"
```

<br>

#### Cadenas Crudas
Para usar una cadena en su forma cruda, es decir, sin interpretar caracteres especiales, usa el signo `!` antes del string. Esto es útil cuando deseas evitar que se procesen secuencias de escape y otros caracteres especiales.

**Ejemplo**

```bash
PRINT !"Line #1\nLine #2 \u0052 \tTEXT"
```

En este ejemplo, la cadena se imprimirá exactamente como está escrita, incluyendo los caracteres especiales en su forma literal.

<br>

#### Conversión a String

Puedes convertir otros tipos de datos a strings utilizando el comando `STRING`:

```bash
PRINT STRING 256;   # "256"
PRINT STRING TRUE;   # "TRUE"
```

<br>


### Números

Los números pueden ser enteros o flotantes, y no hay distinción especial entre ellos en cuanto a su tamaño o formato. Ambos tipos de números se manejan de la misma manera.

**Ejemplo**

```bash
PRINT 18272616
PRINT 1289.232
```

<br>

#### Notación Científica

Puedes utilizar la notación científica para representar números muy grandes o muy pequeños con `e` o `E`.

**Ejemplo**

```bash
PRINT 1.23e4		# Es lo mismo que: 1.23 * 10^4
PRINT 4.56E-3		# Es lo mismo que: 4.56 * 10^-3
```

<br>

#### Infinito y NAN

Scriptal reconoce los valores de `INFINITY` y `NAN` (Not a Number). Puedes usarlos directamente en tus cálculos.

**Ejemplo**

```bash
PRINT 1e308 * 10               # Infinito positivo
PRINT -1e308 * 10              # Infinito negativo
PRINT INFINITY - INFINITY      # NAN
PRINT -(INFINITY)              # Infinito negativo
```

<br>

#### Conversión a Números

Puedes convertir otros tipos de datos a números utilizando los comandos `NUMBER` y `FLOAT`. Estas conversiones son útiles cuando necesitas realizar operaciones matemáticas o procesar datos numéricos.

<br>

**Conversión a Entero**

El comando `NUMBER` convierte un valor a entero. Si el valor no puede ser convertido, se lanzará un error.

```bash
PRINT NUMBER "56";  # 56
PRINT NUMBER 123;  # 123
PRINT NUMBER 10.5;  # 10
```

<br>

**Conversión a Flotante**

El comando `FLOAT` convierte un valor a flotante. Si el valor no puede ser convertido, se lanzará un error.

```bash
PRINT FLOAT "56.25";  # 56.25
PRINT NUMBER 10.5;  # 10.5
PRINT NUMBER "123";  # 123.0
```

<br>

### Arrays

Los arrays o arreglos se utilizan para almacenar múltiples elementos en una sola variable.
Puedes crear y manipular arreglos fácilmente utilizando corchetes para definirlos.

#### Crear una arreglo

Los arreglos se crean utilizando corchetes (`[` y `]`).

**Ejemplo**

```bash
SET $lista AS ["Manzana", "Pera", "Banana"]
```

<br>

#### Elementos del arreglo

- **Ordenados**: Los elementos del arreglo tienen un orden específico.
- **Modificables**: Puedes cambiar los elementos del arreglo después de crearlo.
- **Duplicados Permitidos**: Los arreglos pueden contener valores duplicados.

<br>

#### Acceder a los elementos

Los elementos del arreglo están indexados, comenzando desde 1. Puedes acceder a los elementos utilizando un punto (`.`) seguido del índice después del nombre de la variable.

```bash
SET $fruits AS ["Uva", "Manzana", "Fresa"]
PRINT $fruits.1 # Uva
```
<br>

También puedes usar el comando `GET` para obtener un índice dinámicamente:
```bash
SET $fruits AS ["Uva", "Manzana", "Fresa"]
PRINT GET 2 IN $fruits; # Manzana
```

<br>

> [!IMPORTANT]  
> En Scriptal, todos los índices comienzan desde 1, no desde 0. Esto es importante tenerlo en cuenta al acceder a los elementos del arreglo.

<br>

#### Modificar elementos del Arreglo

Puedes modificar un elemento específico de un arreglo utilizando su índice.


**Ejemplo**

```bash
SET $numeros AS [10, 20, 30]
SET $numeros.2 AS 25
PRINT $numeros.2  # 25
```

<br>

También puedes modificar un elemento utilizando el comando `PUT`:
```bash
SET $numeros AS [10, 20, 30]
PUT "Hola!" AT 3 IN $numeros  # [10, 20, "Hola!"]
```

<br>

#### Agregar y Eliminar Elementos

Scriptal también permite agregar y eliminar elementos de un array utilizando comandos específicos.

**Agregar un Elemento:**

Puedes usar comandos como `APPEND`, `PUT` o `INSERT` para este fin:

```bash
APPEND "Naranja" TO $fruits
PUT "Naranja" IN $fruits
```

<br>

**Eliminar un Elemento:**

```bash
DELETE "Naranja" IN $fruits   # Remove element by value
REMOVE 1 IN $fruits           # Remove element by index
```

<br>

#### Estructura ARRAY

Existe otra forma de crear arrays, utilizando el comando `ARRAY_SET` y el comando `ELEM`:

```bash
ARRAY_SET $fruits
    ELEM "Uva"
    ELEM "Manzana"
    ELEM "Fresa"

PRINT $fruits
```

<br>

Revisa la lista completa de comandos para Arrays [aquí](Commands_ES.md#arrays).

<br>


### Mapas

Los mapas se utilizan para almacenar pares clave - valor, permitiendo una estructura de datos ordenada y modificable sin duplicados. Los mapas son una herramienta fundamental para organizar y gestionar información de manera eficiente.

<br>

#### Crear un Mapa

Puedes inicializar un mapa con valores predeterminados utilizando la estructura `{}`:

```bash
SET $mi_mapa AS {"nombre": "Martin", "edad": 22}
```

<br>

#### Acceder a los elementos

Para acceder a los valores de un mapa, se utiliza la variable del mapa seguida del nombre de la llave, separados por un punto:

```bash
PRINT $mi_mapa.nombre     # Martin
```
<br>

Si necesitas acceder al valor de una llave usando un nombre dinámico, puedes usar el comando `MAP_KEY_GET` o `GET`:

```bash
PRINT MAP_KEY_GET "nombre" IN $mi_mapa;   # Martin
PRINT GET "nombre" IN $mi_mapa;   # Martin
```

La diferencia entre `GET` y `MAP_KEY_GET` radica en que, si la clave no existe, `GET` retornará `NULL` en lugar de lanzar error.

<br>


#### Estructura MAP
También puedes crear o manipular un mapa utilizando el comando `MAP_SET`:

```bash
MAP_SET $mi_mapa
    SKIP
```

<br>

Las llaves de un mapa se pueden definir o modificar utilizando el comando `KEY` (el cual es muy similar a `SET`) dentro del bloque:

```bash
MAP_SET $mi_mapa
    KEY "nombre" AS "Martin"
    KEY "edad" AS 22
```


<br>

#### Eliminar Llaves y Valores

Para eliminar una llave y su valor asociado de un mapa, se utiliza el comando `DELETE`:

```bash
DELETE "edad" IN $mi_mapa
```

<br>

Revisa la lista completa de comandos para mapas [aquí](Commands_ES.md#mapas).


<br>

### Booleanos

Los valores booleanos son fundamentales para la lógica de control y las evaluaciones condicionales. Los valores booleanos solo pueden ser `TRUE` o `FALSE`.

#### Crear un Booleano

Puedes asignar valores booleanos a las variables utilizando el comando `SET`:

```bash
SET $es_verdadero AS TRUE
SET $es_falso AS FALSE
```

<br>

#### Conversión a Booleanos

Puedes convertir otros tipos de datos a booleanos utilizando el comando `BOOL`. Esta conversión es útil para evaluar condiciones basadas en diferentes tipos de datos. El comando BOOL interpreta los valores de la siguiente manera:

- Cualquier valor que no sea `FALSE`, `0`, una cadena vacía (`""`), o un array/mapa vacío se considera `TRUE`.
- Los valores `FALSE`, `0`, una cadena vacía (`""`), y arrays/mapas vacíos se consideran `FALSE`.

<br>

```bash
SET $numero AS 10
BOOL $numero AS $es_verdadero
PRINT $es_verdadero    # TRUE

SET $cero AS 0
BOOL $cero AS $es_falso
PRINT $es_falso    # FALSE
```

<br>

### Patrones

Los patrones (PATTERN) permiten trabajar con expresiones regulares y realizar operaciones avanzadas de procesamiento de texto. A continuación, se explican los aspectos clave sobre los patrones, incluyendo su definición, uso de flags, y comandos relacionados.

#### Definición de Patrones

Para crear un patrón en Scriptal, se utiliza el comando `PATTERN` seguido de una expresión regular como un string. Los patrones permiten realizar búsquedas y manipulaciones de texto basadas en expresiones regulares.

**Sintaxis:**

```bash
PATTERN "expresión regular"
```

<br>

**Ejemplo:**

```bash
PATTERN "^[A-Za-z]+$" # Un patrón que busca cadenas de texto que contengan solo letras
```

<br>

#### Uso de Flags

Las flags o banderas permiten modificar el comportamiento de las expresiones regulares. Se especifican utilizando el comando `PATTERN` junto a un array de flags.

**Sintaxis:**

```bash
PATTERN "expresión regular" WITH [array_de_flags]
```

<br>

**Flags disponibles:**

- `i` o `IGNORECASE` - Ignora la distinción entre mayúsculas y minúsculas.
- `m` o `MULTILINE` - `^` y `$` coinciden con el inicio y fin de cada línea.
- `s` o `DOTALL` - `.` coincide con todos los caracteres, incluidos los saltos de línea.
- `a` o `ASCII` - Coincide solo con caracteres ASCII.
- `l` o `LOCALE` - Basado en la configuración regional del sistema (menos común).
- `u` o `UNICODE` - Permite que los metacaracteres coincidan con caracteres Unicode, en lugar de solo caracteres ASCII. Esta es la configuración predeterminada, y no es necesario especificarla.

<br>

**Ejemplo:**

```bash
PATTERN "michael" WITH ["i"]    # Coincide con michael, Michael, MICHAEL, etc.
PATTERN "^\d{3}-\d{2}-\d{4}$" WITH ["MULTILINE"]
```

<br>

#### Uso de Patrones en Comandos

Los patrones se pueden usar en varias funciones para realizar diferentes operaciones sobre el texto:

- **MATCH**: Busca una coincidencia en el texto.
- **MATCH_FIRST**: Encuentra la primera coincidencia en el texto.
- **MATCH_ALL**: Encuentra todas las coincidencias en el texto.
- **MATCH_FULL**: Busca una coincidencia completa en el texto.

<br>

**Ejemplo:**

```bash
PATTERN "^Hello" AS $pattern # Define un patrón para buscar cadenas que comienzan con "Hello"
MATCH $pattern IN "Hello world" AS $result # Busca una coincidencia en el texto
PRINT $result
```

<br>

#### Uso de Raw Strings

Para evitar que ciertos caracteres especiales como `\n` sean interpretados en los patrones, se pueden utilizar raw strings:

```bash
PATTERN !"\d{2}\s\w+" # Un patrón para buscar dos dígitos seguidos de un espacio y una palabra, sin interpretar caracteres especiales
```

<br>


#### Comandos Relacionados con Patrones

Varios comandos aceptan tanto cadenas de texto como patrones para realizar diferentes operaciones. Estos comandos son útiles para buscar, reemplazar y manipular texto basado en expresiones regulares. Entre ellos están: `COUNT`, `SPLIT`, `FIND`, `REPLACE`, etc.

<br>

Para más detalle, puedes revisar la lista completa de comandos relacionados con patrones [aquí](Commands_ES.md#patrones-y-expresiones-regulares).

<br>

## Conector `AS`

El conector `AS` es uno de lo más usado e importantes. Se utiliza por lo general para definir variables, por ejemplo:

```bash
SET $edad AS 32
```

<br>

Además, se puede utilizar de manera opcional en la mayoría de comandos para almacenar el resultado en una variable:

```bash
UPPERCASE "Hola Mundo" AS $line
PRINT $line
```

De esa forma, evitarías el uso de [ comandos internos ](#comandos-internos).

<br>

**IMPORTANTE**

Ten en cuenta que, al utilizar este conector para definir o modificar una variable, el comando no devolverá nada:

```bash
UPPERCASE "Hola"                   # Devuelve "HOLA"
UPPERCASE "Hola" AS $text          # No devuelve nada

# Por lo tanto...
PRINT UPPERCASE "Hola";            # Imprime "HOLA"
PRINT UPPERCASE "Hola" AS $text;   # Error: Los comandos internos deben devolver algo.
```

<br>


### Abreviación

Si prefieres, puedes utilizar dos puntos (`:`) como abreviatura de `AS`:

```bash
# Puedes usar tanto...
SET $name AS "Martin" 
UPPERCASE $name AS $upper

# Como...
SET $name : "Martin" 
UPPERCASE $name : $upper
```
<br>

Sin embargo, recomendamos utilizar la abreviación sólo cuando el conector `AS` sea obligatorio. De lo contrario, recomendamos dejarlo en su forma original.

```bash
# El conector es obligatorio en comandos como "SET", "KEY", etc. Acá lo podemos abreviar.
SET $name : "Martin"
MAP_SET $map
    KEY "name" : "Martin"
    

# En comandos como "UPPERCASE" y "SPLIT" el conector es opcional, por lo tanto, lo dejamos en su forma original
UPPERCASE $name AS $upper
SPLIT "hello world" BY " " AS $words
```
<br>

## Operadores

Scriptal soporta varios tipos de operadores que permiten realizar operaciones aritméticas, comparaciones y operaciones lógicas. Estos operadores son esenciales para el manejo y la manipulación de datos en tu código.

### Operadores Aritméticos

Estos operadores se utilizan para realizar operaciones matemáticas básicas:

- **Suma (`+`):** Suma dos valores.
- **Resta (`-`):** Resta un valor de otro.
- **Multiplicación (`*`)**: Multiplica dos valores.
- **División (`/`):** Divide un valor entre otro.
- **Módulo (`%`):** Devuelve el resto de la división de dos valores.
- **Exponenciación (`**` o `^`):** Eleva un número a la potencia de otro.

<br>

**Ejemplos**

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

### Operadores de Comparación

Estos operadores se utilizan para comparar dos valores y devuelven un valor booleano (`TRUE` o `FALSE`):


- **`IS` / `==`** - Devuelve `TRUE` si ambos valores son iguales.
- **`IS NOT` / `!=`** - Devuelve `TRUE` si los valores no son iguales.
- **`IS GT` / `>`** - Devuelve `TRUE` si el primer valor es mayor que el segundo.
- **`IS LT` / `<`** - Devuelve `TRUE` si el primer valor es menor que el segundo.
- **`IS GEQ` / `>=`** - Devuelve `TRUE` si el primer valor es mayor o igual que el segundo.
- **`IS LEQ` / `<=`** - Devuelve `TRUE` si el primer valor es menor o igual que el segundo.


<br>

**Ejemplos**

```bash
PRINT 32 IS 32		# TRUE
PRINT 32 IS NOT 45	# TRUE
PRINT 40 IS LT 60 	# FALSE
PRINT $var == $name
```

<br>

### Operadores Lógicos

Estos operadores se utilizan para combinar múltiples expresiones booleanas:

- **`AND` (`&&`)** - Devuelve `TRUE` si ambas expresiones son verdaderas.
- **`OR` (`||`)** - Devuelve `TRUE` si al menos una de las expresiones es verdadera.
- **`NOT`** - Invierte el valor de una expresión booleana.


<br>

**Ejemplos**

```bash
PRINT $n > 10 AND $n < 45
PRINT TRUE OR FALSE
PRINT NOT TRUE == TRUE
```

<br>

### Operadores de Pertenencia

Un operador de pertenencia se emplea para identificar pertenencia en alguna secuencia (arrays, strings, mapas).

- **`IN` (`~~`)** - Devuelve `TRUE` si el valor especificado se encuentra en la secuencia.
- **`NOT IN` (`!~~`)** - Devuelve `TRUE` si el valor especificado no se encuentra en la secuencia.
- **`HAS` (`::`)** - Devuelve `TRUE` si la secuencia posee el valor especificado.
- **`HAS NOT` (`!::`)** - Devuelve `TRUE` si la secuencia no posee el valor especificado.

<br>

La diferencia entre `IN` y `HAS` radica en el orden de los elementos, siguiendo la estructura del inglés:

- {elemento} `IN` {secuencia}: Indica que el elemento está dentro de la secuencia.
- {secuencia} `HAS` {elemento}: Indica que la secuencia contiene el elemento.

**Ejemplos**

```bash
PRINT 32 IN [45, 67, 88]	# FALSE
PRINT "X" IN "John Xavier"	# TRUE
PRINT $fruits HAS "Naranja"	# TRUE
```

<br>

### Combinación de Datos

Scriptal permite una gran flexibilidad en la combinación de datos. Puedes sumar o combinar diferentes tipos de datos, como strings, arrays y mapas.

```bash
PRINT "Hola " + "Mundo"      # Hola Mundo
PRINT [1, 2] + [3, 4]        # [1, 2, 3, 4]
PRINT {"a": 1} + {"b": 2}    # {"a": 1, "b": 2}
```

<br>

### Operadores Especiales vs Conectores

En Scriptal, los operadores y conectores tienen usos específicos dependiendo del contexto en el que se emplean. Aquí se detallan las reglas para su uso:

#### Operadores especiales

Los operadores lógicos y de comparación (`IS`, `NOT`, `IN`, `AND`, `OR`, etc.), se utilizan únicamente en contextos booleanos y condicionales, tales como `WHEN`, `ORWHEN`, `WHILE`, `SET`, `PRINT`, y `BOOL`.

Estos operadores permiten unir diferentes elementos o realizar comparaciones avanzadas. En contextos no booleanos o condicionales, se consideran [**conectores**](#conectores).


<br>

#### Operadores Especiales como conectores

En un contexto no booleano o condicional, los operadores especiales serán interpretados como conectores y no como operadores.

<br>

**Contextos Booleanos**

`SET` y `PRINT` permiten un contexto booleano. Lo que significa que pueden usarse operadores especiales en ellos.

```bash
SET $is_equal AS 32 IS 32     # TRUE
PRINT 45 IS GT 36 AND 48 IS   # TRUE
```

<br>

**Contextos Condicionales**

`WHEN` ofrece un contexto condicional, lo que permite el uso de operadores especiales:

```bash
WHEN 32 IS NOT 45
    PRINT "It's not"
```

<br>


**Contextos normales**

`FIND` en cambio, no ofrece ningún contexto especial. Por lo tanto, los operadores especiales serán considerados conectores:

```bash
FIND "hello" IN "hello wolrd"
```

<br>

#### Alternativas

Si por alguna razón, necesitas usar operadores especiales en contextos no booleanos o condicionales, puedes usar los signos equivalentes. Por ejemplo, el comando `PICK`, recibe 3 parámetros: opción 1, `OR`, y la opción 2:

`PICK {option1} OR {option2}`

Si quisieras usar el operador especial `IN` para comprobar si un elemento se encuentra dentro de un array, puedes usar el signo equivalente, es decir `~~`:

**Ejemplo**

```bash
# Usamos:
PICK 32 ~~ [32, 45] OR FALSE 

# En lugar de:
# PICK 32 IN [32, 45] OR FALSE
```

<br>



## Condiciones

En Scriptal, las condiciones se manejan utilizando los comandos `WHEN`, `ORWHEN` y `ELSE`. Estos comandos permiten controlar el flujo del programa basándose en condiciones específicas.

### WHEN

El comando `WHEN` (conocido como "if" en otros lenguajes) se usa para evaluar una condición. Si la condición se cumple, se ejecuta el bloque de código asociado.

**Sintaxis**
```
WHEN {condición}
    {código}
```

<br>

**Ejemplo**
```bash
WHEN $age >= 18
    PRINT "Eres mayor de edad."
```

<br>

### ORWHEN

El comando `ORWHEN` se utiliza para evaluar una condición alternativa si la primera condición `WHEN` no se cumple. Puedes usar múltiples `ORWHEN` después de un `WHEN`.

**Sintaxis**
```
ORWHEN {condición}
    {código}
```

<br>

**Ejemplo**
```bash
WHEN $age > 18
    PRINT "Eres mayor de edad."
ORWHEN $age == 18
    PRINT "Tienes justo 18 años."
ORWHEN $age > 12
    PRINT "Eres un adolescente."
```

<br>

### ELSE

El comando `ELSE` se usa para definir un bloque de código que se ejecutará si ninguna de las condiciones `WHEN` o `ORWHEN` anteriores se cumple.

**Sintaxis**
```
ELSE
    {código}
```

<br>

**Ejemplo**
```bash
WHEN $age > 18
    PRINT "Eres mayor de edad."
ORWHEN $age == 18
    PRINT "Tienes justo 18 años."
ELSE
    PRINT "Eres menor de edad."
```

<br>


### Comandos AND y OR

Además de ser conectores y operadores, `AND` y `OR` también funcionan como comandos. Pueden ser usados debajo de los comandos `WHEN` y `ORWHEN`:

```bash
# Esto:
WHEN $num % 2 == 0
AND $num > 20
    PRINT "El número es par y mayor que 20."

# Es exactamente lo mismo que esto:
WHEN $num % 2 == 0 AND $num > 20
    PRINT "El número es par y mayor que 20."
```

<br>

### Interpretación como Booleanos

Scriptal interpreta otros tipos de datos como booleanos. Cualquier valor que no esté vacío, que no sea `FALSE` o `0` se considera `TRUE`.

```bash
SET $numero AS 10

WHEN $numero
    PRINT "El número es verdadero."    # Esto se imprimirá porque 10 se considera TRUE

SET $texto AS ""

WHEN $texto
    PRINT "El texto está vacío."    # Esto no se imprimirá porque una cadena vacía se considera FALSE
```

<br>

## Palabras Claves

Las palabras claves son valores constantes predefinidos que representan estados específicos o tipos de datos. Estas palabras claves tienen valores intrínsecos que se utilizan para verificar o establecer condiciones en el código. A continuación, se describen las principales palabras clave y sus valores intrínsecos:

**Generales:**

- `TRUE` - Representa el valor booleano verdadero.
- `FALSE` - Representa el valor booleano falso.
- `NULL` - Representa un valor nulo.
- `NAN` - Representa un valor no numérico.
- `INFINITY` - Representa un número infinito.

<br>

**Especiales:**

- `EMPTY` - Representa un valor vacío o nulo. Retorna `NULL`.
- `STRING` - Representa un valor de cadena de texto. Se utiliza para verificar o inicializar variables de tipo string. Retorna `""` (cadena vacía).
- `NUMBER` - Representa un valor numérico entero por defecto. Utilizado para verificar o inicializar variables numéricas. Retorna `0` (cero).
- `ARRAY` - Representa una colección. Utilizado para inicializar o verificar arrays. Retorna `[]` (array vacío).
- `MAP` - Representa un mapa. Utilizado para inicializar o verificar mapas. Retorna `{}` (mapa vacío).
- `BOOL` - Representa un valor booleano. Retorna `FALSE`.

<br>

**De Texto:**

- `DOT` - Representa un punto (`.`).
- `SPACE` - Representa un espacio en blanco (` `).
- `COMMA` - Representa una coma (`,`).
- `HASH` - Representa un símbolo de almohadilla (`#`).
- `TAB` - Representa un carácter de tabulación (`\t`).
- `NEWLINE` - Representa un salto de línea (`\n`).
- `ELLIPSIS` - Representa puntos suspensivos (`...`).

<br>

**Ejemplos:**

```bash
SET $var_str AS STRING
SET $var_num AS NUMBER

SPLIT $document BY NEWLINE AS $result
```


<br>

## Comparación de Datos

Puedes comparar datos utilizando una serie de comandos específicos diseñados para verificar el tipo de un valor. Estos comandos son útiles para asegurarte de que los datos cumplen con ciertos criterios antes de procesarlos.

### Comandos de Comparación

- `IS_STRING` - Verifica si un valor es un string (cadena de texto).
- `IS_NUMBER` - Verifica si un valor es un número (incluye tanto enteros como flotantes).
- `IS_FLOAT` - Verifica si un valor es un número flotante.
- `IS_INTEGER` - Verifica si un valor es un número entero.
- `IS_ARRAY` - Verifica si un valor es un array.
- `IS_MAP` - Verifica si un valor es un mapa.
- `IS_PATTERN` - Verifica si un valor es un patrón o expresión regular.
- `IS_BOOL` - Verifica si un valor es un booleano (`TRUE` o `FALSE`).
- `IS_NULL` - Verifica si un valor es `NULL`.

<br>

**Otros comandos:**

- `IS_EMPTY` - Verifica si un valor está vacío.
- `IS_NAN` - Verifica si un valor es NAN.
- `IS_INFINITY` - Verifica si un número es infinito.
- `IS_EVEN` - Verifica si un número es par.
- `IS_ODD` - Verifica si un número es impar.
- `IS_DATE` - Verifica si un valor es una fecha.


<br>

### Comparación con Palabras Claves

Puedes utilizar los [operadores de comparación](#operadores-de-comparación) `IS` o `IS NOT` junto palabras claves especiales dentro de condicionales o contextos booleanos para verificar el tipo de datos y tomar decisiones basadas en el resultado. 

**Sintaxis General:**

```
{valor} IS {palabra-clave-especial}
{valor} IS NOT {palabra-clave-especial}
```

<br>

**Ejemplos:**

```bash
WHEN 32 IS NUMBER
    PRINT "El valor es un número"

WHEN "hola" IS STRING
    PRINT "El valor es un string"

WHEN $text IS NOT EMPTY
    PRINT "El string no está vacío."

# "PRINT" y "SET" ofrecen un contexto booleano, por lo tanto, pueden usar operadores de comparación.
SET $nombre AS "John"
PRINT $nombre IS STRING     # TRUE

SET $valid AS $nombre IS NOT NUMBER
PRINT $valid                # TRUE
```
<br>

> [!CAUTION]
> Cuando se utilizan términos como `STRING`, `NUMBER`, `ARRAY`, etc., no se están refiriendo a los comandos para convertir datos. En su lugar, estos se refieren a **palabras clave** que se usan para describir el tipo de datos esperado.


```bash
PRINT NUMBER "23";      # Utilizamos el comando "NUMBER" para convertir el valor, retorna 23.
PRINT 23 IS NUMBER      # Utilizamos la palabra clave "NUMBER" para comparar, retorna TRUE.
```

<br>

Para que un elemento sea considerado comando interno, debe incluir un punto y coma `;` al final o la línea debe comenzar con éste. En contraste, las palabras clave, se utilizan directamente en las expresiones de comparación para verificar el tipo de dato.

<br>

**NOTA**

Aunque `EMPTY` y `NULL` tienen el mismo valor, la comparación entre ambos es muy distinta. Ambas keywords funcionan como `IS_EMPTY` y `IS_NULL` respectivamente:

```bash
PRINT IS_EMPTY [];  # TRUE
PRINT IS_NULL [];   # FALSE

PRINT [] IS EMPTY  # TRUE
PRINT [] IS NULL   # FALSE
```


<br>

### Comandos de Comparación de Strings

También existen comandos para verificar si los strings cumplen ciertas características, entre ellos:

`IS_EMAIL`, `IS_URL`, `IS_DIGIT`, `IS_NUMERIC`, `IS_ALPHA`, `IS_ALPHANUMERIC`.

Revisa la lista completa de comandos para comparación de strings [aquí](Commands_ES.md#comparación-de-strings).




<br>

**Ejemplos:**

```bash
WHEN IS_EMAIL "johnsmith@gmail.com";
    PRINT "Valid Email"

WHEN NOT IS_URL "323232";
    PRINT "Invalid URL"

WHEN IS_ALPHANUMERIC "helloworld";
    PRINT "Only alphanumeric!"
```

<br>


## Procesamiento de texto

El propósito principal de Scriptal es el procesamiento de texto, ya sea desde un string directo o desde un archivo. El procesamiento de texto se inicia cargando el contenido que se desea manipular.

### Cargar contenido

Para comenzar a trabajar con texto en Scriptal, utilizamos el comando `INPUT`, que permite cargar contenido desde diversas fuentes:

- **Desde un String:**
```bash
INPUT "Hello World"
```

<br>

- **Desde un Archivo:**
```bash
INPUT FROM "file.txt"
```

<br>

En pocas palabras, `INPUT` nos permite leer el contenido de un archivo o un string y ofrecer ciertas funciones para procesar dicha información.

Una vez cargado el contenido, se habilitan características adicionales que facilitan el procesamiento del texto. Estas características incluyen:

- [ **Eventos** ](#eventos)
- [**Variables de entorno**](#variables-de-entorno)

<br>

> [!IMPORTANT]
> Solo se puede cargar contenido una sola vez por archivo.

<br>


### Procesar y Almacenar

Puedes guardar fragmentos del texto procesado y exportarlos a un archivo nuevo. Aunque puedes usar comandos como `FILE_WRITE` para manejar archivos, una forma sencilla es con el comando `STORE`. Este comando guarda cadenas de texto como nuevas líneas, que puedes exportar o procesar más tarde según lo necesites.

```bash
STORE "Hello World"
STORE UPPERCASE "Como";  # Almacena la cadena "COMO"
STORE REVERSE "ESTAS";   # Almacena la cadena "SATSE"
```

<br>

### Exportar

Para exportar las líneas que has almacenado previamente, usa el comando `OUTPUT`. Este comando combina todas las líneas guardadas y las escribe en un solo archivo.

```bash
OUTPUT "file.txt"
```

<br>

Si necesitas borrar todas las líneas almacenadas, utiliza el comando `CLEAR_STORAGE`.


<br>

**Notas adicionales:**

El comando `OUTPUT` es una forma práctica y general de exportar el contenido almacenado, pero no es la única opción. También puedes usar otros comandos de manipulación de archivos como `FILE_WRITE`, `JSON_SAVE`, etc. para realizar operaciones similares.

<br>

## Eventos

En Scriptal, los eventos permiten ejecutar comandos en momentos específicos del proceso de carga y análisis del contenido. Los eventos ayudan a organizar el flujo de ejecución y a realizar tareas en diferentes etapas del procesamiento.

### Eventos principales

1. `START` - Se activa al inicio del proceso. Es útil para declarar variables, configurar el entorno o realizar preparativos iniciales.
2. `END` - Se activa al finalizar el proceso. Se usa para realizar tareas de cierre, como ajustes finales o liberar recursos.
3. `GLOBAL` - Es el **evento predeterminado**. Cualquier bloque de código que no esté específicamente asignado a otro evento se ejecutará aquí.

<br>

### Eventos de línea

1. `EACH_LINE` - Se ejecuta para cada línea del contenido cargado.
2. `EVEN_LINES` - Se ejecuta en las líneas con un índice par.
3. `ODD_LINES` - Se ejecuta en las líneas con un índice impar.
4. `CONTENT_LINES` - Se ejecuta para líneas que contienen texto.
5. `EMPTY_LINES` - Se ejecuta para líneas vacías.

<br>

### Definir Eventos

Para definir un evento, se utiliza el comando `ON`, seguido del nombre del evento. Después, se proporciona el código que se ejecutará cuando se active el evento.

**Ejemplo:**
```bash
ON START
    PRINT "Inicio del proceso"

ON END
    PRINT "Fin del proceso"

ON EACH_LINE
    PRINT "Cada línea"

ON EMPTY_LINES
    PRINT "Línea vacía detectada"
```

<br>


Cuando se deja un comando fuera de un evento, éste se define dentro del **evento global** automáticamente:

```bash
PRINT "First, I am outside"        # Evento GLOBAL
ON START
	PRINT "Second, I am inside"    # Evento START
```

<br>

### Orden de Ejecución

La ejecución de eventos sigue el siguiente orden de prioridad, lo que garantiza un flujo controlado en el procesamiento del contenido:

- `START`
- `GLOBAL`
- `EACH_LINE`, `EVEN_LINES`, `ODD_LINES`, `CONTENT_LINES`, `EMPTY_LINES`
- `END`

<br>

**Ejemplo:**
```bash
PRINT "Primero, estoy fuera"        # Evento GLOBAL

ON START
    PRINT "Segundo, estoy dentro"   # Evento START
```

<br>

**Resultado de ejecución del ejemplo:**
```
Segundo, estoy dentro
Primero, estoy fuera
```
<br>


> [!IMPORTANT]
> El comando `ON` no debe estar indentado y no puede recibir variables como parámetros.

<br>

## Variables de entorno

Las variables de entorno en Scriptal, también conocidas como envars, son variables especiales que el usuario no puede modificar directamente. Estas variables se generan dinámicamente en función del contexto de ejecución y están disponibles en diferentes etapas del procesamiento del texto.

### Contexto Global

- `@CONTENT` - Devuelve todo el contenido que se ha cargado previamente.
- `@LINES`   - Devuelve todas las líneas del contenido cargado.
- `@STORED`  - Devuelve las líneas almacenadas.

<br>

**Ejemplo:**
```bash
INPUT "Texto de prueba\nSegundo texto"
PRINT @CONTENT       # Muestra el contenido completo
PRINT @LINES         # Muestra todas las líneas del contenido
```

<br>

### Contexto en Líneas

Estas variables están disponibles solo dentro de eventos que procesan líneas, como `EACH_LINE`, `EVEN_LINES`, `ODD_LINES`, `CONTENT_LINES`, y `EMPTY_LINES`:

- `@LINE` - Devuelve la línea actual siendo procesada.
- `@LINE_INDEX` - Devuelve el índice de la línea actual.
- `@WORDS` - Devuelve todas las palabras de la línea actual como un array.
- `@CHARS` - Devuelve todos los caracteres de la línea actual como un array.

<br>

**Ejemplo:**
```coffeescript
ON EACH_LINE
    PRINT @LINE                # Muestra la línea actual
    PRINT @LINE_INDEX          # Muestra el índice de la línea actual
    PRINT @WORDS               # Muestra todas las palabras en la línea actual
    PRINT @CHARS               # Muestra todos los caracteres en la línea actual
```

<br>

### Contexto en Ciclos

Estas variables están disponibles solo dentro del ciclo `ITERATE` y `REPEAT`:

- `@KEY` - Devuelve el valor de la iteración actual. Si no existe, devuelve el índice.
- `@INDEX` - Devuelve el índice de la iteración actual.

<br>

**Ejemplo:**
```coffeescript
SET $array AS ["a", "b", "c"]
ITERATE $array
    PRINT @KEY       # Muestra el valor de la iteración actual
    PRINT @INDEX     # Muestra el índice de la iteración actual
```

<br>

### Contexto en Definiciones

Estas variables están disponibles dentro de una definición, usando el comando `DEFINE`:

- `@PARAMS` - Devuelve un array con los parámetros del comando utilizado.
- `@SYNTAX` - Devuelve el índice de la sintaxis actual.
- `@SELF` - Permite exportar el comando desde su definición.

<br>

**Ejemplo:**
```coffeescript
DEFINE MI_FUNCION
    PARAMS [STRING]
    PRINT @PARAMS   # Muestra los parámetros pasados a la función

MI_FUNCION "param1"

```

<br>

## Bucles

En Scriptal, los bucles permiten repetir bloques de código bajo ciertas condiciones. Hay dos tipos de bucles principales: `ITERATE` y `REPEAT`, además de `WHILE` para condiciones de repetición más flexibles.



<br>

### Bucle ITERATE

El bucle `ITERATE` permite iterar sobre elementos de un array, caracteres de un string o un rango de números. Puedes usar `ITERATE` para recorrer arrays y realizar operaciones en cada elemento, o para recorrer un rango de números.

Durante la ejecución de este bucle, las variables de entorno `@KEY` y `@INDEX` están disponibles para usarse dentro del mismo.


**Iterar un Array**

```bash
SET $array AS ["Manzana", "Pera", "Banana"]

ITERATE $array
    PRINT @KEY
```

<br>

**Definir Variable para Valor de Iteración**

Puedes definir una variable para almacenar el valor actual de la iteración usando el conector `AS`:

```bash
ITERATE $array AS $key
    PRINT $key
```

<br>

Si necesitas obtener el índice, puedes definir 2 variables dentro de un array:

```bash
ITERATE $array AS [$index, $key]
    PRINT "N° " + $index + " - " + $key
```

<br>

**Iterar sobre un Rango**

Puedes iterar sobre un rango de números usando los conectores `FROM` y `TO`. Puedes especificar un rango ascendente o descendente:

```bash
# Iterar de 1 a 100
ITERATE FROM 1 TO 100
    PRINT @KEY

# Iterar de 1 a 100 con variable
ITERATE FROM 1 TO 100 AS $count
    PRINT $count
    
# Iterar de 100 a 20 en cuenta regresiva
ITERATE FROM 100 TO 20
    PRINT @KEY
```

<br>

### Bucle REPEAT

El bucle `REPEAT` ejecuta un bloque de código un número específico de veces. Es útil cuando necesitas repetir una acción un número determinado de veces.

```bash
SET $n AS 0
REPEAT 5
    INCREMENT $n
    PRINT "Hey you! Counting " + $n + " times!"
```

<br>

### Bucle WHILE

El bucle `WHILE` permite repetir un bloque de código mientras se cumpla una condición específica. Es útil cuando el número de repeticiones no es fijo, sino que depende de una condición que puede cambiar durante la ejecución.

**Sintaxis del Bucle WHILE**

```bash
WHILE {condition}
    # Comandos a ejecutar mientras la condición sea verdadera
```

<br>

**Ejemplo:**
```bash
SET $count AS 1

WHILE $count <= 5
    PRINT "Count: " + $count
    SET $count AS $count + 1
```

<br>


### Comando BREAK

El comando `BREAK` se usa para salir de un bucle de manera inmediata. Al ejecutar `BREAK`, el flujo de control se interrumpe y el programa continúa con el código que sigue después del bucle.


**Ejemplo:**
```bash
SET $numbers AS [1, 2, 3, 4, 5]

ITERATE $numbers
    WHEN @KEY IS 3
        PRINT "Breaking out of the loop"
        BREAK
    PRINT @KEY
```

<br>

### Comando CONTINUE

El comando `CONTINUE` se usa para saltar a la siguiente iteración de un bucle, omitiendo el código restante en la iteración actual. Esto es útil para saltarse ciertas condiciones y continuar con el siguiente ciclo del bucle.

**Ejemplo:**
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
> Los comandos `BREAK` y `CONTINUE` solo tienen efecto dentro de los bucles `ITERATE`, `REPEAT` y `WHILE`.

<br>

## Definir comandos

En Scriptal, puedes crear tus propios comandos personalizados y ejecutarlos en cualquier parte del código. Para definir un nuevo comando, se utiliza el comando `DEFINE` seguido del nombre del comando. El nombre no puede contener signos, a excepción de `_`, y no puede comenzar con números.

**Ejemplo:**
```bash
# Definir el comando
DEFINE SALUDAR
    PRINT "HEY"

# Ejecutar el comando
SALUDAR
```

<br>

### Parámetros

Puedes hacer que tus comandos sean dinámicos utilizando parámetros o argumentos. Para definir parámetros, usa el comando `PARAMS` dentro de la definición del comando.

**Sintaxis**: `PARAMS {ARRAY}`

`PARAMS` requiere un solo parámetro, el cual debe ser un array con los tipos de datos que se esperan. Los parámetros válidos son:

- `STRING` - Solo permite textos.
- `NUMBER` - Solo permite números.
- `ARRAY` - Solo permite arrays o listas.
- `MAP` - Solo permite mapas.
- `BOOL` - Solo permite booleanos.
- `NULL` - Solo permite valores nulos.
- `PATTERN` - Solo permite patrones (expresiones regulares).
- `ANY` - Permite cualquier valor.
- `VAR` - Permite el nombre de una variable en forma cruda. Es decir, no resolverá la variable.

<br>

También puedes seleccionar distintos tipos de arrays:

- `ARRAY_STRING` - Solo permite arrays de strings.
- `ARRAY_NUMBER` - Solo permite arrays de números.
- `ARRAY_BOOL` - Solo permite arrays de booleanos.
- `ARRAY_ARRAY` - Solo permite arrays de arrays.
- `ARRAY_MAP` - Solo permite arrays de mapas.
- `ARRAY_VAR` - Solo permite arrays de variables crudas.

<br>


Para acceder a los parámetros, se utiliza la variable de entorno `@PARAMS`.

**Ejemplo:**
```bash
DEFINE SALUDAR
    PARAMS [STRING]
    PRINT "Hola " + @PARAMS.1

SALUDAR "Martin" # Resultado: Hola Martin
SALUDAR 3232 # ERROR: Se esperaba un STRING, no un NUMBER.
```

<br>


**Múltiples parámetros:**

Si quieres usar varios parámetros, puedes extender `PARAMS` de la siguiente forma:


```bash
DEFINE SALUDAR_A
	PARAMS [STRING, STRING, STRING] # Requiere 3 parámetros de tipo STRING
	PRINT "Hola " + @PARAMS.1
	PRINT "Hola " + @PARAMS.2
	PRINT "Hola " + @PARAMS.3

SALUDAR_A "Martin" "Luis" "Manuel"
```


<br>

**Parámetros de Distintos Tipos:**

Si deseas usar varios tipos de datos para un parámetro, puedes anidar los tipos dentro de un array:

```bash
DEFINE PROCESAR
    PARAMS [NUMBER, [STRING, NUMBER], NUMBER]
    PRINT "Número: " + @PARAMS.1
    PRINT "Texto o Número: " + @PARAMS.2
    PRINT "Número final: " + @PARAMS.3

PROCESAR 1 "texto" 3
PROCESAR 1 2 3
```

<br>

**Conectores como Parámetros**

Cuando defines los parámetros con `PARAMS`, los conectores se reconocen y se filtran automáticamente, dejando solo los valores que realmente necesitas para el procesamiento. Esto significa que `@PARAMS` solo reconocerá tipos de datos y no conectores.

```bash
DEFINE CONECTAR
    PARAMS [STRING, TO, STRING]
    PRINT "Conectando " + @PARAMS.1 + " con " + @PARAMS.2

CONECTAR "A" TO "B" # Resultado: Conectando A con B
```

<br>


### Múltiples Sintaxis

En Scriptal, no existen parámetros opcionales de forma nativa. Sin embargo, puedes simular esta funcionalidad definiendo distintas sintaxis para un mismo comando utilizando el comando `PARAMS` más de una vez. Cada uso de `PARAMS` define una sintaxis distinta. 

La sintaxis seleccionada estará disponible en la variable de entorno `@SYNTAX`:

```bash
DEFINE TIPO_DE
    PARAMS [STRING]             # Syntax 1
    PARAMS [NUMBER]             # Syntax 2
    PARAMS [STRING, STRING]     # Syntax 3

    WHEN @SYNTAX IS 1
        PRINT "Soy un string"
        
    WHEN @SYNTAX IS 2
        PRINT "Soy un número"
    
    WHEN @SYNTAX IS 3
        PRINT "Somos strings"

TIPO_DE "hola"          # Resultado: Soy un string
TIPO_DE 123             # Resultado: Soy un número
TIPO_DE "hola" "mundo"  # Resultado: Somos strings
```

En este ejemplo, el comando `TIPO_DE` puede aceptar un string, un número o dos strings, y actuará de acuerdo al tipo y cantidad de parámetros que reciba.

<br>

### Comando RETURN

El comando `RETURN` se utiliza para devolver un valor desde un comando definido por el usuario. Esto permite crear funciones más complejas y reutilizables.


```bash
DEFINE SUMA
    PARAMS [NUMBER, NUMBER]
    SET $resultado AS @PARAMS.1 + @PARAMS.2
    RETURN $resultado

SET $total AS SUMA 5 10;
PRINT $total # Resultado: 15   
```

<br>

### Alcance Global de los Comandos

Todos los comandos son globales y no tienen un alcance limitado (scope), pero solo estarán disponibles después de ser definidos. Esto significa que un comando no se puede utilizar antes de su definición en el código. Además, debido a esta naturaleza global, **no es posible definir un comando dentro de otro comando**.

<br>

Es importante definir cada comando de manera independiente para evitar conflictos y asegurar que el comando sea accesible desde cualquier parte del código después de su definición.

<br>

### Comando RASSIGN

El comando `RASSIGN` permite exponer variables a contextos superiores, facilitando la comunicación entre diferentes [scopes](#alcance-scope-de-las-variables). Aquí se describe su uso y propósito.   


#### Alcance de las Variables
Las variables creadas dentro de una definición (mediante `DEFINE`) están disponibles únicamente en ese contexto. Esto significa que no se pueden acceder desde fuera de la definición.

```bash
# Definimos el comando
DEFINE COMMAND
    SET $name AS "Mike"
    PRINT $name # Imprime la variable $name (dentro de la definición)

# Ejecutamos el comando
COMMAND

PRINT $name # Error: La variable no existe. Esto se debe a que la variable se creó dentro de la definición.
```

Hasta ahora, la única forma conocida de exponer una variable a un contexto superior era mediante la creación de una variable global utilizando el comando `GLOBAL_SET`.

<br>

#### Exponer Variables

El comando `RASSIGN` permite crear una variable en un contexto superior al finalizar la ejecución del comando definido. Esto facilita el acceso a variables creadas dentro de definiciones desde el contexto donde se invoca el comando.

**Sintaxis**

`RASSIGN {ANY} TO {VAR: STR}`

<br>

**Ejemplo:**

```bash
DEFINE EXPOSE_VAR
    RASSIGN "Hello World" TO "$var" # Crea la variable "$var" en el scope superior.
    PRINT $var   # Error: Variable desconocida, ya que $var no está en el contexto actual.

# Ejecutamos el comando
EXPOSE_VAR
PRINT $var  # "Hello World". La variable se creó en el contexto superior.
```

<br>

#### Variable Nula

Si la variable que recibe `RASSIGN` es `NULL`, el comando **retornará el valor** sin crear la variable:


**Ejemplo:**
```bash
DEFINE TESTING
    PRINT RASSIGN "Hello World" TO NULL;    # Hello World

TESTING # Hello World
# No se crean variables
```

<br>


#### Usos Avanzados de `RASSIGN`

El verdadero propósito de `RASSIGN` es permitir la creación de variables en el contexto superior utilizando la sintaxis del [conector `AS`](#conector-as), simulando la sintaxis de la mayoría de los comandos en Scriptal.

**Ejemplo:**
```bash
UPPERCASE "Hola" AS $upper
PRINT $upper    # HOLA

PRINT UPPERCASE "Mundo";
```

En este caso, `UPPERCASE` puede ser utilizado tanto como un [comando interno](#comandos-internos) como un comando independiente.

<br>

Puedes definir comandos personalizados que exponen variables en el contexto superior.

**Ejemplo:**

```bash
DEFINE STRETCH_WORD
    # Creamos 2 sintaxis
    PARAMS [STRING]
    PARAMS [STRING, AS, VAR]

    # Obtenemos el valor del parámetro #2, si no existe, retornará NULL.
    GET 2 IN @PARAMS AS $VAR

    # Ejemplo: Dividir el string en caracteres individuales y unir con guiones
    SPLIT @PARAMS.1 BY "" AS $word
    JOIN $word WITH "-" AS $result  # Ejemplo: h-e-l-l-o

    # Retorna el resultado si $VAR no se define
    # De lo contrario, asigna el resultado a la variable sin retornar nada.
    RETURN RASSIGN $result TO $VAR;

# Ejecutamos como comando interno
PRINT STRETCH_WORD "Hola";

# Ejecutamos como un comando independiente y almacenamos el valor en una variable.
STRETCH_WORD "WORLD" AS $stretched
PRINT $stretched
```

<br>

En este ejemplo, `STRETCH_WORD` puede ser invocado tanto como un comando interno para imprimir el resultado directamente, como un comando independiente para almacenar el resultado en una variable.

<br>

## Manejo de Errores

En Scriptal, el manejo de errores es crucial para garantizar que tu código funcione correctamente y para identificar problemas en tiempo de ejecución. A continuación, se describen los comandos y prácticas recomendadas para gestionar errores en Scriptal:

### Comando TRY

Inicia un bloque de código en el que se pueden capturar y manejar errores. Usa `TRY` para envolver el código que podría generar una excepción. Se debe usar junto al comando `CATCH`.

**Sintaxis:**
```bash
TRY
    # Código que puede causar errores
```

Los errores de tipo `SyntaxError` no pueden ser capturados.

<br>

### Comando CATCH

Se utiliza para manejar cualquier error que ocurra dentro del bloque `TRY`. Especifica el código que debe ejecutarse si se detecta un error.

**Sintaxis:**
```bash
CATCH
    # Código para manejar el error
```

También se puede utilizar `CATCH AS {VAR}` para guardar los detalles del error en una variable.

**Ejemplo:**
```bash
TRY
    PRINT 3 / 0 # No se puede dividir entre 0
CATCH AS $error
    PRINTF $error
```

<br>

### Comando ERROR

Permite lanzar un error genérico a través de un string.

**Ejemplos:**
```bash
ERROR "Algo ha ocurrido"
```

<br>

### Comando THROW

Permite lanzar un error manualmente. Utiliza `THROW` para generar una excepción con un mensaje específico. Normalmente se debe utilizar con un comando que genere errores en forma de mapas, entre ellos: `SYNTAX_ERROR`, `VALUE_ERROR`, etc.

**Ejemplos:**
```bash
THROW VALUE_ERROR ["El valor no es válido"];
THROW RUNTIME_ERROR ["Algo malo ha ocurrido."];
```

<br>

Revisa la lista completa de comandos para manejo de errores [aquí](Commands_ES.md#manejo-de-errores).


**Ejemplo**

```bash
TRY
    SET $value AS 10
    THROW VALUE_ERROR ["Error de prueba"];
CATCH AS $error_message
    PRINT "Se ha producido un error:"
    PRINTF $error_message
```


<br>

## Módulos

En Scriptal, puedes organizar tu código en módulos para mantenerlo limpio, reutilizable y fácil de mantener. Los módulos permiten incluir definiciones de comandos, variables y cualquier otra lógica necesaria. La funcionalidad de módulos es una característica **BETA**, por lo que algunas características están en desarrollo.

### Importar Módulos

Para importar un módulo, usa el comando `IMPORT` seguido del nombre del archivo del módulo como un string. La extensión del archivo es opcional. El archivo debe estar en la misma carpeta que tu script principal o en una ruta accesible.

**Ejemplo:**

Supongamos que tienes un archivo llamado `saludos.stal` con el siguiente contenido:

```bash
# Archivo: saludos.stal

DEFINE SALUDAR
    PRINT "Hola!"

DEFINE DESPEDIR
    PRINT "Adiós!"
    
EXPORT "SALUDAR"
EXPORT "DESPEDIR"
 
```

<br>

Para importar y utilizar este módulo en tu script principal, harías lo siguiente:

```bash
# Archivo principal

IMPORT "saludos"

SALUDAR   # Resultado: Hola!
DESPEDIR  # Resultado: Adiós!
```

<br>

### Exportar Variables y Comandos

Para compartir variables o comandos de un módulo a otro, se utiliza el comando `EXPORT`. 


<br>

**Variables:**

Puedes exportar variables de manera individual:

```bash
SET $URL AS "https://example.com"
SET $CONSTANT AS "Hello World!"

EXPORT $URL
EXPORT "$CONSTANT"
```

<br>

**Comandos:**

El comando se puede exportar como string o desde la definición del mismo, utilizando `EXPORT @SELF`:

```bash
DEFINE SALUDAR
    PRINT "Hola"

EXPORT "SALUDAR"

DEFINE CONTAR
    EXPORT @SELF
    ITERATE FROM 1 TO 10 AS $i
        PRINT $i
```

<br>

En el script principal, puedes acceder a estas variables o comandos de la siguiente manera:

```bash
IMPORT "module"

PRINT $URL

SALUDAR
CONTAR
```

<br>

### Módulos Complejos

Los módulos se importan completos y no por porciones. No puedes escoger comandos específicos para importar. Además, si un módulo llama a otro que ya ha sido llamado, se lanzará un error de importación circular.

Esto se debe a que en Scriptal no existen ni métodos ni atributos.

<br>

## Librerías

Las librerías permiten extender las capacidades del lenguaje, proporcionando funciones y utilidades adicionales que puedes importar y utilizar en tus scripts. Las librerías funcionan de manera similar a los módulos, pero están predefinidas y disponibles para todos los usuarios de Scriptal.

### Importar librerías

Para importar una librería, se utiliza el comando `IMPORT` seguido del nombre de la librería como un string. Si el nombre especificado no corresponde a un archivo en la raíz del proyecto, Scriptal buscará en las librerías predefinidas.

Supongamos que quieres importar la librería `COLORS`, que proporciona una variedad de funciones relacionadas con colores. Puedes hacerlo de la siguiente manera:

```bash
IMPORT "COLORS"
```

<br>

Si `COLORS` _(COLORS.stal)_ no es un archivo en la raíz del proyecto, Scriptal buscará en las librerías predefinidas. 

<br>

Las librerías están ubicadas en el directorio `modules` en la raíz del ejecutable de Scriptal. Cada librería debe estar dentro de una carpeta con el mismo nombre para que pueda ser reconocida por Scriptal:

```
modules/
    MyLibrary/
        MyLibrary.stal
```

<br>

### Uso de librerías

Una vez importada la librería, puedes utilizar sus funciones y variables como si fueran parte de tu script.

```bash
# Importar la librería COLORS
IMPORT "COLORS"

# Usar una función de la librería COLORS
SET $color AS "#FF5733"
COLOR_DARKEN $color WITH 50 AS $darker_color
PRINT $darker_color  # Resultado: Un color más oscuro que el original

# Otra función de COLORS
COLOR_INVERT $color AS $inverted_color
PRINT $inverted_color  # Resultado: El color invertido
```

<br>

### Librerías disponibles


| Nombre   | Descripción                                                         | Documentación                      |
|----------|---------------------------------------------------------------------|------------------------------------|
| `COLORS` | Ofrece una serie de comandos para manipular y trabajar con colores. | [Ver](../modules/COLORS/COLORS.md) |


<br>

## Convenciones

En Scriptal, el uso de nombres para comandos o funciones sigue un conjunto de convenciones diseñadas para mantener la claridad y la consistencia en el código. A continuación, se describen en detalle las principales convenciones:

<br>

### Separación de Palabras

Cuando un comando o función está compuesto por más de una palabra, se utiliza un guion bajo (`_`) para separar cada palabra. Esto ayuda a distinguir claramente cada palabra dentro del comando, mejorando la legibilidad.


Ejemplo: `FILE_OPEN`

<br>

### Abreviaturas

En los casos donde una palabra se abrevia, no se utiliza el guion bajo para separar las palabras dentro del comando. Esto aplica incluso si el comando consta de más de una palabra. La falta de guion bajo en abreviaturas permite una forma compacta y reconocible de los comandos. 

Ejemplo: `SQRT` (Square Root), `PRINTF` (Print Format), etc.

<br>

### Glosario

Scriptal mantiene un glosario de palabras comúnmente abreviadas que se consideran como completas dentro del contexto de los comandos y funciones. Estas abreviaturas están predefinidas y reconocidas para facilitar la escritura de comandos.

- **VAR**: VARIABLE
- **CMD**: COMMAND
- **DIFF**: DIFFERENCE
- **DEC**: DECIMAL
- **HEX**: HEXADECIMAL
- **OCT**: OCTADECIMAL
- **BIN**: BINARY

<br>

Por eso, existen comandos como `VAR_EXISTS`, etc., en lugar de `VARIABLE_EXISTS`.


<br>

### Variables

Las variables siguen la convención de nombres `snake_case`, similar a Python. Esto significa que las palabras se separan por guion bajo y todas las letras están en minúsculas.

```bash
SET $user_name AS "Martin"
```

<br>

### Constantes

Aunque Scriptal no tiene soporte para constantes en el sentido tradicional, se recomienda utilizar nombres en mayúsculas para variables que deberían ser tratadas como constantes. Esto ayuda a distinguir visualmente las variables que no deben cambiar de valor.

```bash
SET $MAX_CONNECTIONS AS 10
```

<br>

## Recursos Adicionales

- [Lista Completa de Comandos](./Commands_ES.md)
- [Ejemplos de Código](../examples/)


