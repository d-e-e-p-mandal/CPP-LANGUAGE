# 115. C-Style Strings in C++

C-style strings are character sequences traditionally used in C and also supported by C++ for compatibility with C APIs, low-level programming, legacy code, and system interfaces.

C-style strings are usually represented using:

```cpp
char
char[]
const char*
```

The most important rule is:

> A C-style string is a sequence of characters terminated by the null character `'\0'`.

Example:

```cpp
char name[] = "Akash";
```

Conceptually, memory contains:

```text
+-----+-----+-----+-----+-----+------+
| 'A' | 'k' | 'a' | 's' | 'h' | '\0' |
+-----+-----+-----+-----+-----+------+
```

The `'\0'` is called the **null character** or **null terminator**.

It is not the same as:

```cpp
'0'
```

because:

```text
'\0' -> character value 0
'0'  -> character '0'
```

---

# 115.1 Character vs C-Style String

A single character:

```cpp
char ch = 'A';
```

A C-style string:

```cpp
char text[] = "ABC";
```

Memory:

```text
ch:

+-----+
| 'A' |
+-----+


text:

+-----+-----+-----+------+
| 'A' | 'B' | 'C' | '\0' |
+-----+-----+-----+------+
```

Therefore:

```text
char       -> one character

char[]     -> array of characters

C-string   -> character sequence ending with '\0'
```

---

# 115.2 String Literal

A string literal is written using double quotes:

```cpp
"Hello"
```

A character literal uses single quotes:

```cpp
'H'
```

Example:

```cpp
char ch = 'H';

const char* text = "Hello";
```

The string literal:

```cpp
"Hello"
```

contains:

```text
H e l l o \0
```

Therefore it requires **6 characters of storage** when represented as an array.

---

# 115.3 Creating a C-Style String

## 115.3.1 Character Array with String Literal

The easiest form is:

```cpp
char name[] = "Akash";
```

The compiler automatically allocates space for the characters plus `'\0'`.

Conceptually:

```text
name
 |
 v
+-----+-----+-----+-----+-----+------+
|  A  |  k  |  a  |  s  |  h  | \0   |
+-----+-----+-----+-----+-----+------+
```

---

## 115.3.2 Explicit Character Array

You can write the characters manually:

```cpp
char name[] = {'A', 'k', 'a', 's', 'h', '\0'};
```

This is also a valid C-style string.

The `'\0'` is required for string-based C functions to know where the string ends.

---

## 115.3.3 Character Array Without Null Terminator

This is only a character array, not a valid C-style string:

```cpp
char data[] = {'A', 'k', 'a', 's', 'h'};
```

There is no:

```cpp
'\0'
```

at the end.

Therefore, passing it to functions such as:

```cpp
std::strlen(data);
std::cout << data;
```

is not generally valid because those operations expect a null-terminated character sequence.

---

# 115.4 Size of a C-Style String

Consider:

```cpp
char name[] = "Akash";
```

The visible characters are:

```text
A k a s h
```

There are:

```text
5
```

characters.

But the array contains:

```text
A k a s h \0
```

Therefore:

```cpp
sizeof(name)
```

returns:

```text
6
```

Example:

```cpp
#include <iostream>

int main()
{
    char name[] = "Akash";

    std::cout << sizeof(name);
}
```

Output:

```text
6
```

---

# 115.5 `strlen()` vs `sizeof()`

This is a very important difference.

```cpp
#include <cstring>

char name[] = "Akash";
```

Then:

```cpp
std::strlen(name);
```

returns:

```text
5
```

while:

```cpp
sizeof(name);
```

returns:

```text
6
```

Why?

```text
strlen()
    counts characters before '\0'

sizeof()
    returns the total size of the array in bytes
```

Conceptually:

```text
A k a s h \0
<--- 5 --->

<------ 6 ------>  sizeof
```

---

# 115.6 Accessing Characters

C-style strings are arrays, so characters can be accessed using indexes.

```cpp
char name[] = "Akash";

std::cout << name[0];   // A
std::cout << name[1];   // k
std::cout << name[4];   // h
```

Indexes:

```text
A k a s h \0
0 1 2 3 4  5
```

The null terminator is also stored in the array.

You can access it:

```cpp
std::cout << static_cast<int>(name[5]);
```

Result:

```text
0
```

---

# 115.7 Modifying a C-Style String

A character array initialized from a string literal can be modified:

```cpp
char name[] = "Akash";

name[0] = 'A';
name[1] = 'K';
```

Now:

```text
AKash
```

However, a string literal itself must not be modified.

Do not write:

```cpp
char* p = "Hello";
```

and then:

```cpp
p[0] = 'Y';   // undefined behavior
```

For a string literal, use:

```cpp
const char* p = "Hello";
```

because string literals are not writable.

---

# 115.8 C-Style String and `const char*`

A string literal can be used to initialize a pointer:

```cpp
const char* text = "Hello";
```

Conceptually:

```text
text
 |
 v
H e l l o \0
```

The pointer does not own a dynamically allocated string.

It points to the string literal.

---

## `char[]` vs `const char*`

```cpp
char a[] = "Hello";
```

creates an array containing a copy of the characters.

```cpp
const char* b = "Hello";
```

creates a pointer referring to a string literal.

Important difference:

```cpp
a[0] = 'Y';     // valid

// b[0] = 'Y';  // invalid
```

---

# 115.9 Printing C-Style Strings

Using `std::cout`:

```cpp
char name[] = "Akash";

std::cout << name;
```

`operator<<` for a character pointer treats it as a null-terminated C-style string.

Output:

```text
Akash
```

---

## Printing a Single Character

```cpp
std::cout << name[0];
```

Output:

```text
A
```

The type of:

```cpp
name[0]
```

is `char`.

The type of:

```cpp
name
```

in most expressions is converted to a pointer to its first element.

---

# 115.10 Input into a C-Style String

The extraction operator can read into a character array:

```cpp
char name[50];

std::cin >> name;
```

If the user enters:

```text
Akash
```

the array becomes conceptually:

```text
A k a s h \0
```

---

## Problem with `operator>>`

```cpp
char name[50];

std::cin >> name;
```

It stops reading at whitespace.

Input:

```text
Akash Kumar
```

Only:

```text
Akash
```

is stored.

---

# 115.11 `cin.getline()`

To read a complete line:

```cpp
char name[100];

std::cin.getline(name, 100);
```

Input:

```text
Akash Kumar
```

Now the array contains:

```text
Akash Kumar\0
```

The second argument specifies the maximum number of characters to read, including space reserved for the null terminator.

---

# 115.12 `<cstring>` Functions

The header:

```cpp
#include <cstring>
```

provides many C-style string functions.

Important functions:

```text
strlen
strcpy
strncpy
strcat
strncat
strcmp
strncmp
strchr
strrchr
strstr
```

These functions operate on null-terminated character sequences.

---

# 115.13 `strlen()`

`strlen()` returns the number of characters before `'\0'`.

```cpp
#include <cstring>

char text[] = "Hello";

std::cout << std::strlen(text);
```

Output:

```text
5
```

It does not count the null terminator.

---

## Complexity

`strlen()` must normally scan the string until it finds `'\0'`.

Therefore:

```text
Time complexity: O(n)
```

Calling `strlen()` repeatedly inside a loop can therefore be inefficient.

Example:

```cpp
for (std::size_t i = 0; i < std::strlen(text); ++i)
{
    // strlen() may scan repeatedly
}
```

Better:

```cpp
std::size_t n = std::strlen(text);

for (std::size_t i = 0; i < n; ++i)
{
    // use n
}
```

---

# 115.14 `strcpy()`

Copies a C-style string.

```cpp
#include <cstring>

char source[] = "Hello";
char destination[20];

std::strcpy(destination, source);
```

Now:

```text
destination = "Hello"
```

`strcpy()` also copies the terminating `'\0'`.

---

## Important Safety Issue

This is dangerous:

```cpp
char destination[5];

std::strcpy(destination, "Hello");
```

Why?

```text
"Hello" requires 6 characters:

H e l l o \0
```

but:

```text
destination
```

has only 5 bytes.

This causes a buffer overflow and undefined behavior.

Correct:

```cpp
char destination[6];

std::strcpy(destination, "Hello");
```

or better, use `std::string` in modern C++.

---

