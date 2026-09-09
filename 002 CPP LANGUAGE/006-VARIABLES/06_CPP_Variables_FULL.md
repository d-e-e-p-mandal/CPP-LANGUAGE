# 6. Variables in C++

> A **variable** is a named object that represents a region of storage whose value can be used and, when allowed, changed during program execution.
>
> This chapter covers variable declaration, definition, initialization, naming, scope, storage duration, linkage-related concepts, and the major categories of variables in modern C++.

---

# 6.1 Variable Declaration

## 6.1.1 What Is a Variable?

A variable is an object with:

- a type,
- a name,
- a storage location,
- a lifetime,
- a scope,
- and a value (once initialized or assigned appropriately).

Example:

```cpp
int age = 25;
```

Here:

| Part | Meaning |
|---|---|
| `int` | Type |
| `age` | Variable/object name |
| `=` | Initialization syntax |
| `25` | Initializer/value |
| `;` | Statement terminator |

A variable allows a program to store and manipulate data.

```cpp
int score = 100;

score = 150;
```

The variable `score` initially contains `100`, then contains `150`.

---

# 6.1.2 Declaration

A **declaration** introduces a name and its type to the program.

Example:

```cpp
extern int count;
```

This declares `count` as an `int`, but does not define the object.

Another common declaration is:

```cpp
int calculate(int, int);
```

This declares a function.

For variables:

```cpp
extern int total;
```

The declaration tells the compiler:

> There is an `int` object named `total` somewhere.

---

# 6.1.3 Definition

A **definition** provides the entity itself and, for an object, normally causes storage to be associated with it.

```cpp
int total;
```

This is a definition of `total`.

Another example:

```cpp
int age = 25;
```

This both defines and initializes `age`.

### Declaration vs Definition

```cpp
// Declaration
extern int count;

// Definition
int count = 10;
```

The declaration says that `count` exists.

The definition actually defines the object.

---

# 6.1.4 Declaration and Definition Together

Most local variables are declared and defined in the same statement:

```cpp
int x;
```

This is a definition.

```cpp
int x = 10;
```

This is also a definition and performs initialization.

For ordinary variables, you will frequently see:

```cpp
int a = 10;
double price = 99.50;
char grade = 'A';
bool active = true;
```

---

# 6.1.5 Initialization

**Initialization** gives an object its initial value when its lifetime begins.

```cpp
int age = 25;
```

`age` is initialized with `25`.

Initialization is different from assignment.

```cpp
int x = 10;  // initialization
x = 20;      // assignment
```

The first statement creates/initializes `x`.

The second changes the value of an already-existing object.

---

# 6.1.6 Assignment

Assignment replaces the value held by an existing object.

```cpp
int x = 10;

x = 50;
```

Conceptually:

```text
Create x
   ↓
Initialize x with 10
   ↓
x already exists
   ↓
Assign 50 to x
```

### Initialization vs Assignment

| Initialization | Assignment |
|---|---|
| Happens when object is initialized | Happens after object exists |
| Establishes initial state | Changes existing state |
| `int x = 10;` | `x = 20;` |
| Constructor initialization is important for class objects | Assignment may invoke assignment operators |

Example with a class:

```cpp
std::string name = "Deep"; // initialization
name = "Alex";             // assignment
```

---

# 6.1.7 Multiple Variable Declarations

C++ permits multiple declarations/definitions in one statement:

```cpp
int a = 10, b = 20, c = 30;
```

However, for readability, separate declarations are often preferable:

```cpp
int a = 10;
int b = 20;
int c = 30;
```

Be careful with pointers:

```cpp
int* p, q;
```

Only `p` is a pointer.

`q` is an `int`.

Equivalent:

```cpp
int* p;
int q;
```

---

# 6.1.8 `const` Variables

A `const` variable cannot normally be modified after initialization.

```cpp
const int maxUsers = 100;
```

This is valid:

```cpp
const int x = 10;
```

This is invalid:

```cpp
const int x = 10;
x = 20; // ERROR
```

A const object generally needs initialization when it is defined:

```cpp
const int value = 42;
```

---

# 6.1.9 `constexpr` Variables

A `constexpr` variable is intended to hold a constant expression and must be initialized with a constant expression.

```cpp
constexpr int size = 100;
```

Example:

```cpp
constexpr int width = 10;
constexpr int height = 20;
constexpr int area = width * height;
```

`constexpr` is stronger than merely saying:

```cpp
const int value = 10;
```

because `constexpr` communicates compile-time constant-expression intent.

---

# 6.1.10 `auto` Variables

C++ can deduce a variable's type from its initializer using `auto`.

```cpp
auto age = 25;
```

The type is deduced as:

```cpp
int
```

Example:

```cpp
auto price = 99.5;       // double
auto letter = 'A';       // char
auto active = true;      // bool
auto name = "Deep";      // const char*
```

`auto` requires an initializer in ordinary variable declarations:

```cpp
auto x = 10; // OK
```

Not:

```cpp
auto x; // ERROR
```

---

# 6.1.11 Reference Variables

A reference is an alias for another object.

```cpp
int x = 10;
int& ref = x;
```

Changing `ref` changes `x`:

```cpp
ref = 50;

std::cout << x; // 50
```

A reference must be initialized when it is declared.

```cpp
int& ref = x;
```

There is no normal "null reference" value.

---

# 6.1.12 Pointer Variables

A pointer stores an address.

```cpp
int x = 10;
int* p = &x;
```

Here:

```text
x
┌──────────┐
│    10    │
└──────────┘
     ↑
     │
p ───┘
```

Dereferencing:

```cpp
*p = 20;
```

Now `x` becomes `20`.

A pointer can be null:

```cpp
int* p = nullptr;
```

---

# 6.2 Variable Naming

## 6.2.1 Naming Rules

A variable name is an identifier.

Typical C++ identifier rules include:

1. It may contain letters.
2. It may contain digits.
3. It may contain underscores.
4. It cannot begin with a digit.
5. It cannot be a keyword.
6. C++ identifiers are case-sensitive.

Examples:

```cpp
int age;
int student_count;
int total2;
```

Invalid:

```cpp
int 2total;      // ERROR
int student-age; // ERROR
int class;       // ERROR: keyword
```

---

# 6.2.2 Valid Identifiers

Examples:

```cpp
int age;
int age2;
int studentCount;
int student_count;
int _value;
```

Although some underscore-prefixed names are syntactically valid, certain underscore naming patterns are reserved to the implementation. Therefore, avoid reserved forms in application code.

---

# 6.2.3 Invalid Identifiers

```cpp
int 123abc;       // ERROR: starts with digit
int student-name; // ERROR: '-' is not part of an identifier
int my name;      // ERROR: whitespace
int class;        // ERROR: keyword
```

---

# 6.2.4 Case Sensitivity

C++ is case-sensitive.

These are different variables:

```cpp
int value = 10;
int Value = 20;
int VALUE = 30;
```

They are three separate identifiers.

```cpp
value != Value
```

This can cause bugs:

```cpp
int total = 100;

std::cout << Total; // ERROR
```

`Total` and `total` are different names.

---

# 6.2.5 Naming Conventions

C++ does not require one universal naming style.

Common styles include:

### camelCase

```cpp
int studentCount;
double totalPrice;
```

### PascalCase

```cpp
int StudentCount;
```

Often used for types/classes:

```cpp
class StudentRecord {};
```

### snake_case

```cpp
int student_count;
double total_price;
```

### SCREAMING_SNAKE_CASE

Often used for macros or some constants, although `constexpr` constants are frequently written using ordinary project-specific conventions:

```cpp
#define MAX_SIZE 100
```

Prefer project consistency over mixing styles.

---

# 6.2.6 Good Variable Names

Prefer names that communicate meaning:

```cpp
int numberOfStudents;
double accountBalance;
bool isConnected;
```

Avoid meaningless names:

```cpp
int a;
double x;
int temp;
```

Short names can be appropriate for small, obvious scopes:

```cpp
for (int i = 0; i < 10; ++i)
{
}
```

---

# 6.2.7 Boolean Naming

Boolean variables often use names such as:

```cpp
bool isReady;
bool hasPermission;
bool canRead;
bool shouldRetry;
```

This makes conditions easier to read:

```cpp
if (isReady)
{
    start();
}
```

---

# 6.2.8 Scope-Based Naming

Names should remain understandable within their scope.

Example:

```cpp
void process()
{
    int count = 0;

    for (int i = 0; i < 10; ++i)
    {
        // i has a small scope
    }
}
```

A short name such as `i` is conventional for a loop index.

For a broader scope, use a descriptive name:

```cpp
int numberOfProcessedRecords;
```

Avoid unnecessarily generic names for long-lived objects.

---

# 6.2.9 Shadowing

A nested scope can contain a declaration with the same name as an outer declaration.

```cpp
int value = 10;

{
    int value = 20;
    std::cout << value; // 20
}

std::cout << value;     // 10
```

The inner `value` shadows the outer `value`.

Avoid unnecessary shadowing because it can make code confusing.

---

# 6.3 Variable Initialization

C++ has several initialization forms.

The major forms are:

1. Default initialization
2. Value initialization
3. Zero initialization
4. Copy initialization
5. Direct initialization
6. List initialization
7. Aggregate initialization

These forms are related but are not interchangeable.

---

# 6.3.1 Default Initialization

Default initialization occurs when an object is initialized without an initializer.

For a local fundamental-type variable:

```cpp
int x;
```

`x` is default-initialized.

For a local automatic `int`, this means its value is **indeterminate**.

Do not read it before giving it a valid value.

```cpp
int x;

std::cout << x; // ERROR: reading an indeterminate value
```

Better:

```cpp
int x = 0;
```

---

