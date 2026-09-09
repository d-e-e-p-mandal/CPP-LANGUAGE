# 7. Fundamental Data Types in C++

> **Complete study note:** This chapter covers C++ fundamental data types, their signed/unsigned forms, character types, Boolean values, special types, and the important properties of object types such as `sizeof`, `alignof`, ranges, alignment, padding, and object representation.

---

# 7.0 Overview

C++ provides a set of fundamental types that form the basis of almost every program.

The major groups covered here are:

```text
Fundamental Types
│
├── Integer types
│   ├── short
│   ├── int
│   ├── long
│   └── long long
│
├── Signed integer types
│   ├── signed char
│   ├── signed short
│   ├── signed int
│   ├── signed long
│   └── signed long long
│
├── Unsigned integer types
│   ├── unsigned char
│   ├── unsigned short
│   ├── unsigned int
│   ├── unsigned long
│   └── unsigned long long
│
├── Floating-point types
│   ├── float
│   ├── double
│   └── long double
│
├── Character types
│   ├── char
│   ├── signed char
│   ├── unsigned char
│   ├── wchar_t
│   ├── char8_t
│   ├── char16_t
│   └── char32_t
│
├── Boolean
│   └── bool
│
└── Special types
    ├── void
    └── std::nullptr_t
```

---

# 7.0.1 What Is a Fundamental Type?

A fundamental type is a type provided directly by the C++ language rather than a user-defined class, struct, union, or enum.

Examples:

```cpp
int
double
char
bool
void
```

C++ also has compound and user-defined types such as:

```cpp
int*
int&
std::string
std::vector<int>
class Student
struct Point
```

Those are outside the narrow definition of fundamental types.

---

# 7.0.2 Object Types and Non-Object Types

An **object type** is a type that is not a function type, reference type, or `void`.

Examples of object types:

```cpp
int
double
char
bool
int*
Student
```

`void` is not an object type.

A reference is also not an object type.

This distinction becomes important when discussing:

```cpp
sizeof
alignof
object representation
storage
```

---

# 7.0.3 Implementation-Defined Sizes

C++ deliberately does not require every implementation to use the same number of bytes for every fundamental type.

For example:

```cpp
sizeof(int)
```

may commonly be `4`, but portable C++ must not assume that universally.

The language specifies relationships and minimum guarantees rather than one universal size for every type.

---

# 7.1 Integer Types

The standard signed integer types are:

```cpp
short
int
long
long long
```

Their signed forms can also be written explicitly:

```cpp
signed short
signed int
signed long
signed long long
```

---

# 7.1.1 `short`

`short` is a signed integer type.

```cpp
short age = 25;
```

Equivalent spelling:

```cpp
signed short age = 25;
```

Portable minimum width:

```text
at least 16 bits
```

Typical modern systems commonly provide:

```text
sizeof(short) == 2
```

but this is not a universal C++ requirement.

---

# 7.1.2 `int`

`int` is the ordinary signed integer type used for many integer calculations.

```cpp
int count = 100;
```

Equivalent:

```cpp
signed int count = 100;
```

Portable minimum width:

```text
at least 16 bits
```

A very common modern implementation has:

```text
sizeof(int) == 4
```

but portable code should not assume this without checking.

---

# 7.1.3 `long`

`long` is a signed integer type with at least as much range as `int`.

```cpp
long population = 1000000L;
```

Important platform difference:

### LP64 systems

Common on Linux/macOS 64-bit:

```text
int   = 32 bits
long  = 64 bits
```

### LLP64 systems

Common on 64-bit Windows:

```text
int   = 32 bits
long  = 32 bits
long long = 64 bits
```

Therefore:

> Do not assume `long` is 64-bit merely because the program is 64-bit.

---

# 7.1.4 `long long`

`long long` is a signed integer type with at least 64 bits.

```cpp
long long population = 9000000000LL;
```

It is commonly 64 bits on modern systems.

Suffix:

```cpp
LL
```

or:

```cpp
ll
```

can be used for integer literals.

Example:

```cpp
long long x = 10000000000LL;
```

---

# 7.1.5 Integer Type Ordering

C++ guarantees the following minimum relationship:

```text
sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)
```

More precisely, the standard specifies increasing minimum ranges/width relationships; implementations may use equal sizes for adjacent types.

A common model is:

```text
short      16 bits
int        32 bits
long       32 or 64 bits
long long  64 bits
```

---

# 7.1.6 Minimum Integer Widths

The minimum standard widths are approximately:

| Type | Minimum width |
|---|---:|
| `short` | 16 bits |
| `int` | 16 bits |
| `long` | 32 bits |
| `long long` | 64 bits |

These are minimum guarantees, not necessarily actual implementation widths.

---

# 7.1.7 Minimum Ranges for Signed Integers

The minimum required ranges are:

| Type | Minimum range |
|---|---|
| `short` | -32,767 to 32,767 |
| `int` | -32,767 to 32,767 |
| `long` | -2,147,483,647 to 2,147,483,647 |
| `long long` | -9,223,372,036,854,775,807 to 9,223,372,036,854,775,807 |

Modern C++ implementations commonly use two's complement representation, and C++20 standardized two's-complement representation for signed integers.

The minimum table above is deliberately based on the standard's minimum range rather than assuming a particular implementation.

---

# 7.1.8 Typical 64-Bit System

A common system has:

```text
short       2 bytes
int         4 bytes
long        8 bytes on LP64
long long   8 bytes
```

On 64-bit Windows, a common layout is:

```text
short       2 bytes
int         4 bytes
long        4 bytes
long long   8 bytes
```

Therefore, use `<cstdint>` fixed-width types when an exact width is required.

```cpp
std::int32_t
std::int64_t
```

---

# 7.1.9 Integer Literal Suffixes

Examples:

```cpp
10
10U
10L
10UL
10LL
10ULL
```

Meaning:

| Literal | Typical intended type |
|---|---|
| `10` | `int` or suitable signed integer type |
| `10U` | unsigned integer type |
| `10L` | long |
| `10UL` | unsigned long |
| `10LL` | long long |
| `10ULL` | unsigned long long |

Exact literal type selection follows C++ literal rules and depends on value and suffix.

---

# 7.2 Signed Types

Signed integer types can represent negative and positive values.

The explicit signed forms are:

```cpp
signed char
signed short
signed int
signed long
signed long long
```

---

# 7.2.1 `signed char`

```cpp
signed char value = -100;
```

`signed char` is an integer type distinct from plain `char`.

It is commonly used when a small signed integer is required.

It is also commonly used when manipulating raw byte values where signed byte semantics are specifically desired, although `unsigned char` or `std::byte` is often more appropriate for raw storage.

---

# 7.2.2 `signed short`

```cpp
signed short x = -1000;
```

Equivalent to:

```cpp
short x = -1000;
```

