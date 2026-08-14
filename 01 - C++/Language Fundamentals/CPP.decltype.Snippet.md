```cpp
const int ci = 0, &cj = ci;

decltype(ci) x = 0; // x has type const int
decltype(cj) y = x; // y has type const int & and it bound to x
decltype(cj) z; // error


int i = 42, *p = &i, &r = i;  
decltype(r+0)b; // ok:additionyieldsanint;bisan(uninitialized)int
decltype(*p)c; // error:cisint&andmustbeinitialized

decltype((i)) d; // error: d is int& and must be initialized
```
### Concept
[[Concept.CPP.decltype]]
