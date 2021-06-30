

# Types

## (Un)Changeable variable types

val - can assign a value once
var - can assign a value and then change it later in the program

## Numberic Types

Integer
- Byte
- Short
- Int
- Long

Floating point
- Double
- Float

Kotlin does not implicitly convert between number types, as that is a common source of errors in  programs. To obtain the value of different type, cast using ``.to`` methods (e.g. ``toByte()``,``toDouble()``).

```kt
val b: Byte = 1
val i: Int = b.toInt()
```

The compiler can usually infer the type for variables, so they do not need to be explicitly declared.

```kt
val oneMillion = 1_000_000 // You can place underscores in long numbers to make them more readable
val socialSecurityNumber = 999_99_9999L // To specify a Long value explicitly, append L (If initial value exceeds the maximum value of an int, then do not need this)
val hexBytes = 0xFF_EC_DE_5E // Hexadecimals 0x0F
val bytes = 0b11010010_01101001_10010100_10010010 // Binaries 0b
val pi = 3.14 // Double
val one: Double = 1 // This will give a mismatch error
val double = 123.5e10 // 1.235e12
val float = 2.12345f // To explicitly specify a Float, use f or F
```

There are also unsigned types like ``UByte, UShort, UInt, ULong``. The character 'u' or 'U' can be used to explicitly specify the type is unsigned.
```kt
val a  = 42u // UInt
val b = 1uL // ULong
```

## Nullability

Types are non nullable by default. Use ``?`` to indicate a variable can be null. This operator can also be used as a check for null.
```kt
var nullableInt: Int? = null
counter = counter?.increment() // same as: if (counter != null) counter.increment()
```
Elvis operator ``?:`` can be used to chain null tests
```kt
counter = counter?.increment() ?: 0 // if counter not null then increment, otherwise use 0
```

double bang (not null assertion) operator ``!!`` - generally bad idea to use unless dealing with legacy Java code
```kt
val len = s!!.length   // throws NullPointerException if s is null
```

## Numbers on the JVM - Boxing 

Numbers are stored as primitive types, unless a nullable number reference is needed or generics are involved. If a nullable reference is needed i.e. ``Int?``, these are boxed in Java Classes like ``Integer``.

(Auto)Boxing is converting a primitive value into an object of the corresponding wrapper class.
Wrapper class is a class whose object wraps/contains a primitive data type (e.g. converting int to Integer).

Nullable references to the same number can be different objects.
```kt
val a: Int = 100
val boxedA: Int? = a
val anotherBoxedA: Int? = a

val b: Int = 10000
val boxedB: Int? = b
val anotherBoxedB: Int? = b

println(boxedA === anotherBoxedA) // true
println(boxedB === anotherBoxedB) // false
```

Explanation: Java specification states that numbers from -128 to 127 are always boxed to the same object. The above example nullable variables in that range are nullable, so they are boxed, meaning those will always point to the same object on JVM.

## Operators

- +, -, *, /, %
- Division of integers yields an integer
```kt
val x = 5 / 2 // 2
val y = 5 / 2.toDouble() // 2.5
```
- Bitwise operators
    - shl(bits) – signed shift left
    - shr(bits) – signed shift right
    - ushr(bits) – unsigned shift right
    - and(bits) – bitwise and
    - or(bits) – bitwise or
    - xor(bits) – bitwise xor
    - inv() – bitwise inversion
- comparison/equality checks <, >, <=, >=, ==, !=
- range ``x !in a..b``

## Booleans

- ``true`` or ``false``
- nullable counterpart ``Boolean?`` also has ``null``
- built in operators include 
    - ``||`` disjunction
    - ``&&`` conjunction
    - ``!`` negatiion

## Strings and Characters

- Use ``"`` for strings, ``'`` for single characters.
- Special characters start with an escaping backslash - ``\t, \b, \n, \r, \', \", \\ and \$``.
For others, use the Unicode e.g. ``'\uFF00'``
- Concatenate strings with the `+` operator, but using string templates is prefereable (Variable interpolation) ``"I have $red books and $blue books, a total of ${red + blue} books"``
- Characters of a string can be accessed by indexing ``s[i]`` or iterating over them ``c in s``
- Strings are immutable. Any operations that transform strings will return the result in a new String.
- regular strings (``"``) can contain escaped characters (e.g "\n")
- raw strings (``"""``) contains no escaping and can contain new lines

## Lists and Arrays

Declare a list using ``listOf`` (cannot be changed) or ``mutableListOf`` (can be changed).

Arrays dont have mutable and immutable versions like lists. Once you create an array, the size is fixed

Array class - get/set functions turn into the ``[]`` operator
```kt
class Array<T> private constructor() {
    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit

    operator fun iterator(): Iterator<T>
    // ...
}
```
Creating arrays:
```kt
val arr1 = arrayOf(1, 2, 3) // Creates [1, 2, 3]
val arr2 = arrayOfNulls(2) // filled with 2 null elements
// Array constructor - takes array size and function that returns value of elements given its index
val asc = Array(5) { i -> (i*i).toString() } // Gives Array<String> with values ["0", "1", "4", "9", "16"]
```

An array declared like the above can have elements with different types. To declare an array with one type, using e.g. ``intArrayOf()``

Arrays are invariant in kotlin (so it does not allow e.g. assigning an ``Array<String>`` to an ``Array<Any>``).

Using primitive type arrays avoids boxing overhead = e.g. IntArray, ByteArray
```kt
val arr = IntArray(5) // [0, 0, 0, 0, 0]
val arr2 = IntArray(5) { 42 } // [42, 42, 42, 42, 42]
val arr3 = arr + arr2 // Combine two arrays with +
var arr4 = IntArray(5) { it * 1 } // [0, 1, 2, 3, 4] (initialised to the index value using a lambda)
```

## Type casts and checks

TODO

## Conditions and loops

``if`` is an express - it returns a value
```kt
val c = if (a > b) a else b // no ternary operator in Kotlin - use this instead
```

``when`` expression defines a conditional expression with multiple branches, similar to a switch statement in other languages.
```kt
when (x) {
    0, 1 -> print("x is 0 or 1")
    !in 2..10 -> print("x not in the range")
    in validNumbers -> print("x is valid")
    else -> print("no result")
}
// Can also be used like an if/else statement when no argument supplied
when {
    x % 2 == 0 -> print("x is even")
    else -> print("x is odd")
}
// 
```

``for`` loop to iterate throgh arrays/lists/ranges
```kt
for (element in arr)
for ((index, element) in arr.withIndex()) // loop through elements and indexes
for (i in 1..4) print(i) // 1234
for (i in 1..4 downTo 1) print(i) // 4321
for (i in 1..4 step 2) print(i) // 13
for (i in 'a'..'d') print(i) // abcd
```

Kotlin also has ``while`` loops, ``do...while`` loops and ``repeat`' loops
```kt
repeat(2) {
    println("test")
}
```

## functions

Declared with the ``fun`` keyword.

Every function in Kotlin returns something, even if nothing is explicitly specified. Functions like ``main()`` or ``println()`` return ``kotlin.Unit`` (no value).

