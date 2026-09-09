# 131. String Streams

String streams allow C++ programs to treat a string like an input/output stream.

Header:

```cpp
#include <sstream>
```

The three main string-stream classes are:

```text
std::stringstream
std::istringstream
std::ostringstream
```

They are useful for:

- Parsing text
- Converting strings to numbers
- Converting numbers to strings
- Formatting output
- Tokenizing text
- Building complex strings

---

# 131.1 `stringstream`

`std::stringstream` supports **both input and output**.

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::stringstream ss;

    ss << "Age: " << 25;

    std::string result = ss.str();

    std::cout << result;
}
```

Output:

```text
Age: 25
```

Conceptually:

```text
             std::stringstream
                    |
          +---------+---------+
          |                   |
       write <<             read >>
          |                   |
          +---------+---------+
                    |
               string buffer
```

## Writing into `stringstream`

```cpp
std::stringstream ss;

ss << "Hello " << "World " << 123;
```

Get the complete string:

```cpp
std::string result = ss.str();
```

Result:

```text
Hello World 123
```

---

## Reading from `stringstream`

```cpp
std::stringstream ss("100 200");

int a;
int b;

ss >> a >> b;
```

Now:

```text
a = 100
b = 200
```

A `stringstream` can therefore be used as both:

```text
string -> values
values -> string
```

---

## Reading Tokens

```cpp
std::stringstream ss("one two three");

std::string word;

while (ss >> word)
{
    std::cout << word << '\n';
}
```

Output:

```text
one
two
three
```

The extraction operator skips leading whitespace and reads until whitespace.

---

## `str()`

`str()` gets the current contents.

```cpp
std::stringstream ss;

ss << "Hello";

std::string s = ss.str();
```

It can also replace the underlying string:

```cpp
ss.str("New text");
```

Now the stream contains:

```text
New text
```

---

# 131.2 `istringstream`

`std::istringstream` is used primarily for **input/parsing from a string**.

Header:

```cpp
#include <sstream>
```

Example:

```cpp
std::istringstream iss("10 20 30");

int a, b, c;

iss >> a >> b >> c;
```

Result:

```text
a = 10
b = 20
c = 30
```

Conceptually:

```text
string
   |
   v
istringstream
   |
   +---- >> value
   |
   +---- >> value
   |
   +---- >> value
```

---

## Parsing a Line

Suppose we have:

```cpp
std::string line = "100 200 300";
```

We can parse it:

```cpp
std::istringstream iss(line);

int x;
int y;
int z;

iss >> x >> y >> z;
```

Now:

```text
x = 100
y = 200
z = 300
```

---

## Parsing Mixed Data

```cpp
std::string line = "Akash 25";

std::istringstream iss(line);

std::string name;
int age;

iss >> name >> age;
```

Result:

```text
name = "Akash"
age  = 25
```

---

## Parsing Different Types

```cpp
std::istringstream iss("Akash 25 95.5");

std::string name;
int age;
double marks;

iss >> name >> age >> marks;
```

Result:

```text
name  = "Akash"
age   = 25
marks = 95.5
```

---

## Checking Parsing Success

Stream extraction can be used directly in a condition:

```cpp
std::istringstream iss("10 20 30");

int value;

while (iss >> value)
{
    std::cout << value << '\n';
}
```

This continues while extraction succeeds.

For invalid input:

```cpp
std::istringstream iss("10 abc 30");

int value;

while (iss >> value)
{
    std::cout << value << '\n';
}
```

The stream eventually enters a failure state when `"abc"` cannot be extracted as an `int`.

---

## Parsing Comma-Separated Values

Use `std::getline()` with a delimiter:

```cpp
std::string input = "red,green,blue";

std::istringstream iss(input);

std::string token;

while (std::getline(iss, token, ','))
{
    std::cout << token << '\n';
}
```

Output:

```text
red
green
blue
```

The third argument of `getline()` specifies the delimiter.

---

# 131.3 `ostringstream`

`std::ostringstream` is used primarily for **output/building a string**.

```cpp
std::ostringstream oss;

oss << "Name: " << "Akash"
    << ", Age: " << 25;

std::string result = oss.str();
```

Result:

```text
Name: Akash, Age: 25
```

Conceptually:

```text
values
  |
  v
ostringstream
  |
  +---- << value
  |
  +---- << value
  |
  v
string
```

---

## Building a String

```cpp
std::ostringstream oss;

oss << "ID = "
    << 101
    << ", Score = "
    << 95.5;

std::string result = oss.str();
```

Result:

```text
ID = 101, Score = 95.5
```

---

## Formatting with `iomanip`

`ostringstream` works with standard formatting manipulators.

```cpp
#include <iomanip>
#include <sstream>

std::ostringstream oss;

oss << std::fixed
    << std::setprecision(2)
    << 123.4567;

std::string result = oss.str();
```

Result:

```text
123.46
```

---

## Width and Fill

```cpp
#include <iomanip>
#include <sstream>

std::ostringstream oss;

oss << std::setw(5)
    << std::setfill('0')
    << 42;

