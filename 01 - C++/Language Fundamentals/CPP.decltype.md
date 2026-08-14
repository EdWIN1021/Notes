> [!NOTE] Title
> decltype is a compile-time keywords in C++ (introduced in C ++ 11) that inspects the type of an expression without evaluating it.

```cpp
const int ci = 0, &cj = ci;

decltype ( ci ) x = 0;   // x has type const int
decltype ( cj ) y = x;   // y has type const int& and is bound to x
decltype ( cj ) z;       // z is a reference and must be initialized
```

```cpp
int i = 42, *p = &i, &r = i;
decltype (r + 0) b;              // b is an uninitialized int
```

if the expression is an lvalue decltype returns an lvalue reference to the type.
Since p can appear on the left-hand side of an assignment (*p = 50), it is an lvalue.

```cpp
decltype (*p) c;      // error: c is int& and must be initialized
```

When we apply decltype to a variable without any parentheses, we get the type of that variable.

If we wrap the variable’ name in one or more sets of parentheses, the compiler will evaluate the operand as an expression

```cpp
decltype ((i)) d; // error:: d is int& and must be initialized
```
### Implementation
[[Snippet.CPP.decltype]]
