# 5.3.2 Character Types in C++

C++ provides several character types for representing characters and character-oriented data:

```cpp
char
char8_t
char16_t
char32_t
wchar_t
```

These types are related, but they are **not interchangeable**.

A very important point is:

> A C++ character type describes how a character/code unit is represented and stored. It does not automatically mean that one object always represents one complete human-readable character.

For example, Unicode text can require multiple code units to represent a single user-perceived character.

---

# 1. Overview of Character Types

| Type | Main purpose | Typical literal prefix | Typical size |
|---|---|---|---:|
| `char` | Ordinary/narrow character data and byte-oriented data | `'A'` | 1 byte |
| `char8_t` | UTF-8 code units | `u8'A'`* | 1 byte |
| `char16_t` | UTF-16 code units | `u'A'` | 2 bytes |
| `char32_t` | UTF-32 code units | `U'A'` | 4 bytes |
| `wchar_t` | Wide character type, encoding/platform dependent | `L'A'` | implementation-defined |

> \* Character literal rules involving `char8_t` and UTF-8 character literals have evolved with C++ standards. For ordinary UTF-8 text, `u8"..."` is the important string-literal form.

A common way to remember them:

```text
char
  |
  +-- ordinary/narrow character and byte-oriented data

char8_t
  |
  +-- UTF-8 code units

char16_t
  |
  +-- UTF-16 code units

char32_t
  |
  +-- UTF-32 code units

wchar_t
  |
  +-- wide character type
      encoding depends on implementation/platform
```

---

# 2. `char`

## 2.1 What is `char`?

`char` is the fundamental C++ character type.

Example:

```cpp
char grade = 'A';
```

Here:

```text
char  -> type
grade -> variable name
'A'   -> character literal
```

A `char` object occupies one byte in C++:

```cpp
sizeof(char)
```

is always:

```text
1
```

However, one **byte** is not required to contain exactly 8 bits. The number of bits in a byte is given by `CHAR_BIT` from `<climits>`.

On virtually all modern desktop/server systems:

```text
CHAR_BIT = 8
```

but portable C++ code should not assume this without checking.

---

# 3. `char` and Character Literals

A normal character literal uses single quotes:

```cpp
'A'
'b'
'7'
'!'
'\n'
'\t'
```

Example:

```cpp
char c1 = 'A';
char c2 = '7';
char c3 = '\n';
```

These are character literals.

A string literal uses double quotes:

```cpp
"Hello"
```

Therefore:

```cpp
'A'
```

is a character literal, while:

```cpp
"A"
```

is a string literal.

This distinction is extremely important.

---

## 3.1 Character vs String

```cpp
char c = 'A';
```

stores one `char` object.

But:

```cpp
const char* s = "A";
```

refers to a null-terminated character string containing:

```text
'A'
'\0'
```

Conceptually:

```text
'A'
 |
 v
+-----+
| 'A' |
+-----+
```

while:

```text
"A"
 |
 v
+-----+-----+
| 'A' | \0  |
+-----+-----+
```

The string contains a terminating null character.

---

# 4. Signedness of `char`

One important property of `char` is that its signedness is **implementation-defined**.

These are three distinct types:

```cpp
char
signed char
unsigned char
```

Do not assume:

```cpp
char
```

is always the same as:

```cpp
signed char
```

or:

```cpp
unsigned char
```

---

## 4.1 `signed char`

```cpp
signed char value = -10;
```

A `signed char` can represent negative and positive values within its implementation-defined range.

---

## 4.2 `unsigned char`

```cpp
unsigned char value = 250;
```

`unsigned char` represents non-negative values.

It is also especially important for examining and manipulating the raw object representation of memory.

Example:

```cpp
unsigned char byte = 0xFF;
```

---

## 4.3 Plain `char`

```cpp
char value = 'A';
```

Whether plain `char` behaves as signed or unsigned for values outside the common character range is implementation-defined.

If you specifically need signed arithmetic:

```cpp
signed char
```

If you specifically need unsigned byte values:

```cpp
unsigned char
```

If you need ordinary character text:

```cpp
char
```

---

# 5. `char` and Integer Conversion

A `char` is an integral type.

Therefore, it participates in integer conversions.

Example:

```cpp
char c = 'A';

int value = c;
```

The integer value corresponds to the implementation's value for that character in the execution character set.

Do not blindly assume that every character's numerical value is ASCII on every C++ implementation.

For common systems:

```text
'A' -> 65
'B' -> 66
'a' -> 97
```

but portable C++ should rely on language guarantees rather than assuming a particular encoding everywhere.

---

## 5.1 Example

```cpp
#include <iostream>

int main()
{
    char c = 'A';

    std::cout << c << '\n';
    std::cout << static_cast<int>(c) << '\n';
}
```

On an ASCII-compatible system, this commonly prints:

```text
A
65
```

The conversion:

```cpp
static_cast<int>(c)
```

makes the numeric conversion explicit.

---

# 6. Escape Sequences with `char`

C++ supports escape sequences.

Common examples:

| Escape | Meaning |
|---|---|
| `'\n'` | Newline |
| `'\t'` | Horizontal tab |
| `'\r'` | Carriage return |
| `'\b'` | Backspace |
| `'\f'` | Form feed |
| `'\v'` | Vertical tab |
| `'\a'` | Alert/bell |
| `'\\'` | Backslash |
| `'\''` | Single quote |
| `'\"'` | Double quote |
| `'\0'` | Null character |

Example:

```cpp
char newline = '\n';
char tab = '\t';
char backslash = '\\';
char quote = '\'';
```

---

# 7. `char` for ASCII and UTF-8

`char` is commonly used for:

- ASCII-compatible text
- Narrow strings
- UTF-8 encoded byte sequences
- Raw byte-oriented operations

Example:

```cpp
const char* text = "Hello";
```

For UTF-8 text:

```cpp
const char* text = u8"Hello";
```

The exact type of a UTF-8 string literal has important standard-version details, discussed below.

---

# 8. `char` Does Not Mean "Unicode Character"

A common beginner mistake is:

```cpp
char c = '😀';
```

and assuming that `char` can represent every Unicode character as one object.

That is not a correct mental model.

Unicode has a large set of code points, while `char` is only one-byte-sized.

UTF-8 represents many Unicode characters using multiple bytes.

For example, a Unicode character may require more than one UTF-8 code unit:

```text
Unicode character
      |
      v
UTF-8 encoding
      |
      +--> byte
      +--> byte
      +--> ...
```

Therefore:

```cpp
std::string
```

containing UTF-8 text should not automatically be interpreted as:

> one `char` = one Unicode character.

Instead:

> For UTF-8 text, a `char` commonly represents one UTF-8 code unit/byte of the encoded sequence.

---

# 9. `char8_t`

## 9.1 What is `char8_t`?

`char8_t` is a distinct character type introduced in **C++20** for representing UTF-8 code units.

Example:

```cpp
char8_t c = u8'A';
```

The important purpose of `char8_t` is to make UTF-8 character data type-safe and distinguish it from ordinary `char` data.

---

# 10. Why `char8_t` Was Introduced

Before C++20, UTF-8 string literals had an element type based on `char`.

For example:

```cpp
const char* text = u8"Hello";
```

C++20 introduced `char8_t` so UTF-8 encoded text can have a dedicated type.

For example:

```cpp
const char8_t* text = u8"Hello";
```

This makes the distinction explicit:

```text
char
   -> ordinary/narrow character data

char8_t
   -> UTF-8 code units
```

---

# 11. `char8_t` Size

`char8_t` is an unsigned integer type capable of representing UTF-8 code units.

Its size is:

```cpp
sizeof(char8_t)
```

and is one byte in the C++ object-size sense.

Like `char`, one byte means one unit of `sizeof`, not necessarily exactly eight bits on every theoretical C++ implementation.

On modern systems this is normally:

```text
1 byte = 8 bits
```

---

# 12. `char8_t` Example

```cpp
#include <iostream>

int main()
{
    char8_t c = u8'A';

    std::cout << static_cast<unsigned int>(c) << '\n';
}
```

A conversion is useful because directly streaming `char8_t` is not the same as streaming an ordinary `char`.

---

# 13. `char8_t` and UTF-8 Strings

Example:

```cpp
const char8_t* text = u8"Hello";
```

The string literal is a UTF-8 string literal.

Its elements are `char8_t` in C++20 and later.

For example:

```cpp
auto text = u8"Hello";
```

has a pointer/array type based on `char8_t` rather than `char` in C++20 and later.

---

# 14. `char8_t` Is Not `char`

This is important:

```cpp
char c = 'A';
char8_t u = u8'A';
```

They are different types.

You should not assume that:

```cpp
char*
```

and:

```cpp
char8_t*
```

