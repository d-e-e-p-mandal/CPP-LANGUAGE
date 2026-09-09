# 117. `std::string_view`

`std::string_view` is a lightweight, non-owning view of a sequence of characters.

Header:

```cpp
#include <string_view>
```

`std::string_view` was introduced in **C++17**.

The most important difference is:

```text
std::string
    owns the characters

std::string_view
    only refers to existing characters
```

Conceptually:

```text
std::string
+----------------------+
| object               |
|                      |
| owns character data  |
+----------+-----------+
           |
           v
      "Hello World"


std::string_view
+----------------------+
| pointer ------------ +------> "Hello World"
| length               |
+----------------------+
```

A `string_view` does not create or manage the character storage it refers to.

---

# 117.1 Non-Owning String View

## What Does Non-Owning Mean?

Suppose:

```cpp
std::string s = "Hello World";

std::string_view view = s;
```

The string owns the characters:

```text
s
 |
 +----> H e l l o   W o r l d
```

The view simply points to those characters:

```text
view
 |
 +----------------------+
                        |
                        v
              H e l l o   W o r l d
```

No second character buffer is required just to create the view.

---

## `std::string` vs `std::string_view`

### `std::string`

```cpp
std::string s = "Hello";
```

The string owns its data.

It can manage its memory, grow, shrink, and modify its characters.

```cpp
s[0] = 'Y';
```

---

### `std::string_view`

```cpp
std::string_view v = "Hello";
```

The view does not own the characters.

It is primarily a read-only interface:

```cpp
std::cout << v;
```

You cannot modify the characters through a `std::string_view`.

```cpp
// v[0] = 'Y';   // ERROR
```

---

## Why Use `string_view`?

Consider a function that only needs to read text.

```cpp
void print(const std::string& text)
{
    std::cout << text;
}
```

This works with `std::string`, but `string_view` can provide a more general read-only interface:

```cpp
void print(std::string_view text)
{
    std::cout << text;
}
```

Now the function can accept:

```cpp
print("Hello");

std::string s = "Hello";
print(s);

std::string_view v = "Hello";
print(v);
```

This is useful because the function does not need to own or modify the input text.

---

## Important Property

A `string_view` is generally very small.

Conceptually, it contains:

```text
pointer
length
```

The exact object representation is implementation-defined, but it can be thought of as a pointer plus a size.

Therefore:

```cpp
std::string_view view = s;
```

does not mean:

```text
copy all characters
```

It means approximately:

```text
remember where the characters are
remember how many characters are visible
```

---

# 117.2 Construction

## 117.2.1 Default Construction

```cpp
std::string_view view;
```

Creates an empty view.

```cpp
std::cout << view.size();   // 0
```

---

## 117.2.2 Construction from String Literal

```cpp
std::string_view view = "Hello";
```

String literals have static storage duration, so the characters remain alive for the lifetime of the program.

This is safe:

```cpp
std::string_view view = "Hello";
```

---

## 117.2.3 Construction from `std::string`

```cpp
std::string s = "Hello";

std::string_view view = s;
```

The view refers to the characters owned by `s`.

Important:

```text
s owns data
view observes data
```

If `s` is destroyed, the view becomes invalid.

---

## 117.2.4 Construction from Another `string_view`

```cpp
std::string_view a = "Hello";

std::string_view b = a;
```

Both views refer to the same character sequence.

No character copy is needed.

---

## 117.2.5 Construction from Pointer and Length

```cpp
const char* text = "Hello World";

std::string_view view(text, 5);
```

The view contains:

```text
Hello
```

The length explicitly tells the view how many characters belong to it.

This is important because a `string_view` does not need a null terminator.

---

## 117.2.6 Construction from a Character Array

```cpp
char buffer[] = "Hello";

std::string_view view(buffer);
```

The view refers to the characters in `buffer`.

If `buffer` is modified, the characters observed through the view can change.

```cpp
buffer[0] = 'Y';

std::cout << view;   // Yello
```

The view itself does not own a copy.

---

# 117.3 Access

`std::string_view` provides many access operations similar to `std::string`.

Important operations:

```cpp
operator[]
at()
front()
back()
data()
size()
length()
empty()
```

