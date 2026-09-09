# 5. C++ Tokens

Tokens are the **smallest meaningful units** of a C++ program that the compiler can recognize during lexical analysis.

For example:

```cpp
int age = 25;
```

This statement can be viewed as a sequence of tokens:

```text
int   age   =   25   ;
│     │     │    │    │
│     │     │    │    └── punctuator
│     │     │    └─────── literal
│     │     └──────────── operator
│     └────────────────── identifier
└──────────────────────── keyword
```

Understanding tokens is important because every C++ program is ultimately constructed from these lexical building blocks.

---

## 5.1 Token Types

The major token categories covered in this chapter are:

1. **Keywords**
2. **Identifiers**
3. **Literals**
4. **Operators**
5. **Punctuators**

Example:

```cpp
int total = count + 10;
```

| Token | Type | Explanation |
|---|---|---|
| `int` | Keyword | Specifies an integer type |
| `total` | Identifier | Name of a variable |
| `=` | Operator | Assignment operator |
| `count` | Identifier | Name of another object |
| `+` | Operator | Addition operator |
| `10` | Literal | Integer literal |
| `;` | Punctuator | Terminates the statement |

### 5.1.1 Keywords

Keywords are words that have a predefined meaning in C++.

```cpp
int
class
if
return
while
```

They cannot normally be used as identifiers.

```cpp
int class = 10;    // Error
```

---

### 5.1.2 Identifiers

Identifiers are programmer-defined names used for entities such as:

- Variables
- Functions
- Classes
- Objects
- Namespaces
- Enumerations
- Templates
- Members

Example:

```cpp
int age;

void printAge()
{
}
```

Here:

- `age` is an identifier.
- `printAge` is an identifier.

---

### 5.1.3 Literals

A literal represents a value written directly in the source code.

Examples:

```cpp
10
3.14
'A'
"Hello"
true
nullptr
```

Common literal categories include:

- Integer literals
- Floating-point literals
- Character literals
- String literals
- Boolean literals
- Pointer literal `nullptr`
- User-defined literals

Example:

```cpp
int x = 100;
double pi = 3.14159;
char grade = 'A';
const char* name = "Deep";
bool active = true;
```

---

### 5.1.4 Operators

Operators perform operations on operands.

Example:

```cpp
a + b
```

Here:

- `a` → operand
- `+` → operator
- `b` → operand

Common operator categories include:

- Arithmetic
- Relational
- Logical
- Assignment
- Bitwise
- Increment/decrement
- Member access
- Scope resolution
- Conditional
- Memory management
- Cast operators

Example:

```cpp
int result = a + b;
```

`=` and `+` are operators.

---

### 5.1.5 Punctuators

Punctuators organize the structure of a C++ program.

Examples:

```cpp
;
,
()
[]
{}
::
.
->
```

Example:

```cpp
if (x > 10)
{
    std::cout << x;
}
```

The following are important punctuators:

- `(` `)`
- `{` `}`
- `[` `]`
- `;`
- `,`
- `::`
- `.`
- `->`

---

# 5.2 Identifiers

An **identifier** is a name used by the programmer to identify a program entity.

Examples:

```cpp
int age;
double salary;

void calculate()
{
}

class Employee
{
};
```

Identifiers include:

```text
age
salary
calculate
Employee
```

---

## 5.2.1 Identifier Rules

C++ identifiers have syntactic rules.

An identifier can contain:

- Letters (`a-z`, `A-Z`)
- Digits (`0-9`)
- Underscore (`_`)

However, an identifier **cannot start with a digit**.

### Valid identifiers

```cpp
age
age2
student_name
_total
value123
MyClass
```

### Invalid identifiers

```cpp
2age        // Cannot start with a digit
student-name // '-' is not allowed
my name     // Space is not allowed
```

### Example

```cpp
int student1 = 10;
int student2 = 20;
```

Both `student1` and `student2` are valid identifiers.

---

## 5.2.2 Identifier Naming Rules vs Naming Conventions