std::string result = oss.str();
```

Result:

```text
00042
```

---

## Different Number Bases

```cpp
std::ostringstream oss;

oss << std::hex << 255;

std::string result = oss.str();
```

Result:

```text
ff
```

Other bases:

```cpp
oss << std::dec << 255;  // decimal
oss << std::oct << 255;  // octal
oss << std::hex << 255;  // hexadecimal
```

---

# 131.4 String Conversion

String streams can be used for conversion between textual and typed data.

Main directions:

```text
Number / object
      |
      v
  string
```

and:

```text
string
   |
   v
Number / object
```

---

## 131.4.1 Integer to String

Using `ostringstream`:

```cpp
int value = 123;

std::ostringstream oss;

oss << value;

std::string result = oss.str();
```

Result:

```text
"123"
```

For simple conversion, `std::to_string()` is shorter:

```cpp
std::string result = std::to_string(value);
```

---

## 131.4.2 Floating-Point to String

```cpp
double value = 123.456;

std::ostringstream oss;

oss << value;

std::string result = oss.str();
```

For controlled formatting:

```cpp
std::ostringstream oss;

oss << std::fixed
    << std::setprecision(2)
    << value;

std::string result = oss.str();
```

Result:

```text
"123.46"
```

---

## 131.4.3 String to Integer

Using `istringstream`:

```cpp
std::string s = "123";

std::istringstream iss(s);

int value;

iss >> value;
```

Now:

```text
value = 123
```

For simple conversion, `std::stoi()` can be used:

```cpp
int value = std::stoi("123");
```

---

## 131.4.4 String to Double

```cpp
std::string s = "123.45";

std::istringstream iss(s);

double value;

iss >> value;
```

Or:

```cpp
double value = std::stod("123.45");
```

---

## 131.4.5 Generic Conversion

A string stream can convert values without writing a separate conversion function for every numeric type.

Example:

```cpp
template <typename T>
T convert(const std::string& text)
{
    std::istringstream iss(text);

    T value{};
    iss >> value;

    return value;
}
```

Usage:

```cpp
int a = convert<int>("123");
double b = convert<double>("12.34");
```

A production version should also check whether parsing succeeded and whether unwanted trailing input remains.

---

# 131.5 Parsing

Parsing means taking text and interpreting it as structured values.

Example:

```text
"Akash 25 95.5"
```

can become:

```text
name  = "Akash"
age   = 25
marks = 95.5
```

using:

```cpp
std::istringstream iss("Akash 25 95.5");

std::string name;
int age;
double marks;

iss >> name >> age >> marks;
```

---

## 131.5.1 Token Parsing

```cpp
std::string input = "C++ is powerful";

std::istringstream iss(input);

std::string word;

while (iss >> word)
{
    std::cout << word << '\n';
}
```

Output:

```text
C++
is
powerful
```

---

## 131.5.2 Parsing with a Custom Delimiter

```cpp
std::string input = "10,20,30,40";

std::istringstream iss(input);

std::string token;

while (std::getline(iss, token, ','))
{
    std::cout << token << '\n';
}
```

Output:

```text
10
20
30
40
```

If integers are required, convert each token:

```cpp
int value = std::stoi(token);
```

---

## 131.5.3 Parsing a CSV-Like Line

```cpp
std::string line = "101,Akash,95.5";

std::istringstream iss(line);

std::string idText;
std::string name;
std::string marksText;

std::getline(iss, idText, ',');
std::getline(iss, name, ',');
std::getline(iss, marksText, ',');

int id = std::stoi(idText);
double marks = std::stod(marksText);
```

Now:

```text
id    = 101
name  = Akash
marks = 95.5
```

This is useful for simple delimiter-based formats.

A full CSV parser is more complicated because real CSV supports quoted fields, escaped quotes, embedded delimiters, and newlines inside quoted fields.

---

## 131.5.4 Parsing User Input

```cpp
std::string line;

std::getline(std::cin, line);

std::istringstream iss(line);

int age;
double salary;

if (iss >> age >> salary)
{
    std::cout << "Valid input\n";
}
else
{
    std::cout << "Invalid input\n";
}
```

---

# 131.6 Formatting

Formatting means converting values into a desired textual representation.

---

## 131.6.1 Decimal Precision

```cpp
double price = 123.456789;

std::ostringstream oss;

oss << std::fixed
    << std::setprecision(2)
    << price;
```

Result:

```text
123.46
```

---

## 131.6.2 Scientific Notation

```cpp
std::ostringstream oss;

oss << std::scientific << 12345.67;
```

The result is represented using scientific notation.

---

## 131.6.3 Boolean Formatting

By default:

```cpp
bool value = true;

std::ostringstream oss;

oss << value;
```

Result:

```text
1
```

Use `std::boolalpha`:

```cpp
oss << std::boolalpha << value;
```

Result:

```text
true
```

To return to numeric representation:

```cpp
oss << std::noboolalpha;
```

---

## 131.6.4 Alignment

```cpp
std::ostringstream oss;

oss << std::left
    << std::setw(10)
    << "Name";
