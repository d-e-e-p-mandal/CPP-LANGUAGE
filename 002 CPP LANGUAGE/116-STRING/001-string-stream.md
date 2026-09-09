# 116. `std::string`

`std::string` is the standard C++ class used to store and manipulate a sequence of characters.

Header:

```cpp
#include <string>
```

Example:

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string name = "Akash";

    std::cout << name << '\n';
}
```

Unlike a C-style character array, `std::string` manages its own memory and provides many functions for searching, modifying, accessing, and manipulating text.

---

# 116.1 String Construction

There are several ways to create a `std::string`.

## 116.1.1 Default Construction

```cpp
std::string s;
```

Creates an empty string.

```cpp
std::cout << s.size();   // 0
```

---

## 116.1.2 Construction from a String Literal

```cpp
std::string s = "Hello";
```

or:

```cpp
std::string s("Hello");
```

The string owns its character data.

---

## 116.1.3 Copy Construction

```cpp
std::string s1 = "Hello";
std::string s2 = s1;
```

Now both strings contain:

```text
s1 = "Hello"
s2 = "Hello"
```

`std::string` manages its own storage, so modifying `s2` does not normally modify `s1`.

```cpp
s2[0] = 'Y';

std::cout << s1;   // Hello
std::cout << s2;   // Yello
```

---

## 116.1.4 Move Construction

```cpp
std::string s1 = "Hello";

std::string s2 = std::move(s1);
```

Move construction allows the resources of `s1` to be transferred to `s2` instead of copying the characters.

After moving, `s1` is still a valid `std::string`, but its exact contents should not be relied upon.

Include:

```cpp
#include <utility>
```

---

## 116.1.5 Repeated Character Construction

**Syntax:**
```cpp
std::string s(count, character);
```

Example:

```cpp
std::string stars(10, '*');
```

Result:

```text
**********
```

```cpp
std::string s(5, 'A');
```

Result: `AAAAA`

---

## 116.1.6 Construction from Characters

```cpp
std::string s = {'H', 'e', 'l', 'l', 'o'};
```

Result:

```text
Hello
```

---

## 116.1.7 Construction from Another String

```cpp
std::string s1 = "Hello";

std::string s2(s1);
```

You can also construct from a portion:

```cpp
std::string s2(s1, 1, 3);
```

Result:

```text
ell
```

---

## 116.1.8 Construction from a C-String

```cpp
const char* text = "Hello";

std::string s(text);
```

The constructor reads characters from the C-string until `'\0'`.

---

## 116.1.9 Construction with a Specific Length

```cpp
std::string s("HelloWorld", 5);
```

Result:

```text
Hello
```

This is useful when the source contains a known number of characters.

---

# 116.2 Access

Important access functions:

- `operator[]`
- `at()`
- `front()`
- `back()`

---

## 116.2.1 `operator[]`

Accesses a character using its index.

```cpp
std::string s = "Hello";

std::cout << s[0];   // H
std::cout << s[1];   // e
std::cout << s[4];   // o
```

Indexing starts from `0`.

```text
H e l l o
0 1 2 3 4
```

### Important

`operator[]` does **not** perform bounds checking.

```cpp
std::string s = "Hello";

std::cout << s[100];   // invalid access
```

Do not access an index outside: `0 ... size() - 1`

---

## 116.2.2 `at()`

`at()` accesses a character with bounds checking.

```cpp
std::string s = "Hello";

std::cout << s.at(1);   // e
```

If the index is invalid, `at()` throws `std::out_of_range`.

```cpp
try
{
    std::cout << s.at(100);
}
catch (const std::out_of_range& e)
{
    std::cout << "Invalid index";
}
```

Include:

```cpp
#include <stdexcept>
```

### `[]` vs `at()`

| Feature | `[]` | `at()` |
|---|---|---|
| Access | Yes | Yes |
| Bounds checking | No | Yes |
| Invalid index | Undefined behavior | Throws `std::out_of_range` |
| Overhead | Typically lower | Bounds check |

Use `[]` when the index is known to be valid.

Use `at()` when runtime checking is required.

---

## 116.2.3 `front()`

Returns the first character.

```cpp
std::string s = "Hello";

std::cout << s.front();   // H
```

Conceptually:

```cpp
s.front()
```

is equivalent to:

```cpp
s[0]
```

The string must not be empty.

---

## 116.2.4 `back()`

Returns the last character.

```cpp
std::string s = "Hello";

