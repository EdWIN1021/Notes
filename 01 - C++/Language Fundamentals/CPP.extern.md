## extern
- is a declaration in C or C++ that tells the compiler that the variable `i` exists, but it is defined elsewhere, possibly in another file or later in the same file
- It is an error to provide an initializer on an extern inside a function.
```cpp title=PI.h
#ifndef PI_H
#define PI_H

extern double pi; 
extern const int bufsize();

#endif

```

```cpp title=PI.cpp
#include "pi.h"

double pi = 3.1416;
const int bufSize = fcn();
```