are interchangeable.

For example, code expecting:

```cpp
const char*
```

does not automatically accept:

```cpp
const char8_t*
```

This type distinction is one of the main reasons `char8_t` is useful.

---

# 15. `char8_t` and UTF-8 Code Units

UTF-8 is a variable-length encoding.

A Unicode code point may be encoded using:

```text
1 byte
2 bytes
3 bytes
4 bytes
```

Therefore:

```cpp
char8_t
```

represents a UTF-8 **code unit**, not necessarily a complete Unicode code point.

This distinction is extremely important.

---

# 16. Code Point vs Code Unit vs Character

These terms are often confused.

## Code point

A Unicode code point is a numerical value identifying an abstract Unicode character.

Example conceptually:

```text
U+0041
```

represents `A`.

---

## Code unit

A code unit is one storage unit of an encoding.

For UTF-8:

```text
code unit = 8 bits / char8_t-sized unit
```

For UTF-16:

```text
code unit = 16 bits / char16_t-sized unit
```

For UTF-32:

```text
code unit = 32 bits / char32_t-sized unit
```

---

## User-perceived character

A user-perceived character, often called a grapheme cluster, may consist of multiple Unicode code points.

Therefore:

```text
1 user-perceived character
        |
        +--> 1 code point
        |
        +--> multiple code points
```

This means none of the simple C++ character types should automatically be interpreted as:

> exactly one screen character.

---

# 17. `char16_t`

## 17.1 What is `char16_t`?

`char16_t` is a distinct character type designed for representing UTF-16 code units.

Example:

```cpp
char16_t c = u'A';
```

The literal:

```cpp
u'A'
```

is a UTF-16 character literal.

---

# 18. `char16_t` Size

`char16_t` is a 16-bit character type.

Therefore:

```cpp
sizeof(char16_t)
```

is typically:

```text
2 bytes
```

and it is specifically designed around a 16-bit code-unit model.

A `char16_t` object represents one UTF-16 code unit.

---

# 19. `char16_t` Example

```cpp
char16_t letter = u'A';
```

Example:

```cpp
#include <iostream>

int main()
{
    char16_t c = u'A';

    std::cout << static_cast<int>(c) << '\n';
}
```

The explicit conversion is useful because standard output streams do not treat `char16_t` like ordinary narrow `char`.

---

# 20. UTF-16 and Variable Length

UTF-16 is not always one code unit per Unicode code point.

Many Unicode code points fit into one 16-bit code unit.

But some require a **surrogate pair** consisting of two `char16_t` code units.

Conceptually:

```text
Unicode code point
      |
      v
UTF-16
      |
      +--> code unit
      +--> code unit
```

Therefore:

> `char16_t` represents a UTF-16 code unit, not necessarily one complete Unicode code point.

---

# 21. UTF-16 String Literal

A UTF-16 string literal uses the `u` prefix:

```cpp
const char16_t* text = u"Hello";
```

Example:

```cpp
auto text = u"Hello";
```

The literal is an array of `char16_t` code units followed by a null code unit.

---

# 22. UTF-16 Example with Non-BMP Characters

Some Unicode code points are outside the Basic Multilingual Plane.

UTF-16 represents those using two 16-bit code units:

```text
high surrogate
low surrogate
```

Therefore:

```cpp
char16_t
```

cannot by itself represent every Unicode code point as one object.

This is one reason string processing must distinguish:

```text
code units
code points
grapheme clusters
```

---

# 23. `char32_t`

## 23.1 What is `char32_t`?

`char32_t` is a distinct character type designed for UTF-32 code units.

Example:

```cpp
char32_t c = U'A';
```

The literal:

```cpp
U'A'
```

is a UTF-32 character literal.

---

# 24. `char32_t` Size

`char32_t` is a 32-bit character type.

Typically:

```cpp
sizeof(char32_t)
```

is:

```text
4 bytes
```

It is large enough to represent a Unicode code point value directly within the Unicode scalar-value range.

---

# 25. `char32_t` Example

```cpp
char32_t letter = U'A';
```

Example:

```cpp
#include <iostream>

int main()
{
    char32_t c = U'A';

    std::cout << static_cast<std::uint32_t>(c) << '\n';
}
```

If using `std::uint32_t`, include:

```cpp
#include <cstdint>
```

For a simple example, an `unsigned int` conversion may also be used.

---

# 26. UTF-32 String Literal

A UTF-32 string literal uses the `U` prefix:

```cpp
const char32_t* text = U"Hello";
```

Example:

```cpp
auto text = U"Hello";
```

The string is represented as an array of `char32_t` code units followed by a null code unit.

---

# 27. Advantage of `char32_t`

UTF-32 uses one 32-bit code unit for each Unicode code point that can be represented as a Unicode scalar value.

This makes code-point-oriented indexing conceptually simpler than UTF-8 or UTF-16.

For example:

```text
UTF-8:
code point -> 1 to 4 code units

UTF-16:
code point -> 1 or 2 code units

UTF-32:
code point -> 1 code unit
```

However, even UTF-32 does **not** make:

```text
one code unit = one user-perceived character
```

because a grapheme cluster can consist of multiple code points.

---

# 28. `wchar_t`

## 28.1 What is `wchar_t`?

`wchar_t` is a built-in wide character type.

Example:

```cpp
wchar_t c = L'A';
```

The literal:

```cpp
L'A'
```

is a wide character literal.

---

# 29. Important Difference: `wchar_t` Is Implementation-Dependent

Unlike:

```cpp
char8_t
char16_t
char32_t
```

which have specific Unicode-oriented purposes, `wchar_t` is not tied by the C++ language to one particular Unicode encoding.

Its:

- size
- representation
- encoding semantics

are implementation/platform dependent.

Therefore:

> Do not assume `wchar_t` means UTF-16 or UTF-32.

---

# 30. Typical `wchar_t` Platforms

A common situation is:

### Windows

```text
wchar_t -> typically 16 bits
```

and wide strings are commonly associated with UTF-16-style encoding.

### Many Unix-like systems

```text
wchar_t -> typically 32 bits
```

and wide characters are commonly used with a Unicode code-point-oriented representation.

But these are platform conventions, not a universal C++ guarantee.

---

# 31. `wchar_t` Example

```cpp
wchar_t c = L'A';
```

Wide string:

```cpp
const wchar_t* text = L"Hello";
```

Output often uses:

```cpp
std::wcout
```

Example:

```cpp
#include <iostream>

int main()
{
    wchar_t c = L'A';

    std::wcout << c << L'\n';
}
```

---

# 32. `wchar_t` Size

Unlike `char`, `char16_t`, and `char32_t`, the size of `wchar_t` is implementation-defined.

Check it using:

```cpp
sizeof(wchar_t)
```

Example:

```cpp
#include <iostream>

int main()
{
    std::cout << sizeof(wchar_t) << '\n';
}
```

Possible results include:

```text
2
```

or:

```text
4
```

depending on the platform/compiler.

---

# 33. Wide Character Literals

A wide character literal uses:

```cpp
L
```

prefix:

```cpp
L'A'
L'B'
L'\n'
```

A wide string literal uses:

```cpp
L"Hello"
```

Example:

```cpp
wchar_t letter = L'A';
const wchar_t* text = L"Hello";
```

---

# 34. Comparison of All Five Types

## `char`

```cpp
char c = 'A';
```

Main use:

```text
ordinary character / byte-oriented data
```

Encoding:

```text
implementation/context dependent
```

Common UTF-8 usage:

```text
UTF-8 code units can be stored in char
```

---

## `char8_t`

```cpp
char8_t c = u8'A';
```

Main use:

```text
UTF-8 code units
```

Introduced:

```text
C++20
```

---

## `char16_t`

```cpp
char16_t c = u'A';
```

Main use:

```text
UTF-16 code units
```

---

## `char32_t`

```cpp
char32_t c = U'A';
```

Main use:

```text
UTF-32 code units
```

---

## `wchar_t`

```cpp
wchar_t c = L'A';
```

Main use:

```text
wide character representation
```

Encoding:

```text
implementation/platform dependent
```

---

# 35. Character Literal Prefixes

The prefixes are extremely important.

| Prefix | Literal type/purpose | Example |
|---|---|---|
| none | Ordinary character literal | `'A'` |
| `u8` | UTF-8 | `u8'A'` / `u8"..."` |
| `u` | UTF-16 | `u'A'` / `u"..."` |
| `U` | UTF-32 | `U'A'` / `U"..."` |
| `L` | Wide | `L'A'` / `L"..."` |

For string literals:

```cpp
"A"       // ordinary string
u8"A"     // UTF-8 string
u"A"      // UTF-16 string
U"A"      // UTF-32 string
L"A"      // wide string
```

---