std::cout << s.back();   // o
```

Conceptually:

```cpp
s.back()
```

accesses:

```cpp
s[s.size() - 1]
```

The string must not be empty.

---

# 116.3 Modification

Important modification functions:

- `append`
- `insert`
- `erase`
- `replace`
- `push_back`
- `pop_back`

---

## 116.3.1 `append`

Adds characters to the end of the string.

```cpp
std::string s = "Hello";

s.append(" World");
```

Result:

```text
Hello World
```

You can also append another string:

```cpp
std::string a = "Hello ";
std::string b = "World";

a.append(b);
```

Result:

```text
Hello World
```

---

### Append Using `+=`

For normal concatenation:

```cpp
std::string s = "Hello";

s += " World";
```

This is often simpler than:

```cpp
s.append(" World");
```

You can also append a character:

```cpp
s += '!';
```

---

## 116.3.2 `insert`

- Inserts characters at a specified position.

**Syntax:**
```cpp
s.insert(position, text);
```

```cpp
std::string s = "Helo";

s.insert(2, "l");
```

Result: `Hello`

Example:

```cpp
std::string s = "Hello";

s.insert(5, " World");
```

Result:

```text
Hello World
```

---

## 116.3.3 `erase`

- Removes characters.

**Syntax:**
```cpp
s.erase(position, count);
```

```cpp
std::string s = "Hello World";

s.erase(5, 6);
```

Result: `Hello`

Example:

```cpp
std::string s = "Hello";

s.erase(1, 2);
```

Result:

```text
Ho
```

---

### Erase One Character

```cpp
std::string s = "Hello";

s.erase(1, 1);
```

Result:

```text
Hllo
```

---

## 116.3.4 `replace`

- Replaces a portion of a string.

**Syntax:**

```cpp
s.replace(position, count, replacement);
```

```cpp
std::string s = "Hello World";

s.replace(6, 5, "C++");
```

Result:

```text
Hello C++
```

Meaning:
- position    = 6
- count       = 5
- replacement = "C++"


---

### Replace Multiple Characters

```cpp
std::string s = "I like Java";

s.replace(7, 4, "C++");
```

Result:

```text
I like C++
```

---

## 116.3.5 `push_back`

Adds one character to the end.

```cpp
std::string s = "Hell";

s.push_back('o');
```

Result:

```text
Hello
```

`push_back()` accepts a single character.

```cpp
s.push_back('!');
```

---

## 116.3.6 `pop_back`

Removes the last character.

```cpp
std::string s = "Hello";

s.pop_back();
```

Result:

```text
Hell
```

The string must not be empty before calling `pop_back()`.

Safe pattern:

```cpp
if (!s.empty())
{
    s.pop_back();
}
```

---

# 116.4 Searching

Important searching functions:

- `find`
- `rfind`
- `find_first_of`
- `find_last_of`

Other useful functions include:

```cpp
find_first_not_of()
find_last_not_of()
contains()       // C++23
```

---

## 116.4.1 `find`

Finds the first occurrence of a character or substring.

```cpp
std::string s = "Hello World";

std::size_t pos = s.find("World");
```

Result:

```text
6
```

Indexes:

```text
H e l l o   W o r l d
0 1 2 3 4 5 6 7 8 9 10
```

---

### `find` with a Character

```cpp
std::string s = "Hello";

std::size_t pos = s.find('l');
```

Result:

```text
2
```

It returns the first matching position.

---

### Searching from a Position

```cpp
std::string s = "one two one";

std::size_t pos = s.find("one", 4);
```

The search starts from index `4`.

This finds the second `"one"`.

---

### Checking Whether Something Was Found

If the search fails, `find()` returns:

```cpp
std::string::npos
```

Example:

```cpp
std::string s = "Hello";

if (s.find("World") == std::string::npos)
{
    std::cout << "Not found";
}
```

If found:

```cpp
if (s.find("Hello") != std::string::npos)
{
    std::cout << "Found";
}
```

---

## 116.4.2 `rfind`

`rfind()` searches from the end and returns the position of the last occurrence.

```cpp
std::string s = "one two one";

std::size_t pos = s.rfind("one");
```

Result:

```text
8
```

Example with a character:

```cpp
std::string s = "hello";

auto pos = s.rfind('l');
```

Result:

```text
3
```

---

### `find` vs `rfind`

```text
find()
    searches toward the right
    returns first occurrence

rfind()
    searches from the right
    returns last occurrence
```

Example:

```text
one two one
^^^     ^^^
 |       |
