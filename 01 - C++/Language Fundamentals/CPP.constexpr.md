```ad-info
- A constant expression is an expression whose value cannot change and that can be evaluated at compile time.

- The types we can use in a constexpr are known as “literal types” because they are simple enough to have literal values.
You need constants for template parameters or array sizes.
```

```cpp
constexpr int mf = 20;
constexpr int limit = mf + 1; 
constexpr int sz = size(); 
```

```cpp
constexpr int square(int x) {
  return x * x; 
}
```

```cpp
constexpr int *q = nullptr; // q is a const pointer to int
constexpr const int *p = &i; // p is a constant pointer to the const int i
```