# 36. Character Literals vs String Literals

This distinction is essential.

## Character

```cpp
'A'
```

## String

```cpp
"A"
```

Character:

```cpp
char c = 'A';
```

String:

```cpp
const char* s = "A";
```

---

## UTF-16

Character:

```cpp
char16_t c = u'A';
```

String:

```cpp
const char16_t* s = u"A";
```

---

## UTF-32

Character:

```cpp
char32_t c = U'A';
```

String:

```cpp
const char32_t* s = U"A";
```

---

## Wide

Character:

```cpp
wchar_t c = L'A';
```

String:

```cpp
const wchar_t* s = L"A";
```

---

# 37. UTF-8, UTF-16, UTF-32 Comparison

| Encoding | C++ type | Code-unit size | Variable length? |
|---|---|---:|---|
| UTF-8 | `char8_t` | 8 bits | Yes |
| UTF-16 | `char16_t` | 16 bits | Yes |
| UTF-32 | `char32_t` | 32 bits | No, one code unit per Unicode code point |
| Wide representation | `wchar_t` | implementation-defined | Depends on implementation |

For UTF-8:

```text
1 code point -> 1 to 4 code units
```

For UTF-16:

```text
1 code point -> 1 or 2 code units
```

For UTF-32:

```text
1 Unicode scalar value -> 1 code unit
```

---

# 38. Important: Encoding vs Character Type

A character type and a text encoding are related concepts but are not identical.

For example:

```cpp
char
```

does not inherently mean:

```text
UTF-8
```

A `char` object can hold one byte of many kinds of encoded data.

Similarly:

```cpp
wchar_t
```

does not inherently mean:

```text
UTF-16
```

or:

```text
UTF-32
```

The platform and implementation determine its representation and associated wide-character encoding conventions.

---

# 39. `std::string` and Character Types

`std::string` is based on:

```cpp
char
```

Its element type is:

```cpp
char
```

Example:

```cpp
std::string text = "Hello";
```

For UTF-8 text, `std::string` is commonly used to hold the UTF-8 byte sequence:

```cpp
std::string text = u8"Hello";
```

However, in C++20, a `u8` string literal has `char8_t` elements, so directly initializing `std::string` from it is not generally the same as in older standards.

For explicitly typed UTF-8 storage, C++20 provides:

```cpp
std::u8string
```

---

# 40. `std::u8string`

C++20 provides:

```cpp
std::u8string
```

which is an alias for a basic string using `char8_t`.

Example:

```cpp
#include <string>

std::u8string text = u8"Hello";
```

Conceptually:

```text
std::string
    |
    +--> char

std::u8string
    |
    +--> char8_t
```

---

# 41. `std::u16string`

For UTF-16-oriented storage:

```cpp
std::u16string
```

uses:

```cpp
char16_t
```

Example:

```cpp
#include <string>

std::u16string text = u"Hello";
```

---

# 42. `std::u32string`

For UTF-32-oriented storage:

```cpp
std::u32string
```

uses:

```cpp
char32_t
```

Example:

```cpp
#include <string>

std::u32string text = U"Hello";
```

---

# 43. `std::wstring`

For wide strings:

```cpp
std::wstring
```

uses:

```cpp
wchar_t
```

Example:

```cpp
#include <string>

std::wstring text = L"Hello";
```

Remember:

```text
std::wstring
```

inherits the platform-dependent nature of:

```cpp
wchar_t
```

It does not define one universal Unicode encoding.

---

# 44. String Type Summary

| String type | Element type | Typical literal |
|---|---|---|
| `std::string` | `char` | `"Hello"` |
| `std::u8string` | `char8_t` | `u8"Hello"` |
| `std::u16string` | `char16_t` | `u"Hello"` |
| `std::u32string` | `char32_t` | `U"Hello"` |
| `std::wstring` | `wchar_t` | `L"Hello"` |

---

# 45. Null Character

A null character is:

```cpp
'\0'
```

It has value zero in the relevant character representation.

C-style strings use a null character to mark the end of the string.

Example:

```cpp
char text[] = "Hello";
```

Conceptually:

```text
+-----+-----+-----+-----+-----+-----+
| 'H' | 'e' | 'l' | 'l' | 'o' | '\0'|
+-----+-----+-----+-----+-----+-----+
```

The null character is not the same thing as the character:

```cpp
'0'
```

Compare:

```cpp
'\0'   // null character, numeric value zero
'0'    // digit character zero
```

