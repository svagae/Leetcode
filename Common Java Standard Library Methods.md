

# Common Java Standard Library Methods

## String

| Method                                    | Description                                                                                             |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `length()`                                | Returns the length of this string.                                                                      |
| `charAt(int index)`                       | Returns the `char` value at the specified index.                                                        |
| `substring(int beginIndex)`               | Returns a substring of this string from the specified index to the end                                  |
| `substring(int beginIndex, int endIndex)` | Returns a substring of this string between the specified start (inclusive) and end (exclusive) indices. |
| `contains(CharSequence s)`                | Returns true if this string contains the specified sequence of characters.                              |
| `equals(Object obj)`                      | Compares this string to the specified object; returns true if they have the same content.               |
| `startsWith(String prefix)`               | Tests if this string starts with the specified prefix.                                                  |
| `endsWith(String suffix)`                 | Tests if this string ends with the specified suffix                                                     |
| `indexOf(String str)`                     | Returns the index of the first occurrence of the specified substring, or -1 if not found.               |
| `isEmpty()`                               | Returns true if, and only if, this string’s length is 0.                                                |
| `toLowerCase()`                           | Converts all of the characters in this string to lower case (using the default locale).                 |
| `toUpperCase()`                           | Converts all of the characters in this string to upper case (using the default locale).                 |
| `trim()`                                  | Returns a string whose value is this string with any leading and trailing whitespace removed.           |

## HashMap

| Method                        | Description                                                              |
| ----------------------------- | ------------------------------------------------------------------------ |
| `put(K key, V value)`         | Associates the specified value with the specified key in this map.       |
| `get(Object key)`             | Returns the value to which the specified key is mapped, or null if none. |
| `containsKey(Object key)`     | Returns true if this map contains a mapping for the specified key.       |
| `containsValue(Object value)` | Returns true if this map maps one or more keys to the specified value.   |
| `remove(Object key)`          | Removes the mapping for the specified key from this map, if present.     |
| `size()`                      | Returns the number of key-value mappings in this map.                    |
| `isEmpty()`                   | Returns true if this map contains no key-value mappings.                 |
| `clear()`                     | Removes all of the mappings from this map.                               |
| `keySet()`                    | Returns a Set view of the keys contained in this map.                    |
| `values()`                    | Returns a Collection view of the values contained in this map.           |

## HashSet

| Method                              | Description                                                                        |
| ----------------------------------- | ---------------------------------------------------------------------------------- |
| `add(E element)`                    | Adds the specified element to this set if it is not already present.               |
| `remove(Object element)`            | Removes the specified element from this set, if present.                           |
| `contains(Object element)`          | Returns true if this set contains the specified element.                           |
| `size()`                            | Returns the number of elements in this set.                                        |
| `isEmpty()`                         | Returns true if this set contains no elements.                                     |
| `clear()`                           | Removes all of the elements from this set.                                         |
| `addAll(Collection<? extends E> c)` | Adds all elements in the specified collection to this set.                         |
| `removeAll(Collection<?> c)`        | Removes from this set all elements that are contained in the specified collection. |

## Stack

| Method             | Description                                                                        |
| ------------------ | ---------------------------------------------------------------------------------- |
| `push(E item)`     | Pushes an item onto the top of this stack.                                         |
| `pop()`            | Removes and returns the object at the top of this stack.                           |
| `peek()`           | Looks at the object at the top of this stack without removing it.                  |
| `empty()`          | Tests if this stack is empty.                                                      |
| `search(Object o)` | Returns the 1-based position from the top of this stack where the object is found. |

## Character

| Method                  | Description                                                            |
| ----------------------- | ---------------------------------------------------------------------- |
| `isLetter(char ch)`     | Determines if the specified character is a letter.                     |
| `isDigit(char ch)`      | Determines if the specified character is a digit.                      |
| `isUpperCase(char ch)`  | Determines if the specified character is an uppercase character.       |
| `isLowerCase(char ch)`  | Determines if the specified character is a lowercase character.        |
| `isWhitespace(char ch)` | Determines if the specified character is whitespace according to Java. |
| `toLowerCase(char ch)`  | Converts the character argument to lowercase.                          |
| `toUpperCase(char ch)`  | Converts the character argument to uppercase.                          |

## StringBuilder