find    rfind
```

---

## 116.4.3 `find_first_of`

Finds the first character that matches **any character from a set**.

```cpp
std::string s = "abc123";

auto pos = s.find_first_of("0123456789");
```

Result:

```text
3
```

Why?

```text
a b c 1 2 3
0 1 2 3 4 5
      ^
      first digit
```

Important:

```cpp
s.find_first_of("0123456789");
```

does NOT search for the complete substring `"0123456789"`.

It searches for the first occurrence of any one of those characters.

---

### Find First Vowel

```cpp
std::string s = "C++ programming";

auto pos = s.find_first_of("aeiou");
```

This returns the position of the first vowel.

---

## 116.4.4 `find_last_of`

Finds the last character that matches any character from a set.

```cpp
std::string s = "abc123x5";

auto pos = s.find_last_of("0123456789");
```

The result is the position of:

```text
5
```

because it is the last digit.

---

### File Extension Example

```cpp
std::string file = "program.cpp";

auto pos = file.find_last_of('.');
```

Result:

```text
7
```

Then:

```cpp
std::string extension = file.substr(pos + 1);
```

Result:

```text
cpp
```

---

# 116.5 Substrings

## 116.5.1 `substr`

`substr()` creates a new `std::string` containing a portion of the original string.

Syntax:

```cpp
s.substr(position, count);
```

Example:

```cpp
std::string s = "Hello World";

std::string result = s.substr(6, 5);
```

Result:

```text
World
```

---

### Starting Position Only

If the count is omitted:

```cpp
std::string result = s.substr(6);
```

Result:

```text
World
```

It returns everything from position `6` to the end.

---

### Extract First N Characters

```cpp
std::string s = "Hello World";

std::string result = s.substr(0, 5);
```

Result:

```text
Hello
```

---

### Extracting a File Extension

```cpp
std::string file = "program.cpp";

auto pos = file.find_last_of('.');

std::string extension = file.substr(pos + 1);
```

Result:

```text
cpp
```

---

### Important

`std::string::substr()` creates an **owning string**.

```cpp
std::string s = "Hello World";

std::string part = s.substr(0, 5);
```

Conceptually:

```text
s
 |
 +----> "Hello World"

part
 |
 +----> "Hello"
```

The result is independent of the original string.

This differs from:

```cpp
std::string_view
```

whose `substr()` creates another non-owning view.

---

# 116.6 Capacity

Important functions:

- `size`
- `length`
- `capacity`
- `reserve`
- `shrink_to_fit`

Also useful:

```cpp
empty()
max_size()
```

---

## 116.6.1 `size`

Returns the number of characters currently stored.

```cpp
std::string s = "Hello";

std::cout << s.size();
```

Output: `5`

For `std::string`, `size()` returns the number of characters, not including any conceptual C-string terminator.

---

## 116.6.2 `length`

`length()` returns the same value as `size()` for `std::string`.

```cpp
std::string s = "Hello";

std::cout << s.length();
```

Output:

```text
5
```

Therefore:

```cpp
s.size() == s.length()
```

is true.

### Which One Should You Use?

Both are correct.

Use:

```cpp
size()
```

when thinking of the string as a container.

Use:

```cpp
length()
```

when thinking specifically about text.

Most C++ programmers commonly use `size()` because it is consistent with standard containers.

---

## 116.6.3 `capacity`

`capacity()` tells you how many characters can currently be stored without requiring the string to reallocate its storage.

Example:

```cpp
std::string s;

std::cout << s.size() << '\n';
std::cout << s.capacity() << '\n';
```

The exact capacity is implementation-dependent.

The important relationship is:

```text
size     = characters currently used
capacity = storage currently available
```

Example conceptually:

```text
size = 5
capacity = 15