They are different.

---

# 46. Null Character in UTF Strings

UTF-8, UTF-16, and UTF-32 string literals are also terminated by a zero code unit.

For example:

```cpp
u8"Hello"
```

has a terminating zero `char8_t` code unit.

Similarly:

```cpp
u"Hello"
```

has a terminating zero `char16_t` code unit.

And:

```cpp
U"Hello"
```

has a terminating zero `char32_t` code unit.

---

# 47. Character Type and `sizeof`

Example:

```cpp
#include <iostream>

int main()
{
    std::cout << sizeof(char) << '\n';
    std::cout << sizeof(char8_t) << '\n';
    std::cout << sizeof(char16_t) << '\n';
    std::cout << sizeof(char32_t) << '\n';
    std::cout << sizeof(wchar_t) << '\n';
}
```

Typical modern desktop output may look like:

```text
1
1
2
4
4
```

But do not hard-code `wchar_t` size because it is implementation-defined.

Also remember:

```text
sizeof(...)
```

reports a size in units of `sizeof(char)`.

---

# 48. Character Type and `std::is_same`

These types are distinct.

Example:

```cpp
#include <type_traits>

static_assert(!std::is_same_v<char, char8_t>);
static_assert(!std::is_same_v<char, char16_t>);
static_assert(!std::is_same_v<char, char32_t>);
static_assert(!std::is_same_v<char, wchar_t>);
```

This demonstrates that:

```text
char
char8_t
char16_t
char32_t
wchar_t
```

are separate types.

---

# 49. Character Types Are Integral Types

C++ character types participate in the integral type system.

For example:

```cpp
char c = 'A';

int x = c;
```

Likewise:

```cpp
char16_t c16 = u'A';
int x = c16;
```

and:

```cpp
char32_t c32 = U'A';
std::uint32_t x = c32;
```

Appropriate conversions can be used when moving between character and integer types.

---

# 50. Avoid Treating Character Types as Interchangeable

This is a poor approach:

```cpp
char8_t c8 = ...;
char16_t c16 = c8;
char32_t c32 = c16;
```

Although numeric conversions may be possible, this does **not** mean that you have converted text encoding.

A numeric conversion between character types is not the same thing as:

```text
UTF-8 -> UTF-16
```

or:

```text
UTF-16 -> UTF-32
```

Proper encoding conversion requires actual encoding logic.

---

# 51. Encoding Conversion

Suppose you have UTF-8 data:

```text
UTF-8 bytes
```

and need UTF-16:

```text
UTF-16 code units
```

You need an encoding conversion process.

Conceptually:

```text
UTF-8
  |
  | decode
  v
Unicode code points
  |
  | encode
  v
UTF-16
```

Simply doing:

```cpp
char16_t x = static_cast<char16_t>(someChar);
```

does not perform UTF-8 decoding.

This is a critical concept.

---

# 52. Character Type vs Unicode Code Point

Suppose a Unicode code point is:

```text
U+1F600
```

A UTF-8 representation may require four code units.

UTF-16 may require two code units.

UTF-32 can represent the code point using one `char32_t` code unit.

Conceptually:

```text
Unicode code point
       |
       +-------------------+
       |                   |
       v                   v
     UTF-8               UTF-16
  1-4 units             1-2 units
       |
       v
    char8_t
```

and:

```text
Unicode code point
       |
       v
     UTF-32
       |
       v
    char32_t
```

Therefore the storage type tells you about the code-unit representation, not necessarily the number of user-visible characters.

---

# 53. `char8_t` vs `char16_t` vs `char32_t`

## `char8_t`

Use when the data is explicitly UTF-8 code units:

```cpp
std::u8string text = u8"Hello";
```

Advantages:

- Explicit UTF-8 type
- Prevents accidental mixing with ordinary `char` strings
- One code unit corresponds to one byte-sized UTF-8 unit

---

## `char16_t`

Use for UTF-16 code units:

```cpp
std::u16string text = u"Hello";
```

Advantages:

- Explicit UTF-16 representation
- Useful with APIs that require UTF-16
- Fixed 16-bit code-unit size

---

## `char32_t`

Use for UTF-32 code units:

```cpp
std::u32string text = U"Hello";
```

Advantages:

- Fixed 32-bit code-unit size
- One code unit can represent a Unicode scalar value
- Convenient for some code-point-oriented processing

---

# 54. `wchar_t` vs `char16_t` / `char32_t`