## Default Initialization of Class Objects

For a class type:

```cpp
class Student
{
public:
    Student()
    {
        // constructor
    }
};

Student s;
```

The default constructor is used.

Thus, the result of default initialization depends on the type.

---

# 6.3.2 Value Initialization

Value initialization uses empty initializer syntax in contexts such as:

```cpp
int x{};
```

For a fundamental type, this results in zero:

```cpp
int x{};       // 0
double d{};    // 0.0
bool flag{};   // false
char c{};      // '\0'
```

This is one reason modern C++ commonly favors:

```cpp
int count{};
```

over:

```cpp
int count;
```

when a zero-initialized value is desired.

---

# 6.3.3 Zero Initialization

Zero initialization initializes an object or subobject to its zero value according to its type.

For fundamental types:

```cpp
int x{};       // 0
double d{};    // 0.0
bool b{};      // false
char c{};      // '\0'
```

For pointers:

```cpp
int* p{};      // null pointer
```

For class objects, zero initialization can also participate in certain initialization sequences, but it does not simply mean "fill every byte with zero" for every C++ type.

---

# 6.3.4 Static Storage and Zero Initialization

Objects with static storage duration are zero-initialized before other initialization takes place.

Example:

```cpp
int globalValue;
static int staticValue;
```

These objects are zero-initialized before dynamic/static initialization as applicable.

Conceptually:

```text
globalValue  → 0
staticValue  → 0
```

This is different from an uninitialized automatic local:

```cpp
void f()
{
    int localValue; // indeterminate
}
```

---

# 6.3.5 Copy Initialization

Copy initialization uses `=` in a declaration:

```cpp
int x = 10;
```

It is called copy initialization because the syntax conceptually initializes the object from the initializer.

Examples:

```cpp
int x = 10;
double price = 99.5;
char grade = 'A';
```

It can also initialize class objects:

```cpp
std::string name = "Deep";
```

Copy initialization and direct initialization have different language rules, particularly for constructors and conversions.

---

# 6.3.6 Direct Initialization

Direct initialization places the initializer after the type without `=`.

```cpp
int x(10);
```

For class types:

```cpp
std::string name("Deep");
```

Another example:

```cpp
std::vector<int> values(5, 100);
```

This creates a vector containing five elements with value `100`.

Compare:

```cpp
std::vector<int> a(5, 100);
```

with:

```cpp
std::vector<int> b{5, 100};
```

The first uses parentheses and the second uses list initialization, which can select different constructors.

---

# 6.3.7 List Initialization

List initialization uses braces:

```cpp
int x{10};
```

It is one of the most important modern C++ initialization forms.

Examples:

```cpp
int x{10};
double d{3.14};
std::string name{"Deep"};
```

---

## Narrowing Conversion Protection

Braced initialization generally rejects narrowing conversions.

```cpp
int x{3.14}; // ERROR: narrowing conversion
```

Whereas:

```cpp
int x = 3.14;
```

is allowed, although the fractional part is lost.

This makes braces useful for catching certain conversion mistakes at compile time.

---

# 6.3.8 Empty List Initialization

```cpp
int x{};
```

For an `int`, `x` becomes `0`.

```cpp
double d{};
bool b{};
char c{};
```

These are initialized to their zero/false values.

---

# 6.3.9 List Initialization of Containers

```cpp
std::vector<int> numbers{10, 20, 30, 40};
```

This creates a vector containing:

```text
10
20
30
40
```

Similarly:

```cpp
std::array<int, 3> values{1, 2, 3};
```

---

# 6.3.10 Aggregate Initialization

An **aggregate** can be initialized from a brace-enclosed list of element initializers.

Example:

```cpp
struct Point
{
    int x;
    int y;
};

Point p{10, 20};
```

Result:

```text
p.x = 10
p.y = 20
```

This is aggregate initialization.

---

# 6.3.11 Aggregate Initialization with Designated Initializers

C++20 supports designated initialization for aggregates.

```cpp
struct Point
{
    int x;
    int y;
};

Point p{.x = 10, .y = 20};
```

The designators must follow the declaration order for the members in C++.

Example:

```cpp
Point p{.x = 10, .y = 20}; // OK
```

Do not treat C++ designated initialization as identical to C's rules.

---

# 6.3.12 Nested Aggregate Initialization

```cpp
struct Address
{
    int houseNumber;
    int pin;
};

struct Person
{
    int age;
    Address address;
};

Person p{25, {10, 700001}};
```

The nested braces initialize the nested aggregate.

---

# 6.3.13 Initialization of Arrays

Arrays can be initialized using braces:

```cpp
int values[5]{1, 2, 3, 4, 5};
```

If fewer initializers are supplied, remaining elements are initialized appropriately:

```cpp
int values[5]{1, 2};
```

Conceptually:

```text
values[0] = 1
values[1] = 2
values[2] = 0
values[3] = 0
values[4] = 0
```

---