It is important to distinguish **language rules** from **programming conventions**.

The compiler enforces syntax rules.

For example:

```cpp
int 123value = 10;   // Invalid
```

But naming style is generally a convention.

All of these can be syntactically valid:

```cpp
int age;
int studentAge;
int student_age;
int StudentAge;
```

A project may choose one particular style.

### Common naming conventions

#### camelCase

```cpp
int studentAge;
void calculateTotal();
```

#### PascalCase

```cpp
class StudentRecord;
class EmployeeManager;
```

#### snake_case

```cpp
int student_age;
void calculate_total();
```

#### UPPER_CASE

Often used for macros or constants by convention:

```cpp
#define MAX_SIZE 100
```

Modern C++ code frequently prefers:

```cpp
constexpr int maxSize = 100;
```

rather than using macros for constants.

---

## 5.2.3 Case Sensitivity

C++ identifiers are **case-sensitive**.

That means these are different identifiers:

```cpp
age
Age
AGE
aGe
```

Example:

```cpp
int age = 10;
int Age = 20;

std::cout << age << '\n';
std::cout << Age << '\n';
```

Output:

```text
10
20
```

`age` and `Age` refer to different names.

### Important

Do not assume that:

```cpp
Student
student
STUDENT
```

are the same name.

They are three different identifiers.

---

## 5.2.4 Identifiers Cannot Normally Be Keywords

A keyword already has a language-defined meaning.

Therefore, this is invalid:

```cpp
int return = 10;
```

Similarly:

```cpp
int class = 20;
```

is invalid.

But a keyword can appear as part of a larger identifier:

```cpp
int classCount = 10;
int returnValue = 20;
```

Here:

- `classCount` is an identifier.
- `returnValue` is an identifier.

The complete name is not the keyword `class` or `return`.

---

## 5.2.5 Reserved Identifiers

Some identifiers are reserved by the C++ implementation.

A programmer should not casually create names that are reserved to the implementation because doing so can result in undefined behavior or other problems.

### Names beginning with an underscore

Identifiers beginning with an underscore have restrictions depending on their scope and context.

For example, names beginning with:

```cpp
__something
```

are reserved.

Names containing double underscores are also reserved:

```cpp
my__variable
```

Names beginning with an underscore followed by an uppercase letter are reserved:

```cpp
_Foo
```

A safe general rule for application code is:

> **Avoid leading underscores and double underscores for your own identifiers.**

Prefer:

```cpp
int employeeCount;
```

instead of:

```cpp
int _employeeCount;
int __employeeCount;
```

---

## 5.2.6 Identifier Scope Is Separate From Identifier Syntax

Two identifiers can have the same spelling in different scopes.

Example:

```cpp
int value = 100;

void test()
{
    int value = 200;

    std::cout << value;
}
```

The local `value` hides the global `value` within the function.

The important distinction is:

- **Identifier syntax** → whether the name is legally formed.
- **Scope** → where the name can be used.
- **Linkage** → whether declarations in different scopes or translation units can refer to the same entity.

These concepts should not be confused.

---

# 5.3 Keywords

A **keyword** is a reserved word that has a special meaning in the C++ language.

Examples:

```cpp
int
class
if
else
return
for
while
template
try
catch
```

Keywords cannot be used as ordinary identifiers.

---

## 5.3.1 Core C++ Keywords

Some fundamental language keywords are:

```text
alignas
alignof
asm
auto
break
case
catch
class
const
consteval
constexpr
constinit
const_cast
continue
decltype
default
delete
do
dynamic_cast
else
enum
explicit
export
extern
false
for
friend
goto
if
inline
mutable
namespace
new
noexcept
nullptr
operator
private
protected
public
register
reinterpret_cast
requires
return
sizeof
static
static_assert
static_cast
struct
switch
template
this
thread_local
throw
true
try
typedef
typeid
typename
union
using
virtual
void
volatile
wchar_t
while
```