```

For right alignment:

```cpp
oss << std::right
    << std::setw(10)
    << "Name";
```

---

## 131.6.5 Building a Report

```cpp
#include <iomanip>
#include <sstream>
#include <string>

int main()
{
    std::string name = "Akash";
    int age = 25;
    double salary = 45000.75;

    std::ostringstream oss;

    oss << "Name   : " << name << '\n'
        << "Age    : " << age << '\n'
        << "Salary : " << std::fixed
        << std::setprecision(2)
        << salary;

    std::string report = oss.str();
}
```

Result:

```text
Name   : Akash
Age    : 25
Salary : 45000.75
```

---

# 131.7 String Streams vs Other Conversion Methods

C++ provides several ways to convert between strings and values.

| Method | Main Use |
|---|---|
| `std::stringstream` | Read and write strings |
| `std::istringstream` | Parse a string |
| `std::ostringstream` | Build/format a string |
| `std::to_string` | Simple number → string |
| `std::stoi` / `std::stod` | Simple string → number |
| `std::from_chars` | Fast numeric parsing |
| `std::to_chars` | Fast numeric formatting |
| `std::format` | Modern formatted output |

---

## When to Use `stringstream`

Use `stringstream` when you need both directions:

```text
string <-> values
```

Example:

```cpp
std::stringstream ss;

ss << 100;

int value;

ss >> value;
```

---

## When to Use `istringstream`

Use `istringstream` when the main operation is:

```text
string -> values
```

Example:

```cpp
std::istringstream iss("100 200");

int a, b;

iss >> a >> b;
```

---

## When to Use `ostringstream`

Use `ostringstream` when the main operation is:

```text
values -> formatted string
```

Example:

```cpp
std::ostringstream oss;

oss << "Total = " << 100;

std::string result = oss.str();
```

---

## When to Use `std::to_string`

For simple numeric conversion:

```cpp
std::string s = std::to_string(123);
```

It is shorter than:

```cpp
std::ostringstream oss;
oss << 123;
std::string s = oss.str();
```

---

## When to Use `std::from_chars`

For performance-sensitive numeric parsing:

```cpp
#include <charconv>

const char* first = "12345";
const char* last = first + 5;

int value{};

auto result = std::from_chars(first, last, value);

if (result.ec == std::errc{})
{
    // Successful conversion
}
```

`from_chars()` is designed for low-level, efficient conversion and does not require constructing a stream.

---

## When to Use `std::format`

C++20 introduced `std::format`:

```cpp
#include <format>

std::string result =
    std::format("Name: {}, Age: {}", "Akash", 25);
```

This is often cleaner than stream formatting for modern C++ code when the implementation provides `std::format`.

---

# Important Differences

```text
stringstream
    |
    +-- input  >> 
    +-- output <<

istringstream
    |
    +-- input  >>

ostringstream
    |
    +-- output <<
```

Think of them as:

```text
istringstream
"100 200"
    |
    v
  parse
    |
    v
100, 200
```

and:

```text
100, 200
   |
   v
format
   |
   v
"100 200"
```

---

# Common Mistakes

## Mistake 1 — Forgetting `str()`

Wrong:

```cpp
std::ostringstream oss;

oss << 100;

std::string s = oss;    // wrong
```

Correct:

```cpp
std::string s = oss.str();
```

---

## Mistake 2 — Assuming `>>` Reads the Entire Line

```cpp
std::istringstream iss("Akash Kumar");

std::string name;

iss >> name;
```

Only:

```text
Akash
```

is extracted.

For the complete remaining line, use `std::getline()`.

---

## Mistake 3 — Not Checking Conversion

```cpp
std::istringstream iss("abc");

int value;

iss >> value;
```

This fails because `"abc"` is not a valid integer.

Prefer checking:

```cpp
if (iss >> value)
{
    // success
}
else
{
    // failure
}
```

---

## Mistake 4 — Reusing a Stream Without Clearing State

Once a stream enters a failure state, simply changing its contents may not reset the state.

Use:

```cpp
ss.clear();
ss.str("new input");
```

Example:

```cpp
std::stringstream ss("abc");

int value;

ss >> value;      // fails

ss.clear();       // reset state
ss.str("123");    // replace contents

ss >> value;      // succeeds
```

---

# Summary

```text
std::stringstream
    = input + output

std::istringstream
    = input from string

std::ostringstream
    = output to string
```

String streams are especially useful for:

```text
Parsing
    ↓
"100 200 300"
    ↓
100 200 300

Formatting
    ↓
100 + 200 + 300
    ↓
"100 200 300"

Conversion
    ↓
string ↔ values
```

For simple conversions use:

```cpp
std::to_string()
std::stoi()
std::stod()
```

For efficient low-level numeric conversion use:

```cpp
std::from_chars()
std::to_chars()
```

For modern formatted output use:

```cpp
std::format()
```

String streams remain very useful when parsing or formatting requires the familiar C++ stream interface and manipulators such as `std::fixed`, `std::setprecision`, `std::setw`, `std::hex`, and `std::boolalpha`.