| Method                           | Description                                                              |
| -------------------------------- | ------------------------------------------------------------------------ |
| `append(String str)`             | Appends the specified string to this character sequence.                 |
| `insert(int offset, String str)` | Inserts the specified string into this sequence at the given position.   |
| `delete(int start, int end)`     | Removes the characters in the substring (start, end) from this sequence. |
| `deleteCharAt(int index)`        | Removes the character at the specified position.                         |
| `reverse()`                      | Reverses the sequence of characters in this string builder.              |
| `toString()`                     | Returns a string representing the data in this sequence.                 |
| `length()`                       | Returns the length (character count) of this sequence.                   |
| `charAt(int index)`              | Returns the `char` value at the specified index.                         |

## StringBuffer

| Method                           | Description                                                |
| -------------------------------- | ---------------------------------------------------------- |
| `append(String str)`             | Appends the specified string to this character sequence.   |
| `length()`                       | Returns the number of characters in this sequence.         |
| `charAt(int index)`              | Returns the `char` value at the specified index.           |
| `delete(int start, int end)`     | Removes characters in a substring of this sequence.        |
| `deleteCharAt(int index)`        | Removes the character at the specified position.           |
| `insert(int offset, String str)` | Inserts the specified string at the given index position.  |
| `reverse()`                      | Reverses the sequence of characters in this string buffer. |
| `toString()`                     | Converts this character buffer to a `String`.              |

## Arrays

| Method                                | Description                                                                         |
| ------------------------------------- | ----------------------------------------------------------------------------------- |
| `asList(T... a)`                      | Returns a fixed-size list backed by the specified array.                            |
| `sort(T[] a)`                         | Sorts the specified array into ascending order (natural ordering).                  |
| `binarySearch(T[] a, T key)`          | Searches the specified sorted array for the given key using binary search.          |
| `copyOf(T[] original, int newLength)` | Copies the specified array, truncating or padding with default values as necessary. |
| `equals(Object[] a, Object[] b)`      | Returns true if the two specified arrays of objects are equal in content.           |
| `fill(int[] a, int val)`              | Assigns the specified value to each element of the specified array.                 |
| `toString(Object[] a)`                | Returns a string representation of the contents of the array.                       |

## Collections

| Method                                                            | Description                                                                    |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| `addAll(Collection<? super T> c, T... elements)`                  | Adds all of the specified elements to the specified collection.                |
| `sort(List<T> list)`                                              | Sorts the specified list into ascending order (natural ordering).              |
| `reverse(List<?> list)`                                           | Reverses the order of the elements in the specified list.                      |
| `shuffle(List<?> list)`                                           | Randomly permutes the elements in the specified list.                          |
| `binarySearch(List<? extends Comparable<? super T>> list, T key)` | Searches the specified sorted list for the given key using binary search.      |
| `max(Collection<? extends T> coll)`                               | Returns the maximum element of the given collection (natural ordering).        |
| `min(Collection<? extends T> coll)`                               | Returns the minimum element of the given collection (natural ordering).        |
| `fill(List<? super T> list, T obj)`                               | Replaces all of the elements in the specified list with the specified element. |

## Math

| Method                    | Description                                                                                          |
| ------------------------- | ---------------------------------------------------------------------------------------------------- |
| `abs(int a)`              | Returns the absolute value of an `int` value.                                                        |
| `max(int a, int b)`       | Returns the greater of two `int` values.                                                             |
| `min(int a, int b)`       | Returns the smaller of two `int` values.                                                             |
| `pow(double a, double b)` | Returns the value of the first argument raised to the power of the second.                           |
| `sqrt(double a)`          | Returns the correctly rounded positive square root of a `double` value.                              |
| `random()`                | Returns a `double` value with a positive sign, greater than or equal to 0.0 and less than 1.0.       |
| `ceil(double a)`          | Returns the smallest `double` value that is greater than or equal to the argument and is an integer. |
| `floor(double a)`         | Returns the largest `double` value that is less than or equal to the argument and is an integer.     |
| `round(double a)`         | Returns the closest `long` to the argument, with ties rounding up.                                   |

## Objects

| Method                                   | Description                                                                                     |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `equals(Object a, Object b)`             | Returns true if the arguments are equal to each other.                                          |
| `hash(Object... values)`                 | Generates a hash code for a sequence of input values.                                           |
| `requireNonNull(T obj)`                  | Checks that the specified object reference is not null (throws NPE if it is).                   |
| `toString(Object o, String nullDefault)` | Returns `o.toString()` if the first argument is non-null, otherwise returns the default string. |