The exact language keyword set depends on the C++ standard version being used. Modern C++ standards have introduced additional keywords and contextual keywords.

---

# 5.3.2 Type Keywords

Type-related keywords are used to specify or participate in type declarations.

Common examples:

```cpp
bool
char
char8_t
char16_t
char32_t
double
float
int
long
short
signed
unsigned
void
wchar_t
```

Example:

```cpp
int age = 25;
double salary = 50000.50;
char grade = 'A';
bool active = true;
```

### Integer type keywords

```cpp
short
int
long
long long
signed
unsigned
```

Example:

```cpp
unsigned int count = 100;
long long population = 8000000000LL;
```

### Floating-point type keywords

```cpp
float
double
```

Example:

```cpp
float temperature = 25.5f;
double pi = 3.1415926535;
```

### Character types

```cpp
char
char8_t
char16_t
char32_t
wchar_t
```

Example:

```cpp
char c = 'A';
char16_t c16 = u'A';
char32_t c32 = U'A';
wchar_t wc = L'A';
```

---

# 5.3.3 Control-Flow Keywords

Control-flow keywords determine how program execution proceeds.

Important keywords include:

```cpp
if
else
switch
case
default
for
while
do
break
continue
goto
return
```

### `if`

```cpp
if (age >= 18)
{
    std::cout << "Adult";
}
```

### `else`

```cpp
if (age >= 18)
{
    std::cout << "Adult";
}
else
{
    std::cout << "Minor";
}
```

### `switch`

```cpp
switch (choice)
{
    case 1:
        std::cout << "One";
        break;

    case 2:
        std::cout << "Two";
        break;

    default:
        std::cout << "Other";
        break;
}
```

### `for`

```cpp
for (int i = 0; i < 10; ++i)
{
    std::cout << i;
}
```

### `while`

```cpp
while (count < 10)
{
    ++count;
}
```

### `do`

```cpp
do
{
    ++count;
}
while (count < 10);
```

### `break`

Terminates the nearest loop or `switch`.

```cpp
for (int i = 0; i < 10; ++i)
{
    if (i == 5)
        break;
}
```

### `continue`

Skips the remainder of the current loop iteration.

```cpp
for (int i = 0; i < 10; ++i)
{
    if (i % 2 == 0)
        continue;

    std::cout << i;
}
```

### `return`

Returns control from a function.

```cpp
int add(int a, int b)
{
    return a + b;
}
```

### `goto`

Transfers control to a labeled statement.

```cpp
goto end;

std::cout << "Skipped";

end:
std::cout << "Done";
```

`goto` is legal C++, but structured control flow is generally preferred.

---

# 5.3.4 Class-Related Keywords

C++ provides several keywords for defining and controlling classes and their members.

Important examples:

```cpp
class
struct
union
public
private
protected
friend
this
virtual
explicit
mutable
```

### `class`

```cpp
class Student
{
public:
    int age;
};
```

### `struct`

```cpp
struct Point
{
    int x;
    int y;
};
```

The major default-access difference is:

```cpp
class
```

members are `private` by default, while:

```cpp
struct
```

members are `public` by default.

Example:

```cpp
class A
{
    int value;       // private by default
};

struct B
{
    int value;       // public by default
};
```

### `public`

```cpp
class Student
{
public:
    void print();
};
```

### `private`

```cpp
class Student
{
private:
    int age;
};
```

### `protected`

```cpp
class Base
{
protected:
    int value;
};
```

### `this`

Inside a non-static member function, `this` points to the current object.

```cpp
class Student
{
private:
    int age;

public:
    void setAge(int age)
    {
        this->age = age;
    }
};
```

### `virtual`

Used for virtual functions and runtime polymorphism.

```cpp
class Base
{
public:
    virtual void print()
    {
    }
};
```

### `friend`

Allows a specified function or class to access private/protected members.

```cpp
class Box
{
private:
    int value;

    friend void show(const Box&);
};
```

---