# 115.15 `strncpy()`

`strncpy()` copies up to a specified number of characters.

```cpp
char source[] = "Hello";
char destination[20];

std::strncpy(destination, source, sizeof(destination));
```

However, `strncpy()` has subtle behavior and is frequently misunderstood.

If the source length is greater than or equal to the specified count, `strncpy()` may not append a null terminator.

Example:

```cpp
char destination[5];

std::strncpy(destination, "Hello", 5);
```

The result may contain:

```text
H e l l o
```

with no:

```text
'\0'
```

Therefore, blindly replacing `strcpy()` with `strncpy()` does not automatically make code safe.

For modern C++, prefer:

```cpp
std::string
```

or carefully use APIs designed for bounded copying.

---

# 115.16 `strcat()`

`strcat()` appends one C-style string to another.

```cpp
char destination[50] = "Hello ";
char source[] = "World";

std::strcat(destination, source);
```

Result:

```text
Hello World
```

The destination must have enough space for:

```text
existing characters
+
new characters
+
'\0'
```

Otherwise, buffer overflow occurs.

---

# 115.17 `strncat()`

`strncat()` appends up to a specified number of characters.

```cpp
char destination[50] = "Hello ";
char source[] = "World";

std::strncat(destination, source, 5);
```

Result:

```text
Hello World
```

Again, the destination must have enough capacity for the final null-terminated string.

---

# 115.18 `strcmp()`

`strcmp()` compares two C-style strings lexicographically.

```cpp
#include <cstring>

const char* a = "apple";
const char* b = "banana";

int result = std::strcmp(a, b);
```

The result is:

```text
< 0    if a comes before b
  0    if a == b
> 0    if a comes after b
```

Do not depend on the exact negative or positive number.

Only the sign matters.

---

## Correct String Comparison

Do not compare C-string pointers like this:

```cpp
const char* a = "Hello";
const char* b = "Hello";

if (a == b)
{
    // not a general string-content comparison
}
```

`a == b` compares pointer values, not the contents.

Use:

```cpp
std::strcmp(a, b) == 0
```

for C-style string content comparison.

---

# 115.19 `strncmp()`

`strncmp()` compares up to a specified number of characters.

```cpp
const char* a = "Hello";
const char* b = "Help";

int result = std::strncmp(a, b, 3);
```

Only the first three characters are compared:

```text
Hel
Hel
```

Therefore the result is:

```text
0
```

---

# 115.20 `strchr()`

Finds the first occurrence of a character.

```cpp
const char* text = "Hello";

const char* p = std::strchr(text, 'l');
```

`p` points to the first `'l'`.

Conceptually:

```text
H e l l o \0
    ^
    |
    p
```

If the character is not found, it returns:

```cpp
nullptr
```

---

# 115.21 `strrchr()`

Finds the last occurrence of a character.

```cpp
const char* text = "Hello";

const char* p = std::strrchr(text, 'l');
```

It points to the second `'l'`.

---

# 115.22 `strstr()`

Finds the first occurrence of a substring.

```cpp
const char* text = "Hello World";

const char* p = std::strstr(text, "World");
```

`p` points to:

```text
World
```

If not found:

```cpp
p == nullptr
```

---

# 115.23 Manual Traversal

Because a C-style string ends at `'\0'`, it can be traversed manually.

```cpp
char text[] = "Hello";

for (std::size_t i = 0; text[i] != '\0'; ++i)
{
    std::cout << text[i] << '\n';
}
```

Output:

```text
H
e
l
l
o
```

The loop stops when it reaches:

```cpp
'\0'
```

---

# 115.24 Pointer Traversal

A C-string can also be traversed using a pointer.

```cpp
const char* p = "Hello";

while (*p != '\0')
{
    std::cout << *p;
    ++p;
}
```

Output:

```text
Hello
```

This demonstrates how traditional C string processing works.

---

# 115.25 Why the Null Terminator Is Important

Consider:

```cpp
char text[] = {'H', 'i', '\0'};
```

The null terminator tells C-string functions:

```text
"The string ends here."
```

Memory:

```text
+-----+-----+------+
|  H  |  i  | \0   |
+-----+-----+------+
```

Without it:

```cpp
char text[] = {'H', 'i'};
```