For `short`, signedness is implicit.

---

# 7.2.3 `signed int`

```cpp
signed int x = -100;
```

Equivalent to:

```cpp
int x = -100;
```

Usually just write:

```cpp
int x = -100;
```

---

# 7.2.4 `signed long`

```cpp
signed long x = -100000L;
```

Equivalent:

```cpp
long x = -100000L;
```

---

# 7.2.5 `signed long long`

```cpp
signed long long x = -10000000000LL;
```

Equivalent:

```cpp
long long x = -10000000000LL;
```

---

# 7.2.6 Signed Integer Representation

C++20 requires signed integer types to use two's-complement representation.

For an `N`-bit signed integer, the commonly relevant range is:

```text
-2^(N-1) through 2^(N-1)-1
```

For a 32-bit signed integer:

```text
-2^31 through 2^31 - 1
```

which is:

```text
-2,147,483,648 through 2,147,483,647
```

---

# 7.2.7 Signed Integer Overflow

Signed integer overflow is not defined as modular wraparound.

Example:

```cpp
int x = std::numeric_limits<int>::max();
++x;
```

This is not a valid way to obtain the minimum `int`.

Signed overflow can result in undefined behavior.

Unsigned integers have different rules.

---

# 7.3 Unsigned Types

Unsigned integer types contain no negative values.

```cpp
unsigned int count = 100;
```

Available forms:

```cpp
unsigned char
unsigned short
unsigned int
unsigned long
unsigned long long
```

---

# 7.3.1 `unsigned char`

```cpp
unsigned char value = 255;
```

It is an integer type.

It is also particularly important for accessing the object representation of objects.

For raw byte-oriented operations, `unsigned char` can inspect the underlying bytes of an object, subject to the applicable language rules.

---

# 7.3.2 `unsigned short`

```cpp
unsigned short count = 60000;
```

It has at least 16 bits.

Its minimum maximum value is:

```text
65,535
```

on an implementation with the minimum 16-bit width.

---

# 7.3.3 `unsigned int`

```cpp
unsigned int count = 4000000000U;
```

It has the same object representation, value representation, and alignment requirements as the corresponding signed integer type, and uses the corresponding number of bits for value representation.

For an unsigned integer type with `N` value bits, the range is:

```text
0 through 2^N - 1
```

---

# 7.3.4 `unsigned long`

```cpp
unsigned long value = 4000000000UL;
```

Its width is implementation-dependent.

Do not assume that it is 64-bit on all 64-bit systems.

---

# 7.3.5 `unsigned long long`

```cpp
unsigned long long value = 18000000000000000000ULL;
```

It has at least 64 bits and is commonly 64 bits.

---

# 7.3.6 Unsigned Range Formula

If an unsigned integer has `N` value bits:

```text
Minimum = 0
Maximum = 2^N - 1
```

For 8 bits:

```text
0 to 255
```

For 16 bits:

```text
0 to 65,535
```

For 32 bits:

```text
0 to 4,294,967,295
```

For 64 bits:

```text
0 to 18,446,744,073,709,551,615
```

---

# 7.3.7 Unsigned Arithmetic Wraparound

Unsigned arithmetic is performed modulo `2^N` for the relevant width.

Example:

```cpp
unsigned int x = 0;
--x;
```

The result wraps to the maximum value representable by `unsigned int`.

For a 32-bit unsigned integer:

```text
0 - 1
→ 4,294,967,295
```

This behavior is defined, unlike signed overflow.

---

# 7.3.8 Signed vs Unsigned

| Property | Signed | Unsigned |
|---|---|---|
| Negative values | Yes | No |
| Minimum | Negative | 0 |
| Maximum | Smaller positive range | Larger positive range |
| Overflow | Not modular | Modular arithmetic |
| Common use | Quantities that can be negative | Non-negative bit/range values |

---

# 7.3.9 Unsigned Does Not Mean "No Bugs"

Example:

```cpp
std::vector<int> values;

for (std::size_t i = values.size() - 1; i >= 0; --i)
{
    // problematic
}
```

`std::size_t` is unsigned, so `i >= 0` is always true.

A better reverse-loop design is needed.

For example:

```cpp
for (std::size_t i = values.size(); i-- > 0; )
{
    // use values[i]
}
```

or use reverse iterators/ranges.

---

# 7.4 Floating-Point Types

C++ provides:

```cpp
float
double
long double
```

These represent floating-point values.

Examples:

```cpp
float f = 3.14f;
double d = 3.14;
long double ld = 3.14L;
```

---

# 7.4.1 `float`

```cpp
float temperature = 36.5f;
```

`float` is commonly a 32-bit IEEE 754 binary floating-point type, although the C++ standard does not require every implementation to use IEEE 754.

A suffix is commonly used:

```cpp
3.14f
```

Without the suffix:

```cpp
3.14
```

the literal has type `double`.

---

# 7.4.2 `double`

```cpp
double price = 99.99;
```

`double` normally provides greater precision and range than `float`.

A typical implementation:

```text
32-bit float
64-bit double
```

But portable C++ should use type properties rather than blindly assuming those exact formats.

---

# 7.4.3 `long double`

```cpp
long double value = 3.141592653589793238L;
```

`long double` has at least as much precision/range as `double`.

Its representation is implementation-defined.

Common possibilities include:

```text
80-bit extended precision
128-bit format
same representation as double
```

Therefore:

> `long double` does not universally mean "128-bit floating point."

---

# 7.4.4 Floating-Point Precision

Floating-point numbers usually cannot represent every decimal fraction exactly.

Example:

```cpp
double x = 0.1;
```

The mathematical value:

```text
0.1
```

often has no exact finite representation in binary floating-point.

Therefore:

```cpp
double a = 0.1;
double b = 0.2;

if (a + b == 0.3)
{
    // Do not blindly depend on this in numerical code.
}
```

For numerical comparisons, an appropriate tolerance strategy may be required.

---

# 7.4.5 Floating-Point Rounding

Floating-point operations can involve rounding.

This can produce:

```text
representation error
+
operation rounding
+
accumulated numerical error
```

Therefore, floating-point arithmetic should not be treated as exact real-number arithmetic.

---

# 7.4.6 Special Floating Values

Depending on the implementation and floating-point model, you may encounter:

```cpp
+0.0
-0.0
infinity
NaN
```

With IEEE 754 implementations:

```cpp
std::numeric_limits<double>::infinity()
std::numeric_limits<double>::quiet_NaN()
```

can provide special values where supported.

---

# 7.4.7 `NaN`

NaN means:

```text
Not a Number
```

Example:

```cpp
double x = std::numeric_limits<double>::quiet_NaN();
```

NaN has special comparison behavior.

For example:

```cpp
x == x
```

is false for a NaN under the usual IEEE behavior.

Use:

```cpp
std::isnan(x)
```

to test for NaN.

---

# 7.4.8 Floating-Point Limits

Useful facilities include:

```cpp
#include <limits>
```

Example:

```cpp
std::numeric_limits<float>::max()
std::numeric_limits<double>::max()
std::numeric_limits<long double>::max()
```

Other useful properties:

```cpp
std::numeric_limits<T>::lowest()
std::numeric_limits<T>::min()
std::numeric_limits<T>::epsilon()
std::numeric_limits<T>::digits
std::numeric_limits<T>::max_digits10
```

Important:

For floating-point types:

```cpp
numeric_limits<T>::min()
```

means the smallest **positive normalized** value, not the most negative value.

Use:

```cpp
numeric_limits<T>::lowest()
```

for the most negative finite value.

---

# 7.5 Character Types

C++ provides several character-related fundamental types:

```cpp
char
signed char
unsigned char
wchar_t
char8_t
char16_t
char32_t
```

They serve different purposes.

---

# 7.5.1 `char`

```cpp
char grade = 'A';
```

`char` is the ordinary character type.

Important facts:

- It is exactly one byte in C++ terms.
- `sizeof(char)` is always `1`.
- A byte is `CHAR_BIT` bits.
- `CHAR_BIT` is commonly `8`.
- Plain `char` may be signed or unsigned depending on the implementation.
- `char`, `signed char`, and `unsigned char` are distinct types.

---

# 7.5.2 `char` Signedness

Do not assume:

```cpp
char == signed char
```

or:

```cpp
char == unsigned char
```

Plain `char` has implementation-defined signedness.

If you require signed integer semantics:

```cpp
signed char x;
```

If you require unsigned integer semantics:

```cpp
unsigned char x;
```

---

# 7.5.3 `signed char`

```cpp
signed char x = -10;
```

This is an integer type, not merely a "signed version of character encoding."

It is distinct from:

```cpp
char
```

---

# 7.5.4 `unsigned char`

```cpp
unsigned char byte = 255;
```

`unsigned char` is useful for:

- byte-oriented data,
- raw object representation access,
- small unsigned integer values.

Modern C++ also provides:

```cpp
std::byte
```

when the intent is specifically raw byte storage rather than arithmetic.

---

# 7.5.5 `wchar_t`

```cpp
wchar_t c = L'A';
```

`wchar_t` is a distinct integer type intended for wide character values.

Its size and encoding are implementation-defined.

Common examples:

```text
Windows:
    wchar_t commonly 16 bits

Linux/macOS:
    wchar_t commonly 32 bits
```

Therefore:

> `wchar_t` does not universally mean UTF-16 or UTF-32.

---

# 7.5.6 `char8_t`

`char8_t` was introduced in C++20.

```cpp
char8_t c = u8'A';
```

It is a distinct type used for UTF-8 code units.

Example:

```cpp
const char8_t* text = u8"Hello";
```

C++20 changed the element type of ordinary UTF-8 string literals to `char8_t`.

---

# 7.5.7 `char16_t`

```cpp
char16_t c = u'A';
```

`char16_t` is a distinct integer type intended for UTF-16 code units.

A UTF-16 sequence can require:

```text
1 code unit
or
2 code units
```

for a Unicode code point.

Therefore, one `char16_t` is not necessarily one complete Unicode code point.

---

# 7.5.8 `char32_t`

```cpp
char32_t c = U'A';
```

`char32_t` is a distinct integer type intended for UTF-32 code units.

UTF-32 represents each Unicode scalar value using one 32-bit code unit.

It is still important to distinguish:

```text
code unit
code point
grapheme cluster
```

because a user-perceived character may consist of multiple Unicode code points.

---

# 7.5.9 Character Literal Prefixes

| Literal | Type/purpose |
|---|---|
| `'A'` | ordinary character literal |
| `u8'A'` | UTF-8 character literal |
| `u'A'` | `char16_t` character literal |
| `U'A'` | `char32_t` character literal |
| `L'A'` | wide character literal |

Examples:

```cpp
char a = 'A';
char8_t b = u8'A';
char16_t c = u'A';
char32_t d = U'A';
wchar_t e = L'A';
```

---

# 7.5.10 Character Encoding vs Character Type

A type does not automatically tell you how text is encoded.

For example:

```cpp
char
```

does not inherently mean UTF-8.

It may hold bytes from many possible encodings.

Similarly:

```cpp
wchar_t
```

does not inherently mean Unicode.

For Unicode-specific text:

```text
UTF-8  → char8_t code units
UTF-16 → char16_t code units
UTF-32 → char32_t code units
```

---

# 7.5.11 Code Unit vs Code Point

These terms are critical.

### Code unit

A storage unit used by an encoding.

Examples:

```text
UTF-8  → 8-bit code units
UTF-16 → 16-bit code units
UTF-32 → 32-bit code units
```

### Code point

A Unicode value representing a position in the Unicode code space.

### Grapheme cluster

A sequence of code points that can form one user-perceived character.

Therefore:

```text
1 user-perceived character
≠ necessarily 1 code point
≠ necessarily 1 code unit
```

---

# 7.5.12 Character Escape Sequences

Examples:

```cpp
'\n'   // newline
'\t'   // tab
'\r'   // carriage return
'\\'   // backslash
'\''   // single quote
'\0'   // null character
```

Hexadecimal:

```cpp
'\x41'
```

Octal:

```cpp
'\101'
```

Universal character names:

```cpp
U'\u0041'
U'\U00000041'
```

---

# 7.5.13 Null Character

The null character is:

```cpp
'\0'
```

Its value is zero.

It is used to terminate traditional C-style strings:

```cpp
char text[] = "Hello";
```

Conceptually:

```text
H e l l o \0
```

Do not confuse:

```cpp
'\0'
```

with:

```cpp
'0'
```

They are different.

```text
'\0' → numeric value zero
'0'  → character digit zero
```

---

# 7.6 Boolean

C++ has the Boolean type:

```cpp
bool
```

and Boolean values:

```cpp
true
false
```

---

# 7.6.1 `bool`

Example:

```cpp
bool isReady = true;
bool isConnected = false;
```

A Boolean represents a logical truth value.

---

# 7.6.2 `true`

```cpp
bool result = true;
```

`true` represents logical truth.

When converted to an integer:

```cpp
true
```

converts to:

```text
1
```

---

# 7.6.3 `false`

```cpp
bool result = false;
```

When converted to an integer:

```text
false → 0
```

---

# 7.6.4 Boolean-to-Integer Conversion

```cpp
int x = true;
```

produces:

```text
x == 1
```

```cpp
int y = false;
```

produces:

```text
y == 0
```

---

# 7.6.5 Integer-to-Bool Conversion

For conversion to `bool`:

```text
0 → false
non-zero → true
```

Example:

```cpp
bool a = 0;   // false
bool b = 42;  // true
bool c = -10; // true
```

---

# 7.6.6 `sizeof(bool)`

The standard does not require `bool` to occupy exactly one byte.

Common implementations use:

```text
sizeof(bool) == 1
```

but the important point is:

> `sizeof(bool)` is implementation-dependent.

---

# 7.6.7 Boolean in Conditions

```cpp
bool isReady = true;

if (isReady)
{
    // execute
}
```

Boolean expressions are central to:

```cpp
if
while
for
&&
||
!
```

---

# 7.6.8 Avoid Comparing Boolean Values Unnecessarily

Prefer:

```cpp
if (isReady)
{
}
```

instead of:

```cpp
if (isReady == true)
{
}
```

Both can work, but the first is usually clearer.

---

# 7.7 Special Types

The two special types in this outline are:

```cpp
void
std::nullptr_t
```

---

# 7.7.1 `void`

`void` means the absence of a value/type in several language contexts.

Example:

```cpp
void printMessage()
{
}
```

The function returns no value.

---

# 7.7.2 Pointer to `void`

A pointer can point to `void`:

```cpp
void* p = nullptr;
```

A `void*` can hold the address of an object of any object type after an appropriate conversion.

Example:

```cpp
int x = 10;

void* p = &x;
```

To access `x` through the pointer, an appropriate typed pointer is required:

```cpp
int* ip = static_cast<int*>(p);
```

`void*` has no element type for pointer arithmetic or direct dereference.

---

# 7.7.3 `void` Is Not a Normal Object Type

This is invalid:

```cpp
void x; // ERROR
```

You cannot create an ordinary object of type `void`.

Likewise:

```cpp
sizeof(void); // ERROR
```

There is no object size for `void`.

---

# 7.7.4 `void` and Functions

Common:

```cpp
void print();
```

means the function returns no value.

Modern C++ also allows:

```cpp
void f();
```

and this is not the same as C's old-style meaning of an unspecified parameter list; in C++, an empty parameter list means the function takes no parameters.

---

# 7.7.5 `std::nullptr_t`

`std::nullptr_t` is the type of:

```cpp
nullptr
```

It is defined in:

```cpp
#include <cstddef>
```

Example:

```cpp
std::nullptr_t p = nullptr;
```

---

# 7.7.6 Why `nullptr` Exists

Before C++11, code commonly used:

```cpp
int* p = 0;
```

or:

```cpp
int* p = NULL;
```

C++11 introduced:

```cpp
nullptr
```

which has a dedicated null pointer type.

Prefer:

```cpp
int* p = nullptr;
```

---

# 7.7.7 `nullptr` and Overloading

Consider:

```cpp
void f(int);
void f(int*);
```

Calling:

```cpp
f(0);
```

selects:

```cpp
f(int)
```

Calling:

```cpp
f(nullptr);
```

selects:

```cpp
f(int*)
```

This is one reason `nullptr` is safer and clearer.

---

# 7.7.8 `std::nullptr_t` Conversion

A value of `std::nullptr_t` can convert to a null pointer value of any pointer type.

Example:

```cpp
std::nullptr_t n = nullptr;

int* p = n;
double* q = n;
```

Both pointers are null.

---

# 7.8 Type Properties

Important type properties include:

```cpp
sizeof
alignof
minimum ranges
maximum ranges
alignment
padding
object representation
```

---

# 7.8.1 `sizeof`

`sizeof` obtains the size in bytes of an object or type.

Example:

```cpp
int x{};

std::cout << sizeof(x);
```

Or:

```cpp
std::cout << sizeof(int);
```

---

# 7.8.2 `sizeof` Returns `std::size_t`

The result type of `sizeof` is:

```cpp
std::size_t
```

Example:

```cpp
std::size_t size = sizeof(int);
```

Include:

```cpp
#include <cstddef>
```

when explicitly using `std::size_t`.

---

# 7.8.3 `sizeof(char)`

C++ guarantees:

```cpp
sizeof(char) == 1
```

This does **not** mean one byte is necessarily 8 bits.

The number of bits in a byte is:

```cpp
CHAR_BIT
```

from:

```cpp
#include <climits>
```

Modern systems almost universally have:

```text
CHAR_BIT == 8
```

---

# 7.8.4 Size vs Bits

Suppose:

```cpp
sizeof(int) == 4
```

This means:

```text
4 bytes
```

If:

```text
CHAR_BIT == 8
```

then:

```text
4 × 8 = 32 bits
```

But portable C++ should not automatically assume `CHAR_BIT == 8`.

---

# 7.8.5 `sizeof` Examples

```cpp
#include <iostream>

int main()
{
    std::cout << sizeof(char) << '\n';
    std::cout << sizeof(short) << '\n';
    std::cout << sizeof(int) << '\n';
    std::cout << sizeof(long) << '\n';
    std::cout << sizeof(long long) << '\n';

    std::cout << sizeof(float) << '\n';
    std::cout << sizeof(double) << '\n';
    std::cout << sizeof(long double) << '\n';

    std::cout << sizeof(bool) << '\n';
}
```

Exact results are implementation-dependent except for required relationships and guarantees such as `sizeof(char) == 1`.

---

# 7.8.6 `sizeof` on Arrays

```cpp
int values[10]{};

std::cout << sizeof(values);
```

If `int` is 4 bytes:

```text
40 bytes
```

The array itself is not automatically converted to a pointer in this `sizeof` expression.

Therefore:

```cpp
sizeof(values)
```

can determine the total array storage.

---

# 7.8.7 `sizeof` Pointer vs Pointee

```cpp
int x{};
int* p = &x;
```

These can have different sizes:

```cpp
sizeof(x)
sizeof(p)
```

`sizeof(p)` gives the size of the pointer, not the size of the pointed-to object.

---

# 7.8.8 `sizeof` Reference

For a reference expression, `sizeof` yields the size of the referenced type.

Example:

```cpp
int x{};
int& r = x;

sizeof(r)
```

has the same result as:

```cpp
sizeof(int)
```

A reference itself does not have a separately observable `sizeof` result as an object.

---

# 7.8.9 `sizeof` Class Objects

Example:

```cpp
struct A
{
    char c;
    int i;
};
```

You might expect:

```text
1 + 4 = 5
```

but:

```cpp
sizeof(A)
```

may be:

```text
8
```

because of alignment and padding.

---

# 7.8.10 `alignof`

`alignof` obtains the required alignment of a type.

Example:

```cpp
std::cout << alignof(int);
```

Syntax:

```cpp
alignof(type)
```

Example:

```cpp
struct S
{
    char c;
    int i;
};

std::cout << alignof(S);
```

---

# 7.8.11 What Is Alignment?

Alignment is a requirement concerning suitable memory addresses for objects of a particular type.