# 5.3.5 Template-Related Keywords

Templates provide generic programming capabilities.

Important keywords include:

```cpp
template
typename
class
requires
concept
```

### `template`

Defines a template.

```cpp
template <typename T>
T add(T a, T b)
{
    return a + b;
}
```

### `typename`

Indicates a type in many template contexts.

```cpp
template <typename T>
void print(T value)
{
    std::cout << value;
}
```

It is also important when referring to dependent nested types:

```cpp
template <typename T>
void function()
{
    typename T::value_type value;
}
```

### `class` in template parameters

The following is also valid:

```cpp
template <class T>
void print(T value)
{
}
```

For template type parameters, `class` and `typename` are commonly interchangeable:

```cpp
template <class T>
void f(T value)
{
}

template <typename T>
void g(T value)
{
}
```

### `requires`

C++20 introduced constraints and requires-expressions.

```cpp
template <typename T>
requires std::integral<T>
T add(T a, T b)
{
    return a + b;
}
```

### `concept`

A concept defines a named constraint.

```cpp
template <typename T>
concept Number = std::integral<T> || std::floating_point<T>;
```

---

# 5.3.6 Exception-Related Keywords

C++ exception handling uses:

```cpp
try
catch
throw
noexcept
```

### `try`

Contains code that may throw an exception.

```cpp
try
{
    riskyOperation();
}
```

### `catch`

Handles an exception.

```cpp
try
{
    riskyOperation();
}
catch (const std::exception& e)
{
    std::cout << e.what();
}
```

### `throw`

Throws an exception.

```cpp
throw std::runtime_error("Error");
```

### `noexcept`

Specifies that a function does not throw exceptions under its exception specification.

```cpp
void process() noexcept
{
}
```

It can also be conditional:

```cpp
template <typename T>
void process(T value) noexcept(noexcept(value.doSomething()))
{
    value.doSomething();
}
```

---

# 5.3.7 Modern C++ Keywords

Modern versions of C++ introduced several important language features.

## `auto`

Allows type deduction.

```cpp
auto age = 25;
auto price = 99.99;
```

The compiler deduces the type from the initializer.

---

## `decltype`

Obtains the type of an expression.

```cpp
int x = 10;
decltype(x) y = 20;
```

Here `y` has the same declared type as `x`.

---

## `constexpr`

Used for values or functions that can participate in constant evaluation.

```cpp
constexpr int size = 100;
```

Example:

```cpp
constexpr int square(int x)
{
    return x * x;
}
```

---

## `consteval`

Introduced in C++20.

It declares an immediate function, meaning calls must produce a constant expression.

```cpp
consteval int square(int x)
{
    return x * x;
}
```

---

## `constinit`

Introduced in C++20.

It ensures that a variable with static or thread storage duration is constant-initialized.

```cpp
constinit int value = 100;
```

---

## `nullptr`

Represents a null pointer value.

```cpp
int* ptr = nullptr;
```

Prefer:

```cpp
nullptr
```

over the old:

```cpp
NULL
```

or:

```cpp
0
```

when representing a null pointer.

---

## `static_assert`

Performs a compile-time assertion.

```cpp
static_assert(sizeof(int) >= 2);
```

With a message:

```cpp
static_assert(sizeof(int) >= 2, "int is too small");
```

---

## `thread_local`

Specifies thread storage duration.

```cpp
thread_local int counter = 0;
```

Each thread has its own instance of the variable.

---

## `using`

Creates aliases and can also introduce declarations.

Type alias:

```cpp
using Integer = int;

Integer value = 10;
```

It can also be used for namespace or declaration-related purposes.

---

## `alignas`

Specifies alignment.

```cpp
struct alignas(16) Data
{
    int value;
};
```

---

## `alignof`

Queries alignment requirements.

```cpp
std::size_t alignment = alignof(Data);
```

---

## `noexcept`

Used to specify exception guarantees.

```cpp
void f() noexcept
{
}
```

---

## `requires` and `concept`

