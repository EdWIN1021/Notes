
```ad-warning
You should initialize a reference variable when you declare it.
```

- A reference is a name that acts as an alias, or an alternative name, for a previously defined variables.

- if you use a reference as an argument, the function works with the original data instead of with a copy.

```cpp
int rats;
int &rodents = rats;
```

```cpp
const int ci = 1024;
const int &r1 = ci;

r1 = 42;       // error
int &r2 = ci;  // error
```


```cpp
const int &r2 = 42;
```