If a type requires alignment of:

```text
4 bytes
```

an object of that type should be placed at an address satisfying the required alignment.

Conceptually:

```text
Address
1000 ✓
1004 ✓
1008 ✓
1012 ✓
```

may be valid for a 4-byte-aligned type.

Addresses such as:

```text
1002
1006
```

would not satisfy a 4-byte alignment requirement.

---

# 7.8.12 Why Alignment Exists

Alignment can improve:

- CPU memory access efficiency,
- instruction generation,
- hardware compatibility,
- atomic access requirements,
- vector/SIMD operations.

The exact reasons depend on the target architecture and ABI.

---

# 7.8.13 Alignment of Struct Members

Consider:

```cpp
struct S
{
    char c;
    int i;
};
```

A possible layout is:

```text
byte 0       c
byte 1       padding
byte 2       padding
byte 3       padding
byte 4       i
byte 5-7     ...
```

Then:

```text
sizeof(S) = 8
```

on a common implementation.

---

# 7.8.14 Padding

**Padding** is unused storage inserted by the implementation to satisfy alignment and object layout requirements.

Example:

```cpp
struct S
{
    char c;
    int i;
};
```

Possible:

```text
c | P | P | P | i | i | i | i
```

where:

```text
P = padding byte
```

---

# 7.8.15 Tail Padding

A class/struct can also have padding at the end.

Example:

```cpp
struct S
{
    int i;
    char c;
};
```

Possible layout:

```text
i i i i c P P P
```

The trailing padding can help ensure that consecutive elements of an array have the correct alignment.

For:

```cpp
S arr[2];
```

the second object must begin at an address suitable for `S`.

---

# 7.8.16 Member Reordering

Compare:

```cpp
struct A
{
    char c;
    int i;
    char d;
};
```

with:

```cpp
struct B
{
    int i;
    char c;
    char d;
};
```

The second may use less padding on a typical implementation.

However:

> Do not reorder members merely based on a guessed ABI. Measure `sizeof`, `alignof`, and offsets for the target when layout matters.

---

# 7.8.17 `alignas`

C++ provides `alignas` to request increased alignment.

Example:

```cpp
struct alignas(16) Data
{
    int x;
};
```

This requests that `Data` have at least the specified alignment, subject to the language rules.

You can also align an object:

```cpp
alignas(64) char buffer[64];
```

---

# 7.8.18 `std::align_val_t`

C++ also provides facilities for over-aligned allocation.

For normal application code, you usually do not need to manipulate `std::align_val_t` directly.

The key concept is:

```text
normal alignment
vs
over-alignment
```

---

# 7.8.19 Object Representation

Every object of an object type is represented by a sequence of bytes.

The **object representation** is the sequence of bytes that makes up the object.

For example:

```cpp
int x = 42;
```

has some implementation-defined object representation in memory.

You can inspect object representation using character types under the language's permitted access rules.

Example:

```cpp
#include <cstddef>
#include <iostream>

int x = 42;

const unsigned char* bytes =
    reinterpret_cast<const unsigned char*>(&x);

for (std::size_t i = 0; i < sizeof(x); ++i)
{
    std::cout << static_cast<unsigned int>(bytes[i]) << ' ';
}
```

The exact byte values and order are implementation-dependent.

---

# 7.8.20 Endianness

The order of bytes in a multi-byte representation is related to endianness.

Common forms:

```text
Little-endian
Big-endian
```

For example, the bytes of a multi-byte integer may be stored least-significant-byte first or most-significant-byte first.

Do not assume a particular endianness in portable C++.

---

# 7.8.21 Value Representation

The **value representation** is the set of bits in the object representation that participate in representing the value.

Padding bits, where permitted, are not part of the value representation.

This distinction matters when discussing:

```text
object representation
value representation
padding bits
```

---

# 7.8.22 Padding Bits

Some types may have padding bits.

Padding bits do not represent part of the value.

For example, a type could occupy:

```text
16 bits of storage
```

while using fewer bits for its value representation.

Do not assume:

```text
sizeof(T) × CHAR_BIT
```

is always exactly the number of value bits.

---

# 7.8.23 `std::numeric_limits`

Use:

```cpp
#include <limits>
```

to query properties.

Examples:

```cpp
std::numeric_limits<int>::min();
std::numeric_limits<int>::max();

std::numeric_limits<unsigned int>::min();
std::numeric_limits<unsigned int>::max();

std::numeric_limits<double>::lowest();
std::numeric_limits<double>::max();
```

For modern portable code, querying properties is better than hard-coding assumptions.

---

# 7.8.24 Integer Limits

Example:

```cpp
#include <iostream>
#include <limits>

int main()
{
    std::cout << std::numeric_limits<int>::lowest() << '\n';
    std::cout << std::numeric_limits<int>::max() << '\n';

    std::cout << std::numeric_limits<unsigned int>::lowest() << '\n';
    std::cout << std::numeric_limits<unsigned int>::max() << '\n';
}
```

For unsigned integers, `lowest()` is zero.

---

# 7.8.25 `std::numeric_limits<T>::digits`

For integer types:

```cpp
std::numeric_limits<int>::digits
```

reports the number of non-sign bits in the value representation.

For unsigned types, all value bits are reported.

Example:

```cpp
std::cout << std::numeric_limits<unsigned int>::digits;
```

A typical 32-bit `unsigned int` reports:

```text
32
```

---

# 7.8.26 Floating-Point `digits`

For floating-point types:

```cpp
std::numeric_limits<double>::digits
```

describes the number of radix/base-`b` digits in the significand precision.

Useful related properties:

```cpp
digits
digits10
max_digits10
radix
min_exponent
max_exponent
```

---

# 7.8.27 `std::numeric_limits<T>::epsilon()`

For floating-point types:

```cpp
std::numeric_limits<double>::epsilon()
```

represents the difference between `1` and the next representable value greater than `1` in the type.

It is often useful in numerical reasoning, but it is **not** a universal "acceptable error" for every calculation.

---

# 7.8.28 Minimum vs Maximum — Important Terminology

For integers:

```cpp
std::numeric_limits<int>::min()
```

is the minimum representable integer.

For floating-point:

```cpp
std::numeric_limits<double>::min()
```

is the smallest positive normalized value.

Therefore:

```cpp
std::numeric_limits<double>::lowest()
```

is used for the most negative finite value.

This is a common interview question.

---

# 7.8.29 Fixed-Width Integer Types

When exact widths matter, use:

```cpp
#include <cstdint>
```

Examples:

```cpp
std::int8_t
std::int16_t
std::int32_t
std::int64_t

std::uint8_t
std::uint16_t
std::uint32_t
std::uint64_t
```

These types are optional: they are provided only when the implementation has a suitable exact-width integer type.

---

# 7.8.30 Fast and Least Integer Types