# 6.3.14 Common Initialization Comparison

```cpp
int a;          // default initialization; indeterminate for local automatic int
int b{};        // value initialization; 0
int c = 10;     // copy initialization
int d(10);      // direct initialization
int e{10};      // direct-list initialization
int f = {10};   // copy-list initialization
```

For simple integer values, many produce the same final value, but their language rules differ.

---

# 6.3.15 Initialization vs Assignment

Consider:

```cpp
int x = 10;
x = 20;
```

Timeline:

```text
Declaration + definition + initialization
                ↓
          x starts as 10
                ↓
             assignment
                ↓
          x becomes 20
```

Do not say:

> `x = 10` is assignment.

In:

```cpp
int x = 10;
```

the `=` is part of **copy-initialization**.

---

# 6.3.16 Initialization of `const`

```cpp
const int maxValue = 100;
```

After initialization:

```cpp
maxValue = 200; // ERROR
```

Braced initialization:

```cpp
const int maxValue{100};
```

---

# 6.3.17 Initialization of References

```cpp
int x = 10;

int& ref = x;
const int& cref = x;
```

The reference is initialized to refer to `x`.

References cannot normally be declared without an initializer:

```cpp
int& ref; // ERROR
```

---

# 6.3.18 Initialization of Pointers

```cpp
int x = 10;

int* p = &x;
```

Prefer explicit null initialization:

```cpp
int* p = nullptr;
```

or:

```cpp
int* p{};
```

Avoid:

```cpp
int* p = 0;
```

when `nullptr` expresses the intent more clearly.

---

# 6.3.19 Initialization and `auto`

The initializer affects type deduction.

```cpp
auto a = 10;    // int
auto b = 3.14;  // double
```

Braces require special attention:

```cpp
auto x{10};     // int
```

But:

```cpp
auto y = {10, 20, 30};
```

deduces an `std::initializer_list<int>`.

Therefore, `auto` plus braces should be used deliberately.

---

# 6.4 Variable Categories

Variables can be categorized in several overlapping ways.

Important categories include:

- local variables,
- global variables,
- static variables,
- thread-local variables,
- member variables.

These categories describe where a variable is declared and/or how long it exists. They are related to, but not identical to, concepts such as **scope**, **storage duration**, and **linkage**.

---

# 6.4.1 Local Variables

A local variable is declared within a block, function, or other local scope.

Example:

```cpp
void calculate()
{
    int total = 100;
}
```

`total` is local to the function/block.

Its scope is limited to the relevant block.

```cpp
void calculate()
{
    int total = 100;

    std::cout << total; // OK
}

std::cout << total; // ERROR
```

---

# 6.4.2 Block Scope

Variables declared inside a block have block scope.

```cpp
{
    int x = 10;
    std::cout << x;
}

// x is not accessible here
```

Nested blocks create nested scopes:

```cpp
int x = 10;

{
    int y = 20;

    {
        int z = 30;
    }
}
```

---

# 6.4.3 Local Variable Lifetime

A typical automatic local variable has automatic storage duration.

```cpp
void test()
{
    int x = 10;
}
```

Conceptually:

```text
enter function
    ↓
create x
    ↓
use x
    ↓
leave scope
    ↓
x's lifetime ends
```

For class objects, destruction occurs when their lifetime ends.

---

# 6.4.4 Local Static Variables

A local variable can be declared `static`.

```cpp
void counter()
{
    static int count = 0;
    ++count;

    std::cout << count << '\n';
}
```

Calling:

```cpp
counter();
counter();
counter();
```

prints:

```text
1
2
3
```

The important distinction is:

- **scope**: local to the function/block
- **storage duration**: static; it exists for the entire program execution

So `static` does not mean "global scope".

---

# 6.4.5 Global Variables

A variable declared at namespace scope is commonly called a global variable.

```cpp
int globalCount = 100;

int main()
{
    std::cout << globalCount;
}
```

Its scope begins according to the declaration and namespace rules.

A namespace-scope variable can have different linkage depending on how it is declared.

---

# 6.4.6 Global Variable Storage Duration

A namespace-scope variable normally has static storage duration.

Example:

```cpp
int globalValue = 10;
```

It exists for the duration of the program.

This is separate from whether the variable is accessible from another translation unit.

---

# 6.4.7 Global Variables and `extern`

`extern` can declare a variable whose definition is provided elsewhere.

File 1:

```cpp
int total = 100;
```

File 2:

```cpp
extern int total;
```

The second declaration refers to the object defined elsewhere.

This is useful when working across translation units.

Modern C++ projects often prefer namespaces, encapsulation, and carefully designed interfaces instead of unrestricted global mutable state.

---

# 6.4.8 Namespace-Scope `const`

A namespace-scope `const` variable has special linkage rules.

For example:

```cpp
const int maxSize = 100;
```

At namespace scope, a non-`volatile` non-`extern` const variable generally has internal linkage.

If sharing across translation units is intended, common modern approaches include:

```cpp
inline constexpr int maxSize = 100;
```

in a header, or an appropriate `extern` declaration/definition design.

---

# 6.4.9 Static Variables

`static` has different effects depending on where it appears.

### Local variable

```cpp
void f()
{
    static int count = 0;
}
```

This gives the variable static storage duration while keeping local scope.

### Namespace-scope variable

Historically:

```cpp
static int value = 10;
```

at namespace scope gives the name internal linkage.

For modern C++, an unnamed namespace is often preferred for internal namespace-scope entities:

```cpp
namespace
{
    int value = 10;
}
```

However, `static` remains valid C++ and has important legacy and specialized uses.

---

# 6.4.10 Static Storage Duration

An object with static storage duration exists throughout the execution of the program.

Examples include:

```cpp
int globalValue = 10;

static int fileValue = 20;

void f()
{
    static int localValue = 30;
}
```

These objects have static storage duration, though their scopes and linkage can differ.

---

# 6.4.11 Static Local Variable Initialization

Consider:

```cpp
void f()
{
    static int count = expensiveFunction();
}
```

The local static is initialized the first time control passes through its declaration, and its initialization is performed only once.

For a local static with dynamic initialization, C++ provides thread-safe initialization semantics.

Example:

```cpp
int createId()
{
    static int id = generateId();
    return id;
}
```

The static object remains alive after the function returns.

---

# 6.4.12 Thread-Local Variables

A thread-local variable has **thread storage duration**.

Use the `thread_local` specifier:

```cpp
thread_local int counter = 0;
```

Each thread gets its own instance.

Conceptually:

```text
Thread 1 → counter = 10
Thread 2 → counter = 20
Thread 3 → counter = 30
```

They are separate objects.

---

# 6.4.13 Local `thread_local`

A `thread_local` variable can appear at namespace scope and in other permitted contexts.

Example:

```cpp
void process()
{
    thread_local int count = 0;
    ++count;
}
```

Each thread has its own `count`.

Calls from the same thread see that thread's value.

---

# 6.4.14 `thread_local` vs `static`

These describe different storage-duration concepts.

```cpp
static int a = 0;
thread_local int b = 0;
```

`a` has static storage duration.

`b` has thread storage duration.

A `thread_local` variable can also be declared `static` or `extern` in appropriate contexts, so these keywords should not be treated as mutually exclusive in every situation.

---

# 6.4.15 Member Variables

A member variable is a non-static data member declared inside a class or struct.

Example:

```cpp
class Student
{
public:
    std::string name;
    int age;
};
```

Here:

```cpp
name
age
```

are non-static data members.

Each `Student` object normally has its own `name` and `age`.

```cpp
Student a;
Student b;
```

Conceptually:

```text
a
┌──────────────┐
│ name         │
│ age          │
└──────────────┘

b
┌──────────────┐
│ name         │
│ age          │
└──────────────┘
```

Changing `a.age` does not normally change `b.age`.

---

# 6.4.16 Data Member Initialization

Members can be initialized using a constructor member-initializer list:

```cpp
class Student
{
    std::string name;
    int age;

public:
    Student(std::string n, int a)
        : name(n), age(a)
    {
    }
};
```

This initializes the members as part of constructing the object.

Modern C++ also supports default member initializers:

```cpp
class Student
{
    std::string name{"Unknown"};
    int age{0};
};
```

These provide default initialization for the members unless overridden by the constructor's initialization.

---

# 6.4.17 Initialization Order of Members

Non-static data members are initialized in the order of their declaration in the class, **not** the order written in the constructor initializer list.

Example:

```cpp
class Test
{
    int a;
    int b;

public:
    Test()
        : b(20), a(10)
    {
    }
};
```

The actual initialization order is:

```text
a
↓
b
```

because `a` is declared first.

Therefore, write constructor initializers in declaration order.

---

# 6.4.18 Static Data Members

A static data member belongs to the class rather than to each individual object.

```cpp
class Student
{
public:
    static int count;
};
```

There is one shared `Student::count` object for the class, subject to the usual rules for its definition and linkage.

Modern C++ can use an `inline` static data member:

```cpp
class Student
{
public:
    inline static int count = 0;
};
```

This allows the definition to appear in the class definition in a header without requiring a separate definition in one source file.

---

# 6.4.19 Non-Static vs Static Data Members

| Feature | Non-static member | Static data member |
|---|---|---|
| Belongs to | Each object | Class |
| Number | Usually one per object | Shared class-level object |
| Access | `object.member` | `Class::member` or object access |
| Storage | Part of object representation, subject to layout rules | Separate object |
| Example | `int age;` | `inline static int count;` |

Example:

```cpp
class Student
{
public:
    int age;
    inline static int count = 0;
};
```

Each object has its own `age`, but `count` is shared.

---

# 6.4.20 `const` Member Variables

A data member can be `const`:

```cpp
class Student
{
    const int id;

public:
    Student(int value)
        : id(value)
    {
    }
};
```

Because `id` cannot be assigned after initialization, it must be initialized during object construction.

---

# 6.4.21 `mutable` Data Members

A `mutable` non-static data member can be modified even when the containing object is accessed through a const member function/object context where ordinary members cannot be modified.

```cpp
class Cache
{
    mutable int accessCount{0};

public:
    void read() const
    {
        ++accessCount;
    }
};
```

`mutable` is useful for logically-const operations such as caching or instrumentation.

Use it carefully.

---

# 6.4.22 Static, Global, Local, Member — Comparison

| Category | Typical location | Typical storage duration | Scope |
|---|---|---|---|
| Local automatic | Block/function | Automatic | Local/block |
| Local static | Block/function | Static | Local/block |
| Global/namespace variable | Namespace | Static | Namespace |
| Thread-local | `thread_local` declaration | Thread | Depends on scope |
| Non-static member | Class | Part of containing object's lifetime | Class/member access |
| Static data member | Class | Static | Class/member |

---

# 6.4.23 Scope vs Lifetime vs Storage Duration

These terms must not be confused.

## Scope

Where the **name** can be used.

## Lifetime

The period during which the **object exists**.

## Storage duration

The C++ language classification describing how long storage for an object lasts.

Main storage-duration categories:

- automatic,
- static,
- thread,
- dynamic.

Example:

```cpp
void f()
{
    static int x = 0;
}
```

Here:

```text
Scope:
    local to the function/block

Storage duration:
    static

Lifetime:
    throughout the program execution
```

---

# 6.4.24 Automatic Storage Duration

Typical local variables have automatic storage duration:

```cpp
void f()
{
    int x = 10;
}
```

`x` comes into existence as execution reaches its declaration and its lifetime ends when the relevant scope is exited, subject to C++ lifetime rules.

---

# 6.4.25 Dynamic Storage Duration

Objects created dynamically have dynamic storage duration.

```cpp
int* p = new int(10);
```

The allocated object remains alive until it is destroyed:

```cpp
delete p;
```

Modern C++ generally prefers RAII and smart pointers:

```cpp
auto p = std::make_unique<int>(10);
```

This avoids manual `delete` in ordinary code.

Dynamic storage duration is a storage-duration category, but dynamically allocated objects are not usually what we mean by "variables" in the simple introductory sense.

---

# 6.4.26 Storage Duration Summary

| Storage duration | Typical examples |
|---|---|
| Automatic | Local non-static variables |
| Static | Global/namespace variables, local static variables |
| Thread | `thread_local` variables |
| Dynamic | Objects created by dynamic allocation |

---

# 6.4.27 Linkage

Linkage determines whether declarations in different scopes or translation units can refer to the same entity.

Major concepts include:

- no linkage,
- internal linkage,
- external linkage,
- module linkage in modern C++ contexts.

Example:

```cpp
int globalValue = 10;
```

A namespace-scope non-const variable generally has external linkage unless another rule changes that.

Example:

```cpp
static int fileValue = 20;
```

At namespace scope, `static` gives internal linkage.

`extern` can be used to refer to an entity with external linkage defined elsewhere.

---

# 6.4.28 Variable Categories Are Not Exclusive

A variable can fit multiple descriptions.

Example:

```cpp
void f()
{
    static thread_local int x = 0;
}
```

The exact legal combinations depend on context, but the important lesson is that labels such as "local", "static", and "thread-local" describe different properties.

Similarly:

```cpp
class A
{
    int x;
};
```

`x` is:

- a data member,
- non-static,
- associated with each `A` object,
- and has a lifetime tied to the containing object.

---

# 6.5 Declaration, Definition, Initialization, Assignment — One Example

Consider:

```cpp
extern int total;  // declaration

int total = 100;   // definition + initialization

total = 200;       // assignment
```

The sequence is:

```text
Declaration
    ↓
Definition
    ↓
Initialization
    ↓
Object exists
    ↓
Assignment
```

---

# 6.6 Complete Initialization Examples

## Fundamental types

```cpp
int a{};             // 0
int b = 10;          // copy initialization
int c(20);           // direct initialization
int d{30};           // direct-list initialization
int e = {40};        // copy-list initialization

double price{99.99};
char grade{'A'};
bool active{true};
```

---

## Class types

```cpp
std::string name{"Deep"};
std::vector<int> numbers{1, 2, 3};
```

---

## Pointer

```cpp
int value{10};
int* p{&value};
int* nullPointer{};
```

---

## Reference

```cpp
int value{10};
int& ref{value};
```

---

## Constant

```cpp
const int maxUsers{100};
```

---

## Compile-time constant

```cpp
constexpr int bufferSize{1024};
```

---

# 6.7 Common Mistakes

## Mistake 1: Confusing initialization and assignment

Wrong terminology:

```cpp
int x = 10; // "assignment"
```

Better:

```cpp
int x = 10; // initialization
x = 20;     // assignment
```

---

## Mistake 2: Reading an uninitialized local

```cpp
int x;

std::cout << x; // BAD
```

Use:

```cpp
int x{};
```

---

## Mistake 3: Starting an identifier with a digit

```cpp
int 2value; // ERROR
```

Use:

```cpp
int value2;
```

---

## Mistake 4: Using a keyword as a variable name

```cpp
int class; // ERROR
```

---

## Mistake 5: Thinking `static` always means global

This:

```cpp
void f()
{
    static int count{};
}
```

has local scope but static storage duration.

---

## Mistake 6: Assuming all globals are externally visible

Namespace-scope linkage rules matter.

For example:

```cpp
static int value{};
```

at namespace scope has internal linkage.

---

## Mistake 7: Assuming `char` is always signed

```cpp
char c = ...;
```

Whether plain `char` behaves as signed or unsigned is implementation-defined.

If signedness matters, use:

```cpp
signed char
```

or:

```cpp
unsigned char
```

---

## Mistake 8: Confusing `const` and `constexpr`

```cpp
const int x = getValue();
```

may be a runtime-initialized constant object.

```cpp
constexpr int x = 10;
```

requires a constant expression initializer.

---

## Mistake 9: Assuming member initialization follows initializer-list order

```cpp
class A
{
    int x;
    int y;

public:
    A() : y(20), x(10) {}
};
```

Actual order:

```text
x
y
```

because members are initialized in declaration order.

---

# 6.8 Best Practices

## Prefer initialization at declaration

Prefer:

```cpp
int count{};
```

over:

```cpp
int count;
count = 0;
```

---

## Prefer braces when appropriate

```cpp
int value{42};
```

Braces provide narrowing-conversion checks.

---

## Prefer `nullptr`

Use:

```cpp
int* p{};
```

or:

```cpp
int* p = nullptr;
```

instead of:

```cpp
int* p = 0;
```

---

## Use descriptive names

Prefer:

```cpp
double accountBalance{};
```

over:

```cpp
double x{};
```

when the meaning is not obvious.

---

## Minimize mutable global state

Global mutable variables can make:

- dependencies unclear,
- testing harder,
- concurrency harder,
- initialization order more complicated.

Prefer encapsulation and controlled interfaces.

---

## Use `constexpr` for compile-time constants

```cpp
constexpr int maxRetries = 3;
```

when the value is genuinely a compile-time constant.

---

## Use RAII for resources

Do not normally manage resources with raw `new`/`delete`.

Prefer:

```cpp
auto ptr = std::make_unique<MyClass>();
```

when dynamic ownership is required.

---

# 6.9 Quick Reference Table

| Syntax | Meaning |
|---|---|
| `int x;` | Definition; default initialization |
| `int x{};` | Value initialization |
| `int x = 10;` | Copy initialization |
| `int x(10);` | Direct initialization |
| `int x{10};` | Direct-list initialization |
| `int x = {10};` | Copy-list initialization |
| `const int x{10};` | Const object |
| `constexpr int x{10};` | Compile-time constant expression object |
| `auto x = 10;` | Type deduction |
| `int& r = x;` | Reference |
| `int* p = &x;` | Pointer |
| `int* p{};` | Null pointer |
| `static int x{};` | Static storage duration |
| `thread_local int x{};` | Thread storage duration |
| `int x{};` inside class | Non-static data member |
| `inline static int x{};` inside class | Static data member |

---

# 6.10 Exam/Interview Questions

## Basic

1. What is a variable?
2. What is the difference between declaration and definition?
3. What is initialization?
4. What is assignment?
5. What is the difference between initialization and assignment?
6. What are the rules for valid C++ identifiers?
7. Is C++ case-sensitive?
8. What is variable scope?
9. What is a local variable?
10. What is a global variable?

## Initialization

11. What is default initialization?
12. What is value initialization?
13. What is zero initialization?
14. What is copy initialization?
15. What is direct initialization?
16. What is list initialization?
17. What is aggregate initialization?
18. Why is brace initialization useful?
19. What is a narrowing conversion?
20. What happens to missing elements in aggregate/array initialization?

## Storage and categories

21. What is an automatic variable?
22. What is a static local variable?
23. What is a thread-local variable?
24. What is a member variable?
25. What is a static data member?
26. What is the difference between static storage duration and local scope?
27. What is the difference between scope and lifetime?
28. What is storage duration?
29. What is linkage?
30. What does `extern` do?

---

# 6.11 Interview-Level Questions with Answers

## Q1. What is the difference between declaration and definition?

A declaration introduces a name and type. A definition provides the entity itself.

```cpp
extern int x; // declaration

int x = 10;   // definition + initialization
```

---

## Q2. Is `int x = 10;` assignment?

No.

It is copy initialization.

```cpp
int x = 10; // initialization
x = 20;     // assignment
```

---

## Q3. What happens with `int x;` inside a function?