C++20 concepts provide constraints for templates.

```cpp
template <typename T>
concept Addable = requires(T a, T b)
{
    a + b;
};
```

Then:

```cpp
template <Addable T>
T add(T a, T b)
{
    return a + b;
}
```

---

# 5.4 Punctuators

Punctuators are symbols that have structural or syntactic meaning in C++.

Important punctuators include:

```text
;
,
.
->
::
:
?
()
[]
{}
<>
```

Some symbols can have different meanings depending on context.

For example:

```cpp
*
&
+
-
<
>
```

can function as operators or have other syntactic roles.

---

# 5.4.1 Semicolon `;`

The semicolon terminates many C++ statements.

Example:

```cpp
int x = 10;
x = 20;
std::cout << x;
```

Each statement ends with:

```cpp
;
```

### Declaration

```cpp
int value;
```

### Expression statement

```cpp
value = 100;
```

### Return statement

```cpp
return value;
```

### Empty statement

A standalone semicolon is an empty statement:

```cpp
;
```

It is syntactically valid, although usually not useful.

---

# 5.4.2 Comma `,`

The comma is used in several contexts.

### Multiple declarations

```cpp
int a = 10, b = 20, c = 30;
```

### Function arguments

```cpp
add(a, b);
```

### Function parameters

```cpp
int add(int a, int b)
{
    return a + b;
}
```

### Initializer lists

```cpp
int values[] = {10, 20, 30};
```

### Comma operator

The comma can also be an operator.

```cpp
int x = (a = 10, b = 20);
```

The comma operator evaluates its left operand, then its right operand, and the result is the value of the right operand.

---

# 5.4.3 Dot `.`

The dot operator accesses members of an object.

```cpp
object.member
```

Example:

```cpp
struct Student
{
    int age;
};

Student s;
s.age = 20;
```

Here:

```cpp
s.age
```

uses the dot operator.

---

# 5.4.4 Arrow `->`

The arrow operator accesses a member through a pointer.

```cpp
pointer->member
```

Example:

```cpp
Student* ptr = &s;

ptr->age = 20;
```

The following are conceptually equivalent:

```cpp
ptr->age
```

and:

```cpp
(*ptr).age
```

Parentheses are necessary in the second form because `.` has higher precedence than unary `*`:

```cpp
(*ptr).age
```

---

# 5.4.5 Scope Resolution `::`

The scope resolution operator identifies a name in a particular scope.

### Namespace

```cpp
std::cout
std::string
```

Here `std` is a namespace and `cout`/`string` are names within it.

### Class member definition

```cpp
class Student
{
public:
    void print();
};

void Student::print()
{
    std::cout << "Student";
}
```

`Student::print` means the `print` member belonging to `Student`.

### Global scope

```cpp
int value = 100;

void test()
{
    int value = 200;

    std::cout << ::value;
}
```

`::value` refers to the global-scope `value`.

---

# 5.4.6 Colon `:`

The colon is used in several C++ constructs.

### Labels

```cpp
start:
    std::cout << "Start";
```

### `case`

```cpp
switch (value)
{
    case 1:
        std::cout << "One";
        break;
}
```

### Inheritance

```cpp
class Derived : public Base
{
};
```

### Constructor initializer list

```cpp
class Student
{
private:
    int age;

public:
    Student(int value)
        : age(value)
    {
    }
};
```

### Conditional operator

```cpp
int result = condition ? 10 : 20;
```

The colon separates the second and third operands of the conditional expression.

---

# 5.4.7 Question Mark `?`

The question mark participates in the conditional operator:

```cpp
condition ? expression1 : expression2
```

Example:

```cpp
int max = (a > b) ? a : b;
```

Meaning:

```text
if a > b
    result = a
else
    result = b
```

The conditional operator is an expression and therefore can produce a value.

---

# 5.4.8 Parentheses `()`

Parentheses have several important uses.

### Function declaration

```cpp
int add(int a, int b);
```

### Function call