`<cstdint>` also provides:

```cpp
std::int_least32_t
std::uint_least32_t

std::int_fast32_t
std::uint_fast32_t
```

### `least`

Smallest type with at least the requested width.

### `fast`

Type intended to provide fast operations with at least the requested width.

---

# 7.8.31 Pointer-Sized Integer Types

When provided, `<cstdint>` can also provide:

```cpp
std::intptr_t
std::uintptr_t
```

These are integer types capable of holding a converted pointer value back to a pointer type, subject to the implementation's support.

They should not be confused with general-purpose "integer versions of pointers."

---

# 7.9 Complete Type Comparison

| Type | Category | Signed? | Typical size | Portable minimum |
|---|---|---|---|---|
| `signed char` | integer | Yes | 1 byte | at least 8 bits |
| `unsigned char` | integer | No | 1 byte | at least 8 bits |
| `short` | integer | Yes | 2 bytes | at least 16 bits |
| `unsigned short` | integer | No | 2 bytes | at least 16 bits |
| `int` | integer | Yes | 4 bytes | at least 16 bits |
| `unsigned int` | integer | No | 4 bytes | at least 16 bits |
| `long` | integer | Yes | 4/8 bytes | at least 32 bits |
| `unsigned long` | integer | No | 4/8 bytes | at least 32 bits |
| `long long` | integer | Yes | 8 bytes | at least 64 bits |
| `unsigned long long` | integer | No | 8 bytes | at least 64 bits |
| `float` | floating | N/A | commonly 4 | implementation-defined representation |
| `double` | floating | N/A | commonly 8 | at least as much precision/range as `float` |
| `long double` | floating | N/A | commonly 8/16/other | at least as much precision/range as `double` |
| `char` | character/integer | implementation-defined signedness | 1 byte | at least 8 bits |
| `wchar_t` | character/integer | signedness/range implementation-defined | 2/4 commonly | implementation-defined |
| `char8_t` | character/integer | unsigned integer type | commonly 1 byte | C++20 |
| `char16_t` | character/integer | unsigned integer type | commonly 2 bytes | C++11 |
| `char32_t` | character/integer | unsigned integer type | commonly 4 bytes | C++11 |
| `bool` | Boolean | N/A | commonly 1 byte | implementation-defined |
| `void` | special | N/A | no object size | no object values |
| `std::nullptr_t` | special | N/A | implementation-dependent | type of `nullptr` |

**Important:** "Typical size" is for common implementations, not a portable guarantee.

---

# 7.10 Common 64-Bit Data Model Comparison

A useful real-world comparison:

| Type | LP64 | LLP64 |
|---|---:|---:|
| `char` | 1 byte | 1 byte |
| `short` | 2 bytes | 2 bytes |
| `int` | 4 bytes | 4 bytes |
| `long` | 8 bytes | 4 bytes |
| `long long` | 8 bytes | 8 bytes |
| pointer | 8 bytes commonly | 8 bytes commonly |

LP64 is common on Unix-like 64-bit platforms.

LLP64 is common on 64-bit Windows.

This is one of the most important reasons not to assume:

```cpp
sizeof(long) == sizeof(void*)
```

---

# 7.11 Integer Promotions

Small integer types often undergo **integral promotion** in expressions.

For example:

```cpp
char a = 10;
char b = 20;

auto result = a + b;
```

The operands may be promoted to `int` before addition.

Therefore:

```cpp
decltype(result)
```

is commonly:

```cpp
int
```

not `char`.

---

# 7.11.1 Why Promotions Exist

Promotions simplify arithmetic and allow small integer types to participate naturally in expressions.

Examples of types subject to integral promotion include:

```cpp
bool
char
signed char
unsigned char
char16_t
char32_t
wchar_t
short
```

subject to the precise promotion rules.

---

# 7.11.2 Example

```cpp
unsigned char a = 200;
unsigned char b = 100;

auto result = a + b;
```

The addition is not necessarily performed as 8-bit unsigned arithmetic.

The operands are promoted according to the integral promotion rules.

This is a common source of confusion.

---

# 7.12 Usual Arithmetic Conversions

When different arithmetic types participate in an operation, C++ applies conversion rules to determine a common type.

Example:

```cpp
int i = 10;
double d = 2.5;

auto result = i + d;
```

The integer is converted to a floating-point type and the result is:

```text
double
```

These rules are called the **usual arithmetic conversions**.

---

# 7.13 Implicit Conversions

Fundamental types can convert between one another.

Example:

```cpp
int x = 10;
double d = x;
```

The integer converts to `double`.

Reverse conversion:

```cpp
double d = 10.9;
int x = d;
```

can lose the fractional part.

Prefer explicit conversions when the conversion is important:

```cpp
int x = static_cast<int>(d);
```

---

# 7.14 Narrowing Conversions

Braced initialization helps detect narrowing:

```cpp
int x{3.14}; // ERROR
```

Compare:

```cpp
int x = 3.14; // allowed, fractional part is discarded
```

This is one reason modern C++ often prefers braces for initialization.

---

# 7.15 `char` and Integer Conversion

Characters participate in integer conversions.

```cpp
char c = 'A';

int x = c;
```

`x` receives the integer value corresponding to the character representation.

Do not assume the value of every character beyond the basic execution character set without considering encoding/implementation details.

---

# 7.16 Character Type Size Summary

Typical:

```text
char      → 1 byte
char8_t   → 1 byte
char16_t  → 2 bytes
char32_t  → 4 bytes
wchar_t   → 2 or 4 bytes commonly
```

But portable code should use:

```cpp
sizeof(...)
```

and the relevant type/encoding rules rather than assumptions.

---

# 7.17 Integer Range Summary

For common widths:

| Width | Signed range | Unsigned range |
|---:|---|---|
| 8-bit | -128 to 127 | 0 to 255 |
| 16-bit | -32,768 to 32,767 | 0 to 65,535 |
| 32-bit | -2,147,483,648 to 2,147,483,647 | 0 to 4,294,967,295 |
| 64-bit | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 0 to 18,446,744,073,709,551,615 |

These rows assume the stated bit width and C++20 two's-complement signed integer representation.

---

# 7.18 Querying Types at Runtime/Compile Time

Example:

```cpp
#include <iostream>
#include <limits>

int main()
{
    std::cout << "int bytes: "
              << sizeof(int) << '\n';

    std::cout << "int bits: "
              << std::numeric_limits<unsigned int>::digits << '\n';

    std::cout << "int max: "
              << std::numeric_limits<int>::max() << '\n';

    std::cout << "int min: "
              << std::numeric_limits<int>::lowest() << '\n';
}
```

For alignment:

```cpp
std::cout << alignof(int) << '\n';
```

---

# 7.19 Compile-Time Type Information

C++ provides type traits.

```cpp
#include <type_traits>
```

