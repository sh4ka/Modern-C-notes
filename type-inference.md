# Evolution of Type Deduction in C++ Standards

## C++98
- Introduced type deduction primarily through function templates.
- Function templates allow the compiler to deduce the types of the template parameters based on the types of the arguments passed to the function.

## C++11
- Enhanced type deduction with the introduction of two new features: `auto` and `decltype`.
  - `auto`: Enables the compiler to deduce the type of a variable from its initializer.
  - `decltype`: Determines the type of an expression.

## C++14
- Extended the contexts in which `auto` and `decltype` can be used.
- For example, `auto` can now be used for the type of a lambda expression.

## Impact on C++ Software
- The advancements in type deduction make C++ software more adaptable and maintainable.
- By reducing the need for explicit type declarations, the code becomes more concise and less error-prone.

# Type Deduction in Various Contexts

Type deduction in C++ takes place in several contexts:
1. **Function Templates**: When a function template is called, the compiler deduces the template arguments from the function arguments.
2. **`auto` Keyword**: Used for variable type deduction based on the initializer.
3. **`decltype` Expressions**: Used to deduce the type of an expression without evaluating it.
4. **`decltype(auto)`**: Combines the functionality of `decltype` with `auto`, deducing the type based on the initializer, including references.

# Type Deduction for Function Templates

There are 3 main cases, depending not only on the type of expr, but also on the form of ParamType.

Consider the following function template:
```cpp
template<typename T>
void f(ParamType param);

f(expr); // deduce T and ParamType from expr
```

**Case 1: When ParamType is a Reference Type or a Pointer, but Not a Universal Reference**

**Step 1:** If `expr` is a reference, ignore the reference to get the underlying type.  
**Step 2:** Determine `T` by pattern matching the type of `expr` against `ParamType`.

### Examples

#### When ParamType is a Reference Type:
```cpp
int x = 27;
const int cx = x;
const int& rx = x;

// Function template calls
f(x);  // T is int, ParamType is int&
f(cx); // T is const int, ParamType is const int&
f(rx); // T is const int, ParamType is const int&

// Function template calls
f(x);  // T is int, ParamType is int&
f(cx); // T is const int, ParamType is const int&
f(rx); // T is const int, ParamType is const int&
```

When passing x:
T is deduced as int.
ParamType is int&.
When passing cx:
T is deduced as const int.
ParamType is const int&.
When passing rx:
T is deduced as const int.
ParamType is const int&.
When ParamType is a Pointer:
```cpp
template<typename T>
void f(T* param); // param is now a pointer

int x = 27;
const int *px = &x; // px is a pointer to x as a const int

// Function template calls
f(&x); // T is int, ParamType is int*
f(px); // T is const int, ParamType is const int*
```

When passing &x:
T is deduced as int.
ParamType is int*.
When passing px:
T is deduced as const int.
ParamType is const int*.
