# COLORS

- **Version:** 0.1.1
- **Author:** Martin Amaro

> [!NOTE]
> This library is a work in progress, and additional commands will be added in the future.

----

<br>

The `COLORS` library in Scriptal provides functions to manipulate and work with colors. Below is a detailed description of the available commands and their usage.

## Commands

**COLOR_IS_VALID**

- **Syntax:** `COLOR_IS_VALID {COL: STRING} [AS {VAR}]`

- **Description:** Check if a color is valid.

- **Example:**

  ```bash
  PRINT COLOR_IS_VALID "#00aaff";
  ```

<br>

**COLOR_RANDOM**

- **Syntax:** `COLOR_RANDOM [AS {VAR}]`

- **Description:** Generate a random color.

- **Example:**

  ```bash
  PRINT COLOR_RANDOM;
  ```

<br>

**COLOR_DARKEN**

- **Syntax:** `COLOR_DARKEN {COL: STRING} WITH {PERC:NUMBER} [AS {VAR}]`

- **Description:** Darken a color by a specified percentage (0% to 100%).

- **Example:**

  ```bash
  COLOR_DARKEN "#00aaff" WITH 40 AS $darken
  ```

<br>

**COLOR_LIGHTEN**

- **Syntax:** `COLOR_LIGHTEN {COL: STRING} WITH {PERC:NUMBER} [AS {VAR}]`

- **Description:** Lighten a color by a specified percentage (0% to 100%).

- **Example:**

  ```bash
  COLOR_LIGHTEN "#00aaff" WITH 40 AS $lighten
  ```

<br>

**COLOR_INVERT**

- **Syntax:** `COLOR_INVERT {COL: STRING} [AS {VAR}]`

- **Description:** Inverts the values of a specified color. The command takes a color in hex format (e.g., `#RRGGBB`) and returns its inverse by changing each component (red, green, and blue) to its opposite value within the range of 00 to FF. This is commonly used to get the complementary color, which is the visual inverse of the original.

- **Example:**

  ```bash
  COLOR_INVERT "#00FF00" AS $inverted_color
  ```

<br>

**COLOR_GRAYSCALE**

- **Syntax:** `COLOR_GRAYSCALE {COL: STRING} [AS {VAR}]`

- **Description:** Converts the specified color to grayscale. The command takes a color and computes its grayscale equivalent by averaging the red, green, and blue components. The result is a color where all components have the same value, representing the grayscale intensity.

- **Example:**

  ```bash
  PRINT COLOR_GRAYSCALE "#00aaff";
  ```

<br>

**COLOR_BLEND**

- **Syntax:** `COLOR_BLEND {COL: STRING} AND {COL: STRING} WITH {PERC:NUMBER} [AS {VAR}]`

- **Description:** Blends two colors together based on the specified percentage. The command takes two colors in hexadecimal format and combines them. The `PERC` value determines the blend ratio between the two colors, where 0% results in the first color and 100% results in the second color. Intermediate values produce a mix of the two colors.

- **Example:**

  ```bash
  PRINT COLOR_BLEND "#00aaff" AND "#ffaa00" WITH 50;
  ```

<br>

**COLOR_MIX**

- **Syntax:** `COLOR_MIX {COL: STRING} AND {COL: STRING} [AS {VAR}]`

- **Description:** Mixes two colors together in equal proportions. The command takes two colors in hexadecimal format and calculates the average of their RGB components to produce a new mixed color. This method creates a color that is a blend of the two input colors with equal weight.

- **Example:**

  ```bash
  PRINT COLOR_MIX "#00aaff" AND "#ffaa00";
  ```

<br>

**COLOR_SELECT_DARKEST**

- **Syntax:** `COLOR_SELECT_DARKEST {COLORS: ARRAY} [AS {VAR}]`

- **Description:** Selects the darkest color from a provided array of colors. The command takes an array of colors in hexadecimal format and identifies the color with the lowest brightness or value. The resulting color is the one with the least intensity, making it the darkest among the given options.

- **Example:**

  ```bash
  PRINT COLOR_SELECT_DARKEST ["#000", "#FFF", "#DDD"];
  ```

<br>


**COLOR_SELECT_LIGHTEST**

- **Syntax:** `COLOR_SELECT_LIGHTEST {COLORS: ARRAY} [AS {VAR}]`

- **Description:** Selects the lightest color from a provided array of colors. The command takes an array of colors in hexadecimal format and identifies the color with the highest brightness or value. The resulting color is the one with the greatest intensity, making it the lightest among the given options.

- **Example:**

  ```bash
  PRINT COLOR_SELECT_LIGHTEST ["#000", "#FFF", "#DDD"];
  ```

<br>

**COLOR_TO_RGB**

- **Syntax:** `COLOR_TO_RGB {COL: STRING} [AS {VAR}]`

- **Description:** Converts a color from hexadecimal format to its RGB (Red, Green, Blue) components. The command takes a color in hexadecimal format (e.g., `#RRGGBB`) and returns its equivalent RGB values as an array, where each component is represented by a number between 0 and 255.

- **Example:**

  ```bash
  PRINT COLOR_TO_RGB "#FF0000";
  ```

<br>

**COLOR_MAKE_RGB**

- **Syntax:** `COLOR_MAKE_RGB {RGB: ARRAY} [AS {VAR}]`

- **Description:** Creates a color in hexadecimal format from the provided RGB (Red, Green, Blue) components. The command takes an array of three numbers representing the red, green, and blue values (each between 0 and 255) and converts them into a hexadecimal color code

- **Example:**

  ```bash
  PRINT COLOR_MAKE_RGB [240, 50, 30];
  ```

<br>


## Change Log

### 0.1.1 (Aug 27, 2024)

+ Basic color manipulation.