Examples:

```cpp
std::is_integral_v<int>
std::is_floating_point_v<double>
std::is_signed_v<int>
std::is_unsigned_v<unsigned int>
```

Example:

```cpp
static_assert(std::is_integral_v<int>);
static_assert(std::is_floating_point_v<double>);
```

These are useful when writing generic code.

---

# 7.20 `std::is_same`

You can compare types:

```cpp
#include <type_traits>

static_assert(std::is_same_v<int, signed int>);
```

This is true because:

```cpp
int
```

and:

```cpp
signed int
```

are alternative spellings for the same type.

But:

```cpp
static_assert(!std::is_same_v<char, signed char>);
```

because `char` is a distinct type.

---

# 7.21 Fundamental Types vs Fixed-Width Types

Do not confuse:

```cpp
int
```

with:

```cpp
std::int32_t
```

`int` is a fundamental type.

`std::int32_t` is an optional typedef/type alias supplied by `<cstdint>` when the implementation provides an exact 32-bit signed integer type.

Use:

```cpp
int
```

when the program simply needs an ordinary integer.

Use:

```cpp
std::int32_t
```

when exactly 32 bits are part of the requirement.

---

# 7.22 Choosing the Correct Integer Type

## Use `int`

For ordinary counters and calculations:

```cpp
int count{};
```

---

## Use `std::int64_t`

When an exact 64-bit signed integer is required:

```cpp
std::int64_t id{};
```

---

## Use `unsigned`

When unsigned modular arithmetic or bit-level representation is intentional:

```cpp
unsigned int mask{};
```

Do not automatically use unsigned merely because a value cannot logically be negative.

---

## Use `std::size_t`

For sizes and indices where the API uses it:

```cpp
std::size_t size = container.size();
```

But be aware that it is unsigned.

---

## Use `float`

When lower precision is sufficient and memory/bandwidth/performance considerations favor it:

```cpp
float position{};
```

---

## Use `double`

As the usual default floating-point type:

```cpp
double price{};
```

---

## Use `long double`

When the implementation's extended precision is useful and the platform-specific behavior is acceptable:

```cpp
long double highPrecision{};
```

---

# 7.23 Common Mistakes

## Mistake 1: Assuming `int` is always 32-bit

Wrong assumption:

```text
int is always 4 bytes.
```

It is common, but not a universal language guarantee.

---

## Mistake 2: Assuming `long` is always 64-bit

Wrong:

```text
64-bit OS → long is 64-bit
```

Windows LLP64 commonly has:

```text
long = 32 bits
```

---

## Mistake 3: Assuming `char` is always signed

Wrong:

```cpp
char c = 200;
```

Do not assume whether `c` is signed or unsigned.

Use an explicit type when signedness matters.

---

## Mistake 4: Assuming `wchar_t` means Unicode

`wchar_t` is implementation-defined and does not universally correspond to one Unicode encoding.

---

## Mistake 5: Assuming `long double` is always 128-bit

It is not.

---

## Mistake 6: Assuming floating-point is exact

```cpp
double x = 0.1;
```

does not imply exact binary representation of mathematical 0.1.

---

## Mistake 7: Using unsigned values carelessly

Unsigned values wrap around.

```cpp
unsigned int x = 0;
--x;
```

This produces the maximum value, not `-1`.

---

## Mistake 8: Confusing `'\0'` and `'0'`

```cpp
'\0' // null character
'0'  // digit zero
```

---

## Mistake 9: Assuming `sizeof(T)` equals number of value bits

Padding bits and representation details can matter.

---

## Mistake 10: Assuming structure size equals member-size sum

```cpp
struct S
{
    char c;
    int i;
};
```

Padding can make:

```cpp
sizeof(S)
```

larger than:

```text
sizeof(char) + sizeof(int)
```

---

# 7.24 Best Practices

### 1. Use `sizeof` for size assumptions

```cpp
static_assert(sizeof(int) >= 2);
```

---

### 2. Use `<cstdint>` when exact width matters

```cpp
std::uint32_t flags{};
```

---

### 3. Use `std::numeric_limits`

Instead of hard-coding limits:

```cpp
std::numeric_limits<int>::max()
```

---

### 4. Prefer `nullptr`

```cpp
int* p = nullptr;
```

instead of:

```cpp
int* p = 0;
```

---

### 5. Prefer `double` for ordinary floating-point calculations

Unless there is a reason to use another type.

---

### 6. Use explicit signedness when required

```cpp
signed char
unsigned char
```

rather than relying on plain `char`.

---

### 7. Use brace initialization

```cpp
int x{10};
double d{3.14};
```

This provides useful narrowing checks.

---

# 7.25 Practical Program

```cpp
#include <cstddef>
#include <cstdint>
#include <iostream>
#include <limits>

int main()
{
    short s{10};
    int i{100};
    long l{1000L};
    long long ll{1000000LL};

    unsigned short us{10};
    unsigned int ui{100};
    unsigned long ul{1000UL};
    unsigned long long ull{1000000ULL};

    float f{3.14f};
    double d{3.141592653589793};
    long double ld{3.141592653589793238L};

    char c{'A'};
    wchar_t wc{L'A'};
    char8_t c8{u8'A'};
    char16_t c16{u'A'};
    char32_t c32{U'A'};

    bool flag{true};

    int* pointer{nullptr};

    std::cout << "sizeof(short): " << sizeof(s) << '\n';
    std::cout << "sizeof(int): " << sizeof(i) << '\n';
    std::cout << "sizeof(long): " << sizeof(l) << '\n';
    std::cout << "sizeof(long long): " << sizeof(ll) << '\n';

    std::cout << "sizeof(float): " << sizeof(f) << '\n';
    std::cout << "sizeof(double): " << sizeof(d) << '\n';
    std::cout << "sizeof(long double): " << sizeof(ld) << '\n';

    std::cout << "sizeof(char): " << sizeof(c) << '\n';
    std::cout << "sizeof(wchar_t): " << sizeof(wc) << '\n';
    std::cout << "sizeof(char8_t): " << sizeof(c8) << '\n';
    std::cout << "sizeof(char16_t): " << sizeof(c16) << '\n';
    std::cout << "sizeof(char32_t): " << sizeof(c32) << '\n';

    std::cout << "sizeof(bool): " << sizeof(flag) << '\n';

    std::cout << "alignof(int): " << alignof(int) << '\n';

    std::cout << "int minimum: "
              << std::numeric_limits<int>::lowest() << '\n';

    std::cout << "int maximum: "
              << std::numeric_limits<int>::max() << '\n';

    std::cout << "uint maximum: "
              << std::numeric_limits<unsigned int>::max() << '\n';

    return 0;
}
```

---

# 7.26 Interview Questions

## Basic