---

## 117.3.1 `operator[]`

```cpp
std::string_view view = "Hello";

std::cout << view[0];   // H
std::cout << view[1];   // e
```

Indexes start at `0`.

```text
H e l l o
0 1 2 3 4
```

Like `std::string::operator[]`, it does not perform bounds checking.

```cpp
std::string_view view = "Hello";

// view[100];   // invalid access
```

---

## 117.3.2 `at()`

`at()` performs bounds checking.

```cpp
std::string_view view = "Hello";

std::cout << view.at(1);   // e
```

Invalid access throws `std::out_of_range`.

```cpp
try
{
    std::cout << view.at(100);
}
catch (const std::out_of_range&)
{
    std::cout << "Invalid index";
}
```

---

## `[]` vs `at()`

| Feature | `[]` | `at()` |
|---|---|---|
| Access character | Yes | Yes |
| Bounds checking | No | Yes |
| Invalid index | Undefined behavior | Throws |
| Typical overhead | Lower | Bounds check |

---

## 117.3.3 `front()`

Returns the first character.

```cpp
std::string_view view = "Hello";

std::cout << view.front();   // H
```

The view must not be empty.

---

## 117.3.4 `back()`

Returns the last character.

```cpp
std::string_view view = "Hello";

std::cout << view.back();   // o
```

The view must not be empty.

---

## 117.3.5 `data()`

Returns a pointer to the viewed character sequence.

```cpp
std::string_view view = "Hello";

const char* p = view.data();
```

### Very Important

`data()` does **not** guarantee that the viewed sequence is null-terminated.

For example:

```cpp
std::string s = "Hello World";

std::string_view view(s.data(), 5);
```

The view contains:

```text
Hello
```

but the underlying memory contains:

```text
Hello World
```

Therefore:

```cpp
printf("%s", view.data());
```

is not a correct general way to print a `string_view`.

Use:

```cpp
std::cout << view;
```

or create an owning string:

```cpp
std::string copy(view);

printf("%s", copy.c_str());
```

---

## 117.3.6 `size()`

Returns the number of characters in the view.

```cpp
std::string_view view = "Hello";

std::cout << view.size();   // 5
```

---

## 117.3.7 `length()`

For `std::string_view`, `length()` and `size()` return the same value.

```cpp
view.length() == view.size()
```

---

## 117.3.8 `empty()`

Checks whether the view contains zero characters.

```cpp
std::string_view view;

if (view.empty())
{
    std::cout << "Empty";
}
```

---

## 117.3.9 `substr()`

`string_view::substr()` creates another view.

```cpp
std::string_view view = "Hello World";

std::string_view part = view.substr(6, 5);
```

Result:

```text
World
```

Important difference:

```text
std::string::substr()
    -> creates a new std::string

std::string_view::substr()
    -> creates another std::string_view
```

Conceptually:

```text
original:
Hello World
      |
      +---- view

substr:
World
      |
      +---- points into original characters
```

No character copy is required merely to create the substring view.

---

# 117.4 Searching

`std::string_view` provides searching operations similar to `std::string`.

Important functions:

```cpp
find()
rfind()
find_first_of()
find_last_of()
find_first_not_of()
find_last_not_of()
```

C++23 also provides:

```cpp
contains()
```

---

## 117.4.1 `find()`

Finds the first occurrence.

```cpp
std::string_view view = "Hello World";

auto pos = view.find("World");
```

Result:

```text
6
```

If not found:

```cpp
if (view.find("C++") == std::string_view::npos)
{
    std::cout << "Not found";
}
```

---

## 117.4.2 `rfind()`

Finds the last occurrence.

```cpp
std::string_view view = "one two one";

auto pos = view.rfind("one");
```

Result:

```text
8
```

---

## 117.4.3 `find_first_of()`

Finds the first character that belongs to a specified set.

```cpp
std::string_view view = "abc123";

auto pos = view.find_first_of("0123456789");
```

Result:

```text
3
```

It searches for any one character from the supplied set.

---

## 117.4.4 `find_last_of()`

Finds the last character belonging to a specified set.

```cpp
std::string_view view = "abc123x5";

auto pos = view.find_last_of("0123456789");
```