```cpp
add(10, 20);
```

### Grouping expressions

```cpp
int result = (a + b) * c;
```

### Conditions

```cpp
if (x > 10)
{
}
```

### Cast syntax

C++ also has functional-style casts:

```cpp
int value = int(3.14);
```

However, modern C++ generally provides named casts such as:

```cpp
static_cast<int>(3.14);
```

---

# 5.4.9 Square Brackets `[]`

Square brackets are used for indexing, arrays, lambda captures, and other language features.

### Array indexing

```cpp
int values[3] = {10, 20, 30};

std::cout << values[0];
```

The index `0` accesses the first element.

### Subscript operator

For an object that supports subscripting:

```cpp
container[index]
```

For example:

```cpp
std::vector<int> values = {10, 20, 30};

std::cout << values[1];
```

### Lambda capture

Square brackets introduce a lambda capture list:

```cpp
int x = 10;

auto f = [x]()
{
    std::cout << x;
};
```

---

# 5.4.10 Curly Braces `{}`

Curly braces define blocks and initialization structures.

### Function body

```cpp
void test()
{
    int x = 10;
}
```

### Class body

```cpp
class Student
{
    int age;
};
```

### Namespace body

```cpp
namespace company
{
    int value;
}
```

### Initialization

```cpp
int values[] = {10, 20, 30};
```

### List initialization

```cpp
int x{10};
std::vector<int> values{1, 2, 3};
```

Brace initialization is a major feature of modern C++.

---

# 5.4.11 Angle Brackets `<>`

Angle brackets have several important uses.

### Template arguments

```cpp
std::vector<int>
```

Here:

```text
std::vector
      └── template
          argument: int
```

### Multiple template arguments

```cpp
std::map<std::string, int>
```

### Comparison operators

The same symbols can act as relational operators:

```cpp
if (a < b)
{
}

if (a > b)
{
}
```

They can also participate in:

```cpp
a <= b
a >= b
```

### Stream operators

They are also used in:

```cpp
std::cout << value;
std::cin >> value;
```

In these expressions, `<<` and `>>` are operators.

---

# 5.5 Tokenization Example

Consider this program:

```cpp
int main()
{
    int age = 25;

    if (age >= 18)
    {
        std::cout << "Adult";
    }

    return 0;
}
```

A simplified token breakdown is:

```text
int       keyword
main      identifier
(         punctuator
)         punctuator
{         punctuator

int       keyword
age       identifier
=         operator
25        integer literal
;         punctuator

if        keyword
(         punctuator
age       identifier
>=        operator
18        integer literal
)         punctuator

{         punctuator

std       identifier
::        punctuator
cout      identifier
<<        operator
"Adult"   string literal
;         punctuator

}         punctuator

return    keyword
0         integer literal
;         punctuator

}         punctuator
```

This illustrates how a C++ source file is composed from lexical elements.

---

# 5.6 Whitespace and Tokens

Whitespace is generally used to separate tokens and improve readability.

Example:

```cpp
int x = 10;
```

and:

```cpp
int     x     =     10;
```

are equivalent in this context.

Whitespace can include:

- Spaces
- Tabs
- Newlines

However, whitespace is sometimes important because it separates tokens.

For example:

```cpp
int value;
```

is different from trying to combine the text into one identifier.

Comments are also removed/handled during preprocessing and lexical processing and do not become ordinary program tokens in the same way as source-code identifiers or literals.

---

# 5.7 Tokens vs Lexemes

A useful terminology distinction is:

- **Token** → a categorized lexical unit.
- **Lexeme** → the actual character sequence in the source that forms that unit.

Example:

```cpp
int age = 25;
```

Possible lexemes are:

```text
int
age
=
25
;
```

Their token categories are approximately:

```text
int  → keyword
age  → identifier
=    → operator
25   → integer literal
;    → punctuator
```

---

# 5.8 Important Token Rules to Remember

### Rule 1: Keywords have predefined meanings