1. What are fundamental data types in C++?
2. What is the difference between fundamental and user-defined types?
3. What are the standard integer types?
4. What is the difference between `short`, `int`, `long`, and `long long`?
5. Is `int` always 32-bit?
6. Is `long` always 64-bit?
7. What is the minimum width of `long long`?
8. What is an unsigned integer?
9. What is signed integer overflow?
10. How does unsigned overflow behave?

---

## Floating point

11. What is the difference between `float`, `double`, and `long double`?
12. Is `double` always 64-bit?
13. Is `long double` always 128-bit?
14. Why can `0.1` not be represented exactly in binary floating point?
15. What is NaN?
16. What is floating-point epsilon?
17. What is the difference between `numeric_limits<T>::min()` and `lowest()` for floating-point types?

---

## Character types

18. Is `char` always signed?
19. Is `char` the same type as `signed char`?
20. Is `char` always 8 bits?
21. What is `wchar_t`?
22. Is `wchar_t` always Unicode?
23. What is `char8_t`?
24. What is the difference between `char16_t` and `char32_t`?
25. What is a code unit?
26. What is a code point?
27. What is the difference between `'\0'` and `'0'`?

---

## Boolean and special types

28. What is `bool`?
29. What happens when `true` converts to `int`?
30. What happens when a non-zero integer converts to `bool`?
31. Can an object of type `void` be created?
32. What is `std::nullptr_t`?
33. Why is `nullptr` better than `NULL`?

---

## Type properties

34. What does `sizeof` return?
35. Why is `sizeof(char)` always 1?
36. What is `CHAR_BIT`?
37. What does `alignof` do?
38. What is alignment?
39. What is padding?
40. What is tail padding?
41. What is object representation?
42. What is value representation?
43. What is endianness?
44. Why can `sizeof(struct)` be larger than the sum of member sizes?

---

# 7.27 Interview Answers

## Q1. Is `int` always 32-bit?

No.

C++ guarantees minimum properties, but the exact width is implementation-defined.

32-bit `int` is extremely common.

---

## Q2. Is `long` always 64-bit on a 64-bit computer?

No.

For example:

```text
LP64:
long = 64 bits

LLP64:
long = 32 bits
```

64-bit Windows commonly uses LLP64.

---

## Q3. Is `char` the same as `signed char`?

No.

They are distinct types.

Plain `char` has implementation-defined signedness.

---

## Q4. Why is `sizeof(char) == 1`?

Because C++ defines the size of a byte in terms of `char`, and `sizeof` measures the number of such bytes.

The number of bits per byte is given by:

```cpp
CHAR_BIT
```

---

## Q5. Why can a struct have padding?

Because members may require particular alignment.

Example:

```cpp
struct S
{
    char c;
    int i;
};
```

The implementation may insert padding between `c` and `i`.

---

## Q6. What is the difference between `min()` and `lowest()`?

For integer types:

```cpp
numeric_limits<int>::min()
```

is the most negative value.

For floating-point types:

```cpp
numeric_limits<double>::min()
```

is the smallest positive normalized value.

For the most negative finite floating-point value:

```cpp
numeric_limits<double>::lowest()
```

---

## Q7. Why use `std::int32_t`?

When exactly 32 bits are required.

```cpp
std::int32_t value{};
```

The type is provided only when the implementation has a suitable exact-width signed integer type.

---

## Q8. What is `std::nullptr_t`?

It is the type of the null pointer literal:

```cpp
nullptr
```

Example:

```cpp
std::nullptr_t x = nullptr;
```

---

# 7.28 Quick Revision

```text
short
    signed integer
    minimum 16 bits

int
    ordinary signed integer
    minimum 16 bits
    commonly 32 bits

long
    signed integer
    minimum 32 bits
    32 or 64 bits commonly

long long
    signed integer
    minimum 64 bits

unsigned
    non-negative integer
    modulo arithmetic

float
    floating-point
    commonly 32 bits

double
    floating-point
    commonly 64 bits

long double
    at least as much precision/range as double
    representation implementation-defined

char
    1 byte
    signedness implementation-defined

wchar_t
    wide character type
    size/encoding implementation-defined

char8_t
    UTF-8 code unit type
    C++20

char16_t
    UTF-16 code unit type

char32_t
    UTF-32 code unit type

bool
    true / false

void
    no object values

std::nullptr_t
    type of nullptr

sizeof
    size in bytes

alignof
    required alignment

padding
    unused layout space

object representation
    bytes making up an object
```

---

# 7.29 Final Mental Model

When choosing or analyzing a C++ fundamental type, ask:

```text
1. What kind of value am I storing?
        ↓
2. Integer / floating / character / Boolean?
        ↓
3. Can it be negative?
        ↓
4. Is an exact width required?
        ↓
5. What range is required?
        ↓
6. What precision is required?
        ↓
7. Does encoding matter?
        ↓
8. Does memory layout matter?
        ↓
9. Does alignment matter?
        ↓
10. Should I query the implementation?
```

For example:

```cpp
std::int64_t userId{};
```

means:

```text
integer
↓
signed
↓
exactly 64 bits, if the type is provided
```

Whereas:

```cpp
int userCount{};
```

means:

```text
ordinary implementation-defined-width signed integer
```

And:

```cpp
char8_t byte{};
```

means:

```text
UTF-8 code unit type
```

not:

```text
arbitrary Unicode character
```

---

# 7.30 Final Takeaway

The most important facts to remember are:

1. **Fundamental types are the basic built-in types of C++.**
2. `short`, `int`, `long`, and `long long` have implementation-dependent widths subject to standard minimum guarantees.
3. Signed integers represent negative and positive values.
4. Unsigned integers represent non-negative values and use modular arithmetic.
5. Signed overflow must not be treated as defined wraparound.
6. `float`, `double`, and `long double` have implementation-dependent representations and precision.
7. `char` is exactly one C++ byte, but plain `char` signedness is implementation-defined.
8. `wchar_t` is implementation-defined and is not inherently a Unicode encoding.
9. `char8_t`, `char16_t`, and `char32_t` are distinct character/code-unit types associated with UTF-8, UTF-16, and UTF-32 respectively.
10. `bool` represents logical truth values.
11. `void` cannot be used to create an ordinary object.
12. `std::nullptr_t` is the type of `nullptr`.
13. `sizeof` measures storage in C++ bytes.
14. `alignof` reports type alignment requirements.
15. Padding can make an object larger than the sum of its member sizes.
16. Object representation describes the bytes making up an object.
17. Use `<limits>` to query numeric properties.
18. Use `<cstdint>` when exact integer widths are part of the requirement.
19. Use `sizeof` and `alignof` instead of assuming platform layout.
20. Separate **type**, **value range**, **representation**, **encoding**, **alignment**, and **storage size** concepts.

---

# End of Chapter 7 — Fundamental Data Types