a C-string function does not know where the string ends.

It may continue reading memory beyond the array looking for `'\0'`.

That can cause:

- Undefined behavior
- Incorrect output
- Memory access errors
- Security vulnerabilities

---

# 115.26 Buffer Overflow

One of the biggest risks of C-style strings is buffer overflow.

Example:

```cpp
char name[5];

std::strcpy(name, "Akash");
```

Required:

```text
A k a s h \0
```

Required size:

```text
6
```

Available:

```text
5
```

Therefore the destination is too small.

---

## Safe Size

```cpp
char name[6];

std::strcpy(name, "Akash");
```

Now there is enough space.

---

## Better Modern C++ Approach

Instead of:

```cpp
char name[100];

std::strcpy(name, "Akash");
```

prefer:

```cpp
std::string name = "Akash";
```

`std::string` manages its storage automatically.

---

# 115.27 C-Style Strings and `std::string`

Both can represent text.

### C-style string

```cpp
char name[] = "Akash";
```

### C++ string

```cpp
std::string name = "Akash";
```

Comparison:

| Feature | C-style string | `std::string` |
|---|---|---|
| Representation | `char[]` / `char*` | Class |
| Null terminated | Yes, for C-string | `c_str()` representation is null-terminated |
| Automatic memory management | No for raw arrays | Yes |
| Dynamic resizing | Manual | Automatic |
| Bounds checking | No built-in `[]` check | `at()` available |
| Concatenation | `strcat()` / manual | `+`, `+=`, `append()` |
| Comparison | `strcmp()` | `==`, `<=>`, etc. |
| Searching | `<cstring>` functions | `find()`, `rfind()`, etc. |
| Substring | Manual / pointer operations | `substr()` |
| Buffer overflow risk | High if unmanaged | Much lower |
| Legacy C API compatibility | Excellent | Use `c_str()` / `data()` |
| Modern C++ preference | Usually only when needed | Usually preferred |

---

# 115.28 Converting C-Style String to `std::string`

```cpp
const char* text = "Hello";

std::string s(text);
```

Now:

```text
C-string
   |
   v
"Hello"
   |
   v
std::string
```

You can also directly write:

```cpp
std::string s = "Hello";
```

---

# 115.29 Converting `std::string` to C-Style String

Use:

```cpp
std::string s = "Hello";

const char* p = s.c_str();
```

Now `p` points to a null-terminated representation.

Example:

```cpp
void c_api(const char* text)
{
    // C-style API
}

std::string s = "Hello";

c_api(s.c_str());
```

---

## Important Lifetime Rule

Do not keep the pointer and then modify or destroy the string without considering pointer invalidation.

```cpp
std::string s = "Hello";

const char* p = s.c_str();

s += " World";

// p may no longer be valid to use
```

If the string changes its storage, the old pointer can become invalid.

Use the pointer only while its source string remains in an appropriate unchanged state.

---

# 115.30 Character Array vs Pointer

These two are different:

```cpp
char a[] = "Hello";
```

and:

```cpp
const char* b = "Hello";
```

### Array

```text
a
|
v
+---+---+---+---+---+---+
| H | e | l | l | o | \0|
+---+---+---+---+---+---+
```

The array itself owns its elements.

### Pointer

```text
b
|
v
H e l l o \0
```

The pointer merely refers to the string literal.

Therefore:

```cpp
sizeof(a)
```

gives the size of the array:

```text
6
```

while:

```cpp
sizeof(b)
```

gives the size of the pointer, not the string.

---

# 115.31 `sizeof` on a C-String Parameter

Consider:

```cpp
void print(char text[])
{
    std::cout << sizeof(text);
}
```

Inside the function, the parameter is adjusted to a pointer type.

So:

```cpp
sizeof(text)
```

does **not** give the original array size.

It gives the size of the pointer.

Use:

```cpp
std::strlen(text)
```

to determine the C-string length.

Example:

```cpp
void print(const char* text)
{
    std::cout << std::strlen(text);
}
```

---

# 115.32 Passing C-Strings to Functions

Read-only C-string:

```cpp
void print(const char* text)
{
    std::cout << text;
}
```

Usage:

```cpp
print("Hello");

char name[] = "Akash";
print(name);
```

