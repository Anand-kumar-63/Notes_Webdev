
## Character and Length Methods

| **Method**                    | **Return Type** | **Description**                                                                                      | **Example**                              |
| ----------------------------- | --------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| **`length()`**                | `int`           | Returns the number of characters in the string.                                                      | `"Java".length()` returns `4`.           |
| **`charAt(int index)`**       | `char`          | Returns the character at the specified index (0-based).                                              | `"Java".charAt(1)` returns `'a'`.        |
| **`indexOf(String str)`**     | `int`           | Returns the index of the **first** occurrence of the specified substring. Returns `-1` if not found. | `"hello".indexOf("ll")` returns `2`.     |
| **`lastIndexOf(String str)`** | `int`           | Returns the index of the **last** occurrence of the specified substring.                             | `"banana".lastIndexOf("a")` returns `5`. |
| **`isEmpty()`**               | `boolean`       | Checks if the string has a length of 0.                                                              | `"".isEmpty()` returns `true`.           |
| **`isBlank()`**               | `boolean`       | Checks if the string is empty or contains only whitespace (added in Java 11).                        | `" ".isBlank()` returns `true`.          |

## Comparison Methods
| **Method**                                   | **Return Type** | **Description**                                                                                                                                                                                                      |
| -------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`equals(Object obj)`**                     | `boolean`       | Compares the content of two strings for equality (case-sensitive). **Use this** instead of `==` for content comparison.                                                                                              |
| **`equalsIgnoreCase(String anotherString)`** | `boolean`       | Compares the content of two strings for equality, ignoring case differences.                                                                                                                                         |
| **`compareTo(String anotherString)`**        | `int`           | Compares two strings lexicographically (based on Unicode value). Returns 0 if strings are equal, a negative value if the current string is lexicographically less than the argument, and a positive value otherwise. |
| **`contains(CharSequence s)`**               | `boolean`       | Checks if the string contains the specified sequence of characters.                                                                                                                                                  |
| **`startsWith(String prefix)`**              | `boolean`       | Tests if the string begins with the specified prefix.                                                                                                                                                                |
| **`endsWith(String suffix)`**                | `boolean`       | Tests if the string ends with the specified suffix.                                                                                                                                                                  |

## Manipulation and Transformation Methods
| **Method**                                         | **Return Type** | **Description**                                                                                                                         |
| -------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **`substring(int beginIndex)`**                    | `String`        | Returns a new string that is a substring starting from `beginIndex` to the end of the string.                                           |
| **`substring(int beginIndex, int endIndex)`**      | `String`        | Returns a new string starting from `beginIndex` up to, but not including, `endIndex`.                                                   |
| **`concat(String str)`**                           | `String`        | Appends the specified string to the end of the current string (equivalent to the `+` operator).                                         |
| **`replace(char oldChar, char newChar)`**          | `String`        | Returns a new string replacing **all** occurrences of `oldChar` with `newChar`.                                                         |
| **`replaceAll(String regex, String replacement)`** | `String`        | Replaces **all** substrings that match the given regular expression (`regex`) with the `replacement`.                                   |
| **`split(String regex)`**                          | `String[]`      | Splits the string around matches of the given regular expression and returns the result as an array of strings.                         |
| **`toLowerCase()`**1                               | `String`2       | Returns a new string with all characters converted to lowercase.3                                                                       |
| **`toUpperCase()`**4                               | `String`5       | Returns a new string with all characters converted to uppercase.6                                                                       |
| **`trim()`**7                                      | `String`8       | Returns a new string with9 leading and trailing whitespace removed.                                                                     |
| **`strip()`**                                      | `String`        | Returns a new string with leading and trailing whitespace removed (using Unicode rules, generally better than `trim()` in modern Java). |
| **`toCharArray()`**                                | `char[]`        | Converts the string into a new character array.                                                                                         |

## Static Utility Methods
|**Method**|**Return Type**|**Description**|
|---|---|---|
|**`String.valueOf(dataType arg)`**|`String`|Converts various data types (int, long, char, boolean, object, etc.) to their string representation.|
|**`String.join(CharSequence delimiter, CharSequence... elements)`**|`String`|Concatenates a set of elements using the specified delimiter.|
|**`String.format(String format, Object... args)`**|`String`|Returns a formatted string using the specified format string and arguments (similar to C's `printf`).|