Do not replace:

```cpp
wchar_t
```

with:

```cpp
char16_t
```

or:

```cpp
char32_t
```

without considering the API and platform.

`wchar_t` is:

```text
implementation-defined wide character type
```

while:

```cpp
char16_t
```

and:

```cpp
char32_t
```

have defined 16-bit and 32-bit character-type purposes.

---

# 55. Common Character Type Mistakes

## Mistake 1 — Thinking `char` is always ASCII

Wrong assumption:

```text
char = ASCII character
```

Better:

```text
char = ordinary one-byte character type
```

It is commonly used for ASCII and UTF-8 byte sequences, but the type itself does not universally define ASCII.

---

## Mistake 2 — Thinking `char` can hold every Unicode character

Wrong:

```cpp
char c = ...; // assumed to hold any Unicode character
```

Better:

```text
char8_t  -> UTF-8 code unit
char16_t -> UTF-16 code unit
char32_t -> UTF-32 code unit
```

---

## Mistake 3 — Thinking `wchar_t` always means Unicode

Wrong:

```text
wchar_t = Unicode
```

Better:

```text
wchar_t = implementation-defined wide character type
```

---

## Mistake 4 — Thinking `char16_t` always represents one complete character

Wrong:

```text
char16_t = one Unicode character
```

Better:

```text
char16_t = one UTF-16 code unit
```

A Unicode code point may require two UTF-16 code units.

---

## Mistake 5 — Thinking UTF-32 means one visible character

Even if one Unicode scalar value fits in one `char32_t`, one visible grapheme can contain multiple Unicode code points.

Therefore:

```text
char32_t != guaranteed one screen character
```

---

## Mistake 6 — Converting encoding with `static_cast`

Wrong mental model:

```cpp
char16_t x = static_cast<char16_t>(utf8Byte);
```

This does not decode UTF-8.

Encoding conversion is a multi-step operation involving the encoding rules.

---

## Mistake 7 — Confusing `'0'` and `'\0'`

```cpp
'0'   // digit zero
'\0'  // null character
```

They are different values.

---

# 56. Practical Selection Guide

Use:

### `char`

When you need:

```text
ordinary narrow characters
C-style narrow strings
byte-oriented storage
platform APIs expecting char*
UTF-8 bytes when using char-based APIs
```

Example:

```cpp
std::string name = "Deep";
```

---

### `char8_t`

When you explicitly want:

```text
UTF-8 code units
```

Example:

```cpp
std::u8string name = u8"Deep";
```

---

### `char16_t`

When you need:

```text
UTF-16 code units
```

Example:

```cpp
std::u16string text = u"Hello";
```

---

### `char32_t`

When you need:

```text
UTF-32 code units
code-point-oriented representation
```

Example:

```cpp
std::u32string text = U"Hello";
```

---

### `wchar_t`

When an API specifically requires:

```cpp
wchar_t
```

or:

```cpp
std::wstring
```

and you understand the platform-specific encoding semantics.

Example:

```cpp
std::wstring text = L"Hello";
```

Do not choose `wchar_t` simply because you think it automatically means Unicode.

---

# 57. Complete Example

```cpp
#include <cstdint>
#include <iostream>
#include <string>

int main()
{
    char c = 'A';

    char8_t c8 = u8'A';

    char16_t c16 = u'A';

    char32_t c32 = U'A';

    wchar_t wc = L'A';

    std::cout << "char: "
              << c
              << '\n';

    std::cout << "char numeric value: "
              << static_cast<int>(c)
              << '\n';

    std::cout << "char8_t: "
              << static_cast<unsigned int>(c8)
              << '\n';

    std::cout << "char16_t: "
              << static_cast<unsigned int>(c16)
              << '\n';

    std::cout << "char32_t: "
              << static_cast<std::uint32_t>(c32)
              << '\n';

    std::wcout << L"wchar_t: "
               << wc
               << L'\n';

    std::cout << "sizeof(char): "
              << sizeof(char)
              << '\n';

    std::cout << "sizeof(char8_t): "
              << sizeof(char8_t)
              << '\n';

    std::cout << "sizeof(char16_t): "
              << sizeof(char16_t)
              << '\n';

    std::cout << "sizeof(char32_t): "
              << sizeof(char32_t)
              << '\n';

    std::cout << "sizeof(wchar_t): "
              << sizeof(wchar_t)
              << '\n';
}
```

This example demonstrates:

```text
char
char8_t
char16_t
char32_t
wchar_t
```

and their different literal prefixes.

---

# 58. Character Type Cheat Sheet

```text
char
    Ordinary/narrow character type
    sizeof(char) == 1
    Signedness is implementation-defined
    Commonly used for byte-oriented data and narrow strings

char8_t
    C++20
    UTF-8 code unit
    8-bit-oriented character type
    Used by u8 strings in C++20+

char16_t
    UTF-16 code unit
    16-bit character type
    Used by u strings

char32_t
    UTF-32 code unit
    32-bit character type
    Used by U strings
    Can represent a Unicode scalar value in one code unit

wchar_t
    Wide character type
    Size is implementation-defined
    Encoding/representation is platform dependent
    Used by L character/string literals
```

---

# 59. Literal Prefix Cheat Sheet

```cpp
'A'
```

```text
ordinary character literal
```

---

```cpp
u8'A'
```

```text
UTF-8 character literal syntax
```

---

```cpp
u'A'
```

```text
UTF-16 character literal
```

---

```cpp
U'A'
```

```text
UTF-32 character literal
```

---

```cpp
L'A'
```

```text
wide character literal
```

String equivalents:

```cpp
"Hello"
u8"Hello"
u"Hello"
U"Hello"
L"Hello"
```

---

# 60. Final Comparison

| Feature | `char` | `char8_t` | `char16_t` | `char32_t` | `wchar_t` |
|---|---|---|---|---|---|
| Main purpose | Narrow/byte character | UTF-8 code unit | UTF-16 code unit | UTF-32 code unit | Wide character |
| Distinct type | Yes | Yes | Yes | Yes | Yes |
| Width | 1 byte | 1 byte | 2 bytes | 4 bytes | Implementation-defined |
| Encoding fixed by type? | No | UTF-8 | UTF-16 | UTF-32 | No |
| Introduced | Original C/C++ | C++20 | C++11 | C++11 | Original C/C++ |
| Literal | `'A'` | `u8'A'` | `u'A'` | `U'A'` | `L'A'` |
| String literal | `"A"` | `u8"A"` | `u"A"` | `U"A"` | `L"A"` |
| Corresponding STL string | `std::string` | `std::u8string` | `std::u16string` | `std::u32string` | `std::wstring` |
| Unicode encoding role | Commonly UTF-8 bytes | UTF-8 | UTF-16 | UTF-32 | Platform-dependent |
| One code unit = one code point? | Not generally | No | Not always | Yes for Unicode scalar values | Depends |

---

# 61. Most Important Concepts to Remember

## `char`

```cpp
char c = 'A';
```

Think:

> Ordinary/narrow character or byte-oriented storage.

---

## `char8_t`

```cpp
char8_t c = u8'A';
```

Think:

> UTF-8 code unit.

---

## `char16_t`

```cpp
char16_t c = u'A';
```

Think:

> UTF-16 code unit.

---

## `char32_t`

```cpp
char32_t c = U'A';
```

Think:

> UTF-32 code unit.

---

## `wchar_t`

```cpp
wchar_t c = L'A';
```

Think:

> Platform-dependent wide character type.

---

# 62. One-Line Memory Trick

```text
char    -> ordinary/narrow
char8_t -> UTF-8
char16_t -> UTF-16
char32_t -> UTF-32
wchar_t -> wide/platform-dependent
```

Or:

```text
       Character Types
             |
     +-------+-------+-------+-------+
     |       |       |       |       |
    char   char8   char16  char32  wchar
            |       |       |
           UTF-8   UTF-16  UTF-32
```

---

# 63. Final Takeaway

The five C++ character types should not be memorized only by their sizes. Their **representation and encoding purpose** are more important.

```cpp
char
```

is the ordinary character/byte-oriented type.

```cpp
char8_t
```

is specifically associated with UTF-8 code units.

```cpp
char16_t
```

is specifically associated with UTF-16 code units.

```cpp
char32_t
```

is specifically associated with UTF-32 code units.

```cpp
wchar_t
```

is a wide character type whose representation and encoding are implementation-dependent.

The most important distinction is:

```text
Character type
      ≠
Unicode code point
      ≠
Unicode encoding
      ≠
User-perceived character
```

For Unicode text, always think in terms of:

```text
encoding
   ↓
code units
   ↓
code points
   ↓
grapheme clusters / user-perceived characters
```

That mental model prevents many common C++ Unicode and character-processing bugs.
