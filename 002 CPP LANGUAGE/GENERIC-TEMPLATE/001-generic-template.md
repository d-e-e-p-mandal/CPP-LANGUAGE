# C++ Templates (`<T>`) — Complete Notes

## What are Templates?

A **Template** is a C++ feature that allows you to write **generic (type-independent)** code.

Instead of writing the same code for different data types, you write it once using a **template parameter**.

Templates are the foundation of **Generic Programming** in C++.

---

# Generic Programming

**Generic Programming** is a programming technique where code is written independently of any specific data type.

Instead of writing separate code for `int`, `double`, `char`, etc., one template works for all.

Example without templates:

```cpp
class IntBox
{
    int value;
};

class DoubleBox
{
    double value;
};

class StringBox
{
    string value;
};
```

The same code is repeated multiple times.

Using templates:

```cpp
template<class T>
class Box
{
    T value;
};
```

Now the same class works for every data type.

---

# Template Syntax

```cpp
template<class T>
class Box
{
};
```

or

```cpp
template<typename T>
class Box
{
};
```

Both are identical.

---

# Breaking Down the Syntax

```cpp
template<class T>
class Box
{
};
```

## 1. `template`

```cpp
template
```

Declares that the following class or function is a **template**.

---

## 2. `class T`

```cpp
class T
```

Declares a **Template Type Parameter**.

`T` is a placeholder for an actual data type.

Examples

```cpp
Box<int>
```

means

```text
T = int
```

```cpp
Box<double>
```

means

```text
T = double
```

---

## 3. `class Box`

```cpp
class Box
```

Defines a class named `Box`.

Notice that the class name is simply `Box`.

Correct

```cpp
template<class T>
class Box
{
};
```

Incorrect

```cpp
class Box<T>
{
};
```

---

# What is `T` Called?

Official C++ terminology:

- Template Type Parameter ✅
- Template Parameter
- Type Parameter

Common informal names:

- Generic Type Parameter
- Placeholder Type

---

# Is `T` a Keyword?

No.

`T` is just a normal identifier.

These are all valid:

```cpp
template<class Apple>
class Box
{
};
```

```cpp
template<class DataType>
class Box
{
};
```

```cpp
template<class X>
class Box
{
};
```

The name `T` is simply the standard convention because it stands for **Type**.

---

# Why Use Templates?

Without templates

```cpp
class IntBox
{
    int value;
};

class DoubleBox
{
    double value;
};
```

With templates

```cpp
template<class T>
class Box
{
    T value;
};
```

Now

```cpp
Box<int>

Box<double>

Box<char>

Box<string>
```

all work.

---

# Using a Class Template

```cpp
template<class T>
class Box
{
public:
    T value;
};
```

Create objects

```cpp
Box<int> a;

Box<double> b;

Box<string> c;
```

Compiler understands

```text
a → Box<int>

b → Box<double>

c → Box<string>
```

---

# Template Instantiation

When you write

```cpp
Box<int> b;
```

the compiler generates

```cpp
class Box<int>
{
    int value;
};
```

When you write

```cpp
Box<double> d;
```

compiler generates

```cpp
class Box<double>
{
    double value;
};
```

This process is called

> **Template Instantiation**

---

# Template Class Declaration

Only declaration

```cpp
template<class T>
class Box;
```

Meaning

> There exists a template class named `Box`.

No object can be created because the class body is missing.

---

# Template Class Definition

```cpp
template<class T>
class Box
{
private:
    T value;

public:
    void set(T v)
    {
        value = v;
    }

    T get()
    {
        return value;
    }
};
```

Now objects can be created.

```cpp
Box<int> box;
```

---

# Function Templates

Templates can also be used with functions.

```cpp
template<class T>
T maximum(T a, T b)
{
    return a > b ? a : b;
}
```

Usage

```cpp
maximum(10, 20);

maximum(2.5, 5.4);
```

Compiler creates

```cpp
maximum<int>()

maximum<double>()
```

---

# Multiple Template Parameters

```cpp
template<class T, class U>
class Pair
{
};
```

Example

```cpp
Pair<int, string>
```

Here

```text
T = int

U = string
```

---

# Default Template Parameters

```cpp
template<class T = int>
class Box
{
};
```

Now

```cpp
Box<>
```

means

```cpp
Box<int>
```

---

Example from STL

```cpp
template<
    class T,
    class Container = deque<T>
>
class stack;
```

Here

```
T = element type

Container = deque<T> by default
```

Writing

```cpp
stack<int>
```

is equivalent to

```cpp
stack<int, deque<int>>
```

---

# `class` vs `typename`

Both are exactly the same.

```cpp
template<class T>
```

and

```cpp
template<typename T>
```

are equivalent.

Modern C++ often prefers `typename` because it clearly indicates a type.

---

# Where is `Box<T>` Used?

Correct

```cpp
Box<int> obj;
```

```cpp
Box<double> obj;
```

```cpp
template<class T>
void Box<T>::print()
{
}
```

Incorrect

```cpp
class Box<T>
{
};
```

Reason

The template parameter must be declared before the class.

---

# Common Template Parameter Names

| Name | Meaning |
|------|---------|
| T | Type |
| U | Second Type |
| K | Key |
| V | Value |
| E | Element |
| N | Integer Constant |

---

# Templates vs Generics

| C++ | Java / C# |
|------|-----------|
| Templates | Generics |
| `template<class T>` | `<T>` |
| Class Template | Generic Class |
| Function Template | Generic Method |
| Template Type Parameter | Generic Type Parameter |

---

# Advantages of Templates

- Code reuse
- Type safety
- No duplicate code
- Better maintainability
- Compile-time polymorphism
- High performance (no runtime overhead)

---

# Disadvantages of Templates

- Longer compile time
- Larger executable size (code generation for each type)
- Complex compiler error messages
- Harder to debug

---

# Important Terminology

| Term | Meaning |
|------|---------|
| Template | A blueprint for generic code |
| Generic Programming | Writing code independent of data type |
| Template Parameter | Placeholder used inside a template |
| Template Type Parameter | Placeholder representing a type (`T`) |
| Template Argument | Actual type supplied (`int`, `double`) |
| Class Template | Template that creates classes |
| Function Template | Template that creates functions |
| Template Instantiation | Compiler generates code for a specific type |

---

# Flow Diagram

```text
template<class T>
        │
        ▼
Template Type Parameter (T)
        │
        ▼
Class Template
        │
        ▼
Box<int>
Box<double>
Box<string>
        │
        ▼
Compiler creates actual classes
        │
        ▼
Template Instantiation
```

---