[ H ][ e ][ l ][ l ][ o ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  <--------- used -------->
  <---------------- capacity --------------------------->
```

Capacity is always at least as large as size.

---

## 116.6.4 `reserve`

`reserve()` requests storage capacity for at least a specified number of characters.

```cpp
std::string s;

s.reserve(1000);
```

This does **not** make the string contain 1000 characters.

```cpp
std::cout << s.size();   // 0
```

It only requests enough capacity for future growth.

Conceptually:

```text
Before:

size     = 0
capacity = small

After reserve(1000):

size     = 0
capacity >= 1000
```

---

### Why Use `reserve()`?

Suppose a string will grow many times:

```cpp
std::string result;

for (int i = 0; i < 100000; ++i)
{
    result += "abc";
}
```

The string may need to grow its storage repeatedly.

If the approximate final size is known:

```cpp
std::string result;

result.reserve(300000);

for (int i = 0; i < 100000; ++i)
{
    result += "abc";
}
```

This can reduce the number of reallocations.

---

### `reserve()` Does Not Change `size()`

```cpp
std::string s;

s.reserve(100);

std::cout << s.size();      // 0
std::cout << s.capacity();  // at least 100
```

This distinction is very important:

```text
reserve() -> storage
resize()  -> actual string size
```

---

## 116.6.5 `shrink_to_fit`

Requests that the string reduce unused capacity.

```cpp
std::string s;

s.reserve(1000);

s = "Hello";

s.shrink_to_fit();
```

Before:

```text
size     = 5
capacity = potentially much larger
```

After the request:

```text
size     = 5
capacity may become closer to 5
```

### Important

`shrink_to_fit()` is a **non-binding request**.

The implementation is allowed not to reduce the capacity.

Therefore, never write code that depends on:

```cpp
s.capacity() == s.size();
```

after `shrink_to_fit()`.

---

# Important Supporting Function: `empty()`

Although not listed in the requested section, `empty()` is commonly used with capacity/size operations.

```cpp
std::string s;

if (s.empty())
{
    std::cout << "String is empty";
}
```

Equivalent logical check:

```cpp
s.size() == 0
```

But:

```cpp
s.empty()
```

expresses the intention more clearly.

---

# `size`, `capacity`, and `reserve` Together

Consider:

```cpp
std::string s;

s.reserve(100);

s += "Hello";
```

Conceptually:

```text
After reserve(100):

size     = 0
capacity >= 100

After += "Hello":

size     = 5
capacity >= 100
```

So:

```text
capacity = allocated/available storage
size     = currently used characters
```

---

# `reserve()` vs `resize()`

This is one of the most important distinctions.

## `reserve()`

Changes capacity.

```cpp
std::string s;

s.reserve(100);
```

Result conceptually:

```text
size     = 0
capacity >= 100
```

---

## `resize()`

Changes the actual number of characters.

```cpp
std::string s;

s.resize(100);
```

Result:

```text
size = 100
```

The string now contains 100 characters.

For example:

```cpp
std::string s = "Hi";

s.resize(5, 'X');
```

Result:

```text
HiXXX
```

Remember:

```text
reserve -> prepare storage
resize  -> change string length
```

---

# Quick Reference

```cpp
#include <string>

std::string s = "Hello World";
```

## Construction

```cpp
std::string a;
std::string b = "Hello";
std::string c(b);
std::string d(5, 'A');
```

## Access

```cpp
s[0];
s.at(0);
s.front();
s.back();
```

## Modification

```cpp
s.append(" C++");
s.insert(5, "!");
s.erase(5, 1);
s.replace(0, 5, "Hi");
s.push_back('!');
s.pop_back();
```

## Searching

```cpp
s.find("World");
s.rfind("o");
s.find_first_of("aeiou");
s.find_last_of("aeiou");
```

## Substring

```cpp
s.substr(0, 5);
```

## Capacity

```cpp
s.size();
s.length();
s.capacity();
s.reserve(100);
s.shrink_to_fit();
```

---

# Important Rules to Remember

1. `std::string` owns and manages its character storage.
2. String indexes start at `0`.
3. `operator[]` does not perform bounds checking.
4. `at()` performs bounds checking and can throw `std::out_of_range`.
5. `front()` returns the first character.
6. `back()` returns the last character.
7. `append()` adds text to the end.
8. `insert()` adds text at a specified position.
9. `erase()` removes characters.
10. `replace()` replaces a portion of the string.
11. `push_back()` adds one character.
12. `pop_back()` removes one character.
13. `find()` returns the first matching position.
14. `rfind()` returns the last matching position.
15. `find_first_of()` searches for the first character belonging to a set.
16. `find_last_of()` searches for the last character belonging to a set.
17. Search failure is represented by `std::string::npos`.
18. `substr()` creates a new owning `std::string`.
19. `size()` and `length()` return the same value for `std::string`.
20. `capacity()` represents currently available storage capacity.
21. `reserve()` requests capacity without changing the string's size.
22. `shrink_to_fit()` requests reduction of unused capacity but is not guaranteed to do so.
23. `reserve()` and `resize()` are different: `reserve()` changes storage capacity, while `resize()` changes the number of characters.