For a local automatic `int`, the object is default-initialized and has an indeterminate value. Reading it before assigning/initializing a valid value is erroneous.

Prefer:

```cpp
int x{};
```

when zero initialization is wanted.

---

## Q4. What is the difference between `int x{}` and `int x;`?

```cpp
int x{};
```

value-initializes `x`, giving an `int` the value `0`.

```cpp
int x;
```

default-initializes the local automatic `int`, leaving it with an indeterminate value.

---

## Q5. What does `static` do to a local variable?

Example:

```cpp
void f()
{
    static int count{};
}
```

The name has local/block scope, but the object has static storage duration and persists between function calls.

---

## Q6. What is a thread-local variable?

A variable declared with `thread_local` has thread storage duration. Each thread has its own instance.

```cpp
thread_local int counter{};
```

---

## Q7. What is a member variable?

A non-static data member is declared inside a class/struct and normally exists as part of each object.

```cpp
struct Student
{
    int age;
};
```

Each `Student` object has its own `age`.

---

## Q8. What is a static data member?

A static data member belongs to the class rather than each individual object.

```cpp
struct Student
{
    inline static int count{};
};
```

There is one shared `count` object for the class.

---

## Q9. Why prefer `{}` initialization?

Example:

```cpp
int x{3.14}; // compile-time error: narrowing
```

Braced initialization can prevent unintended narrowing conversions.

---

## Q10. Are scope and lifetime the same?

No.

Scope describes where a name can be used.

Lifetime describes when an object exists.

Example:

```cpp
void f()
{
    static int x{};
}
```

`x` has local scope but static storage duration and a lifetime that lasts for the program execution.

---

# 6.12 Revision Notes

### Declaration

```cpp
extern int x;
```

Introduces a declaration without defining the object.

### Definition

```cpp
int x;
```

Defines an object.

### Initialization

```cpp
int x{10};
```

Gives the object its initial value.

### Assignment

```cpp
x = 20;
```

Changes the value of an existing object.

### Local

```cpp
void f()
{
    int x{};
}
```

### Global / namespace-scope

```cpp
int x{};
```

### Static local

```cpp
void f()
{
    static int x{};
}
```

### Thread-local

```cpp
thread_local int x{};
```

### Member

```cpp
struct A
{
    int x{};
};
```

---

# 6.13 One-Page Mental Model

Think about every variable using these questions:

```text
1. What is its TYPE?
       ↓
2. What is its NAME?
       ↓
3. Where is it DECLARED?
       ↓
4. Is this a DECLARATION or DEFINITION?
       ↓
5. How is it INITIALIZED?
       ↓
6. Can it be ASSIGNED later?
       ↓
7. What is its SCOPE?
       ↓
8. What is its STORAGE DURATION?
       ↓
9. What is its LIFETIME?
       ↓
10. What is its LINKAGE?
```

Example:

```cpp
void process()
{
    static int count{0};
    ++count;
}
```

Analysis:

```text
Name:
    count

Type:
    int

Definition:
    Yes

Initialization:
    value initialization with 0

Scope:
    local/block scope

Storage duration:
    static

Lifetime:
    entire program execution

Assignment:
    possible because it is not const
```

---

# 6.14 Final Summary

C++ variables are best understood by separating several concepts.

### Declaration

Introduces a name and its type:

```cpp
extern int x;
```

### Definition

Defines the object:

```cpp
int x;
```

### Initialization

Gives the object its initial state:

```cpp
int x{10};
```

### Assignment

Changes an existing object's value:

```cpp
x = 20;
```

### Initialization forms

```cpp
int a;          // default initialization
int b{};        // value initialization
int c = 10;     // copy initialization
int d(10);      // direct initialization
int e{10};      // direct-list initialization
int f = {10};   // copy-list initialization
```

### Variable categories

```cpp
void f()
{
    int local{};              // local automatic
    static int persistent{};  // local static
}

int global{};                 // namespace-scope/global

thread_local int tls{};       // thread-local

struct A
{
    int member{};             // non-static data member
    inline static int shared{}; // static data member
};
```

The most important principle is:

> **Do not confuse name/scope, object lifetime, storage duration, initialization, assignment, and linkage. They are different properties of a C++ object.**

---

# 6.15 Cheat Sheet

```cpp
// Declaration
extern int x;

// Definition
int x;

// Initialization
int a{10};

// Assignment
a = 20;

// Local variable
void f()
{
    int local{};
}

// Static local
void g()
{
    static int count{};
}

// Namespace/global variable
int globalValue{};

// Thread-local
thread_local int threadValue{};

// Non-static member
struct User
{
    int age{};
};

// Static member
struct User2
{
    inline static int count{};
};

// Constant
const int maxValue{100};

// Compile-time constant
constexpr int bufferSize{1024};

// Pointer
int* p{};

// Reference
int value{};
int& ref{value};
```

**End of Chapter 6 — Variables**