```cpp
int
if
return
class
```

cannot normally be used as identifiers.

---

### Rule 2: Identifiers are case-sensitive

```cpp
value
Value
VALUE
```

are different names.

---

### Rule 3: Identifiers cannot start with a digit

Valid:

```cpp
value1
```

Invalid:

```cpp
1value
```

---

### Rule 4: Avoid reserved identifier patterns

Avoid names such as:

```cpp
__name
_Name
```

and generally avoid leading underscores in application-level identifiers.

---

### Rule 5: Literals represent written values

```cpp
100
3.14
'A'
"Hello"
true
nullptr
```

---

### Rule 6: Operators perform or participate in operations

```cpp
+
-
*
/
=
==
&&
||
```

---

### Rule 7: Punctuators provide structure

```cpp
;
,
()
[]
{}
::
.
->
```

---

# 5.9 Complete Example

```cpp
#include <iostream>
#include <string>

class Student
{
private:
    std::string name;
    int age;

public:
    Student(std::string n, int a)
        : name(n), age(a)
    {
    }

    void print() const
    {
        if (age >= 18)
        {
            std::cout << name << " is an adult\n";
        }
        else
        {
            std::cout << name << " is a minor\n";
        }
    }
};

int main()
{
    Student student("Deep", 25);

    student.print();

    return 0;
}
```

Important token examples from this program:

| Token | Category | Example |
|---|---|---|
| `class` | Keyword | `class Student` |
| `Student` | Identifier | Class name |
| `private` | Keyword | Access specifier |
| `public` | Keyword | Access specifier |
| `std` | Identifier | Namespace name |
| `::` | Punctuator | Scope resolution |
| `string` | Identifier | Type name |
| `(` `)` | Punctuators | Parameters/calls |
| `:` | Punctuator | Constructor initializer / inheritance |
| `,` | Punctuator | Separates arguments |
| `>=` | Operator | Comparison |
| `<<` | Operator | Stream insertion |
| `"Deep"` | String literal | String value |
| `25` | Integer literal | Integer value |
| `;` | Punctuator | Statement termination |
| `{` `}` | Punctuators | Blocks/class body |

---

# 5.10 Quick Revision

```text
C++ SOURCE CODE
      │
      ▼
  Characters
      │
      ▼
  Lexical analysis
      │
      ▼
    Tokens
      │
      ├── Keywords
      ├── Identifiers
      ├── Literals
      ├── Operators
      └── Punctuators
```

### Token categories

| Category | Purpose | Examples |
|---|---|---|
| Keyword | Reserved language word | `int`, `class`, `if` |
| Identifier | Programmer-defined name | `age`, `Student` |
| Literal | Directly written value | `10`, `"Hello"`, `true` |
| Operator | Performs an operation | `+`, `=`, `==` |
| Punctuator | Structures syntax | `;`, `()`, `{}`, `::` |

### Identifier checklist

```text
✓ Letters allowed
✓ Digits allowed after the first character
✓ Underscore allowed
✓ Case-sensitive
✗ Cannot start with a digit
✗ Cannot normally be a keyword
✗ Avoid reserved implementation identifiers
```

### Punctuator checklist

```text
;    statement termination
,    separation / comma operator
.    object member access
->   pointer member access
::   scope resolution
:    labels / case / inheritance / initializer lists
?    conditional operator
()   calls / parameters / grouping
[]   subscripting / lambda captures
{}   blocks / initialization
<>   templates / comparisons / other operators
```

---

# 5.11 Key Takeaway

A C++ program is built from many lexical elements. The most important categories for understanding source code are:

```text
Keywords
Identifiers
Literals
Operators
Punctuators
```

For example:

```cpp
int age = 25;
```

can be understood as:

```text
int  → keyword
age  → identifier
=    → operator
25   → literal
;    → punctuator
```

Once these categories are understood, reading C++ syntax becomes much easier because you can recognize **what each piece of source code represents** before learning the larger language constructs built from those pieces.