The result points to:

```text
5
```

---

## 117.4.5 `find_first_not_of()`

Finds the first character that is not part of the specified set.

```cpp
std::string_view view = "   Hello";

auto pos = view.find_first_not_of(' ');
```

This finds:

```text
H
```

---

## 117.4.6 `find_last_not_of()`

```cpp
std::string_view view = "Hello   ";

auto pos = view.find_last_not_of(' ');
```

This finds:

```text
o
```

---

## 117.4.7 `contains()` — C++23

```cpp
std::string_view view = "Hello World";

if (view.contains("World"))
{
    std::cout << "Found";
}
```

This is clearer than:

```cpp
if (view.find("World") != std::string_view::npos)
{
    // found
}
```

---

# 117.5 Lifetime

Lifetime is the **most important concept** when using `std::string_view`.

A `string_view` does not own the characters.

Therefore:

> The characters being viewed must remain alive and sufficiently stable for as long as the view is used.

---

## 117.5.1 Safe View of a String Literal

```cpp
std::string_view view = "Hello";
```

Safe because the string literal has static storage duration.

---

## 117.5.2 Safe View While the String Exists

```cpp
std::string s = "Hello";

std::string_view view = s;

std::cout << view;
```

This is safe because `s` is still alive.

---

## 117.5.3 Dangling View from a Local String

Bad:

```cpp
std::string_view getName()
{
    std::string name = "Akash";

    return name;
}
```

What happens?

```text
function starts
    |
    v
name owns "Akash"
    |
    v
return string_view
    |
    v
function ends
    |
    v
name destroyed
    |
    v
string_view points to destroyed data
```

The returned view is dangling.

Correct approach:

```cpp
std::string getName()
{
    return "Akash";
}
```

If ownership must be returned, return `std::string`.

---

## 117.5.4 Temporary String

This is dangerous:

```cpp
std::string_view view = std::string("Hello");
```

The temporary `std::string` is destroyed at the end of the full expression.

After that:

```cpp
view
```

refers to destroyed storage.

Do not store a `string_view` referring to a temporary string.

---

## 117.5.5 String Reallocation

Even if the original string is still alive, its character storage can move.

Example:

```cpp
std::string s = "Hello";

std::string_view view = s;

s += " World";
```

If the append causes reallocation, the old character buffer can be replaced.

The view may then refer to the old invalidated storage.

Therefore, do not assume a view remains valid after operations on the source string that can invalidate references or pointers.

---

## 117.5.6 Source String Modification

Even without reallocation, modifying the source can change what the view observes.

```cpp
std::string s = "Hello";

std::string_view view = s;

s[0] = 'Y';

std::cout << view;
```

Output:

```text
Yello
```

The view did not receive a copy. It observes the same characters.

---

## 117.5.7 Returning a View from a Function

This can be safe:

```cpp
std::string_view getText()
{
    static const std::string text = "Hello";

    return text;
}
```

The `static` string remains alive for the lifetime of the program.

But this is unsafe:

```cpp
std::string_view getText()
{
    std::string text = "Hello";

    return text;
}
```

General rule:

```text
Return string_view only when you can guarantee
the referenced characters outlive the returned view.
```

---

# 117.6 Performance

`std::string_view` can improve performance when functions only need to inspect text.

---

## 117.6.1 Avoiding Character Copies

Suppose:

```cpp
std::string s = "Hello World";

std::string_view view = s;
```

Creating `view` does not require copying all characters.

Conceptually:

```text
std::string:
    owns 11 characters

string_view:
    pointer + length
```

This makes creating views cheap.

---

## 117.6.2 Avoiding Allocation for Substrings

With `std::string`:

```cpp
std::string s = "Hello World";

std::string part = s.substr(0, 5);
```

A new string is created containing:

```text
Hello
```

With `string_view`:

```cpp
std::string s = "Hello World";

std::string_view view = s;

std::string_view part = view.substr(0, 5);
```

The second version creates another view over the existing characters.

Conceptually:

```text
s
|
+----> Hello World
       ^
       |
       +---- part view
```

No new character buffer is needed for the substring view.

---

## 117.6.3 Function Parameters

For a read-only function:

```cpp
void process(std::string_view text)
{
    // read text
}
```

This can accept:

```cpp
process("Hello");

std::string s = "Hello";
process(s);

std::string_view v = "Hello";
process(v);
```

The function does not need to create an owning copy just to inspect the characters.

---

## 117.6.4 Allocation vs No Allocation

Conceptually:

### `std::string`

```cpp
std::string part = s.substr(0, 5);
```

```text
source string
     |
     +---- existing buffer

new string
     |
     +---- new buffer containing "Hello"
```

### `std::string_view`

```cpp
std::string_view part = view.substr(0, 5);
```

```text
source string
     |
     +---- existing buffer
              ^
              |
          part view
```

The view itself does not need a character allocation.

---

## 117.6.5 Important: `string_view` Is Not Always Faster

`string_view` is useful, but it is not automatically better in every situation.

If a function needs to store the text beyond the lifetime of the input, it needs ownership.

For example:

```cpp
class Person
{
    std::string name;
};
```

is appropriate when the object needs to own the name.

Using:

```cpp
class Person
{
    std::string_view name;
};
```

requires careful lifetime management.

The source characters must remain alive as long as the `Person` uses the view.

---

# `std::string` vs `std::string_view`

| Feature | `std::string` | `std::string_view` |
|---|---|---|
| Owns data | Yes | No |
| Can modify characters | Yes | No |
| Owns memory | Yes | No |
| Can allocate | Yes | View itself does not allocate character storage |
| `substr()` result | New `std::string` | New `std::string_view` |
| Cheap to copy | Relatively | Very cheap |
| Lifetime responsibility | Managed by object | Caller/source must manage |
| Can dangle | Normally not from its own data | Yes |
| Null termination | `c_str()` guaranteed | Not guaranteed |
| Best for | Owning/storing text | Read-only observation |

---

# Example: Efficient Function Parameter

Instead of:

```cpp
void print(const std::string& text)
{
    std::cout << text;
}
```

you can often use:

```cpp
void print(std::string_view text)
{
    std::cout << text;
}
```

Now all of these can be passed:

```cpp
print("Hello");

std::string s = "Hello";
print(s);

std::string_view v = "Hello";
print(v);
```

The function only needs a read-only view, so ownership is unnecessary.

---

# Complete Example

```cpp
#include <iostream>
#include <string>
#include <string_view>

void process(std::string_view text)
{
    std::cout << "Text: " << text << '\n';
    std::cout << "Size: " << text.size() << '\n';

    auto pos = text.find("C++");

    if (pos != std::string_view::npos)
    {
        std::cout << "C++ found at position: "
                  << pos << '\n';
    }
}

int main()
{
    std::string message = "Learning C++ strings";

    std::string_view view = message;

    process(view);

    std::string_view part = view.substr(9, 3);

    std::cout << "Part: " << part << '\n';
}
```

Output:

```text
Text: Learning C++ strings
Size: 21
C++ found at position: 9
Part: C++
```

---

# Important Rules to Remember

1. `std::string_view` was introduced in C++17.
2. `string_view` is **non-owning**.
3. It generally behaves conceptually like a pointer plus a length.
4. Creating a view normally does not copy the characters.
5. `string_view` is primarily used for read-only access.
6. `operator[]` does not perform bounds checking.
7. `at()` performs bounds checking.
8. `front()` accesses the first character.
9. `back()` accesses the last character.
10. `size()` and `length()` return the number of viewed characters.
11. `data()` does not guarantee null termination.
12. `find()` returns the first matching position.
13. `rfind()` returns the last matching position.
14. `find_first_of()` searches for any character from a set.
15. `find_last_of()` searches from the end for any character from a set.
16. `string_view::substr()` creates another view, not an owning string.
17. A `string_view` can become dangling when its source characters are destroyed.
18. A `string_view` can become invalid when the source string reallocates.
19. Never return a view to a local `std::string`.
20. Never store a view referring to a temporary `std::string`.
21. Use `std::string` when ownership is required.
22. Use `std::string_view` when you only need a temporary/read-only view.
23. `string_view` can reduce character copies and allocations.
24. `string_view` is not automatically faster if ownership or a long-lived copy is ultimately required.