If the function needs to modify the character array:

```cpp
void change(char* text)
{
    text[0] = 'X';
}
```

Usage:

```cpp
char name[] = "Akash";

change(name);
```

Do not pass a string literal to a function requiring writable `char*`.

---

# 115.33 Arrays and Function Parameters

Suppose:

```cpp
char name[100] = "Akash";
```

You can pass it:

```cpp
void print(const char* text)
{
    std::cout << text;
}

print(name);
```

The array decays to a pointer to its first character when passed to the function.

Conceptually:

```text
name
 |
 v
'A' 'k' 'a' 's' 'h' '\0'

        |
        v
     char*
```

---

# 115.34 C-Style String Literals and `const`

Use:

```cpp
const char* text = "Hello";
```

not:

```cpp
char* text = "Hello";
```

Modern C++ does not allow converting a string literal to `char*`.

The literal's characters must not be modified.

Correct:

```cpp
const char* text = "Hello";
```

If writable storage is required:

```cpp
char text[] = "Hello";

text[0] = 'Y';
```

---

# 115.35 Embedded Null Characters

A C++ character array can contain a null character before its end:

```cpp
char text[] = {'A', 'B', '\0', 'C', 'D', '\0'};
```

The array contains:

```text
A B \0 C D \0
```

But C-string functions see only:

```text
AB
```

because they stop at the first `'\0'`.

For example:

```cpp
std::strlen(text);
```

returns:

```text
2
```

This is an important difference between:

```text
character array
```

and:

```text
null-terminated C-string
```

---

# 115.36 Common C-String Functions Quick Reference

| Function | Purpose | Typical Complexity |
|---|---|---:|
| `strlen()` | Get string length | O(n) |
| `strcpy()` | Copy string | O(n) |
| `strncpy()` | Copy up to n characters | O(n) |
| `strcat()` | Append string | O(n + m) |
| `strncat()` | Append up to n characters | O(n + m) |
| `strcmp()` | Compare strings | O(min(n,m)) |
| `strncmp()` | Compare up to n chars | O(n) |
| `strchr()` | Find first character | O(n) |
| `strrchr()` | Find last character | O(n) |
| `strstr()` | Find substring | Depends on implementation/algorithm |

The actual performance can depend on the standard-library implementation and input.

---

# 115.37 Complete Example

```cpp
#include <cstring>
#include <iostream>

int main()
{
    char first[50] = "Hello";
    char second[] = " World";

    // Length
    std::cout << "Length: "
              << std::strlen(first)
              << '\n';

    // Append
    std::strcat(first, second);

    std::cout << "After append: "
              << first
              << '\n';

    // Search
    const char* p = std::strstr(first, "World");

    if (p != nullptr)
    {
        std::cout << "World found\n";
    }

    // Compare
    if (std::strcmp(first, "Hello World") == 0)
    {
        std::cout << "Strings are equal\n";
    }
}
```

Output:

```text
Length: 5
After append: Hello World
World found
Strings are equal
```

---

# 115.38 When Are C-Style Strings Used in C++?

Modern C++ normally prefers:

```cpp
std::string
```

or:

```cpp
std::string_view
```

But C-style strings are still important.

Common situations include:

### 1. C APIs

Many C libraries expect:

```cpp
const char*
```

Example:

```cpp
c_api_function(text.c_str());
```

### 2. Operating-System APIs

Some low-level system interfaces use character buffers and pointers.

### 3. Legacy C/C++ Code

Older projects may use:

```cpp
char[]
char*
strcpy()
strlen()
strcmp()
```

### 4. Embedded/System Programming

Fixed-size character buffers can be useful when memory is tightly controlled.

### 5. Low-Level Programming

Direct access to character buffers can be necessary for certain protocols, binary formats, and hardware interfaces.

---

# 115.39 Best Practices

## Prefer `std::string` for Normal C++ Code

Instead of:

```cpp
char name[100];
```

prefer:

```cpp
std::string name;
```

when dynamic text ownership is required.

---

## Prefer `std::string_view` for Read-Only Views

If a function only needs to inspect text and does not need ownership:

```cpp
void process(std::string_view text)
{
    // read-only
}
```

---

## Use C-Style Strings at API Boundaries

When a C API requires:

```cpp
const char*
```

convert:

```cpp
std::string s = "Hello";

c_api(s.c_str());
```

---

## Always Allocate Enough Space

For:

```cpp
char text[] = "Hello";
```

the required array size is:

```text
5 visible characters + 1 null terminator
= 6
```

---

## Be Careful with `strcpy()` and `strcat()`

These functions do not know the capacity of the destination array.

If the destination is too small, a buffer overflow can occur.

---

# 115.40 C-Style String Memory Model

Consider:

```cpp
char name[] = "Akash";
```

Memory can be visualized as:

```text
Stack
+--------------------------------+
| name                           |
|                                |
| +----+----+----+----+----+--+ |
| | A  | k  | a  | s  | h  |\0| |
| +----+----+----+----+----+--+ |
+--------------------------------+
```

The exact storage location depends on how and where the object is declared.

For:

```cpp
const char* p = "Akash";
```

conceptually:

```text
p
 |
 +----> string literal storage
             |
             v
       +----+----+----+----+----+--+
       | A  | k  | a  | s  | h  |\0|
       +----+----+----+----+----+--+
```

The pointer itself and the string literal storage are separate objects.

---

# 115.41 C-Style String vs Character Buffer

These terms should not be confused.

### Character buffer

```cpp
char buffer[100];
```

This is simply an array of 100 characters.

It may or may not contain a valid C-string.

### C-style string

```cpp
char text[] = "Hello";
```

This is a character sequence terminated by `'\0'`.

Therefore:

```text
Every C-string is stored in character storage,
but not every character buffer is a valid C-string.
```

---

# 115.42 Modern C++ Recommendation

For normal text handling:

```cpp
std::string
```

is generally the preferred choice.

For non-owning read-only text:

```cpp
std::string_view
```

is often appropriate.

For C APIs:

```cpp
std::string::c_str()
```

or:

```cpp
std::string::data()
```

can provide access to the character representation.

Use raw:

```cpp
char[]
char*
```

when you specifically need low-level character-buffer behavior, interoperability, fixed-size storage, or legacy APIs.

---

# Quick Reference

## Create

```cpp
char text[] = "Hello";
```

## Access

```cpp
text[0];
text[1];
```

## Length

```cpp
std::strlen(text);
```

## Copy

```cpp
std::strcpy(destination, source);
```

## Append

```cpp
std::strcat(destination, source);
```

## Compare

```cpp
std::strcmp(a, b);
```

## Find Character

```cpp
std::strchr(text, 'l');
```

## Find Substring

```cpp
std::strstr(text, "World");
```

## Input

```cpp
char text[100];

std::cin >> text;
```

Complete line:

```cpp
std::cin.getline(text, 100);
```

## Convert to `std::string`

```cpp
std::string s(text);
```

## Convert `std::string` to C-string

```cpp
const char* p = s.c_str();
```

---

# Important Rules to Remember

1. A C-style string is a null-terminated character sequence.
2. The null terminator is `'\0'`, not `'0'`.
3. `"Hello"` contains 5 visible characters but requires 6 characters when stored as a C-string.
4. `strlen()` does not count `'\0'`.
5. `sizeof(array)` includes the null terminator when the array is initialized from a string literal.
6. `sizeof(pointer)` gives the pointer size, not the string length.
7. `operator[]` can access individual characters.
8. A character array initialized from a literal can be modified.
9. A string literal must not be modified.
10. Use `const char*` when pointing to a string literal.
11. `strcpy()` copies the terminating `'\0'`.
12. `strcpy()` can overflow the destination buffer if it is too small.
13. `strncpy()` does not always guarantee null termination.
14. `strcat()` requires enough destination space for the complete result.
15. `strcmp()` compares string contents, not pointer addresses.
16. `strchr()` finds a character.
17. `strstr()` finds a substring.
18. C-string functions normally stop at the first `'\0'`.
19. A character buffer without `'\0'` is not automatically a valid C-string.
20. `std::string` is generally preferred for normal modern C++ string ownership.
21. `std::string_view` is useful for non-owning read-only text.
22. C-style strings remain important for C APIs, legacy code, low-level programming, and fixed character buffers.
