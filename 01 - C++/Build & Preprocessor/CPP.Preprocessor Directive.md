```ad-important
- preprocessor is a program that runs before the compiler.
- Preprocessor variables, including names of header guards, must be unique throughout the program.
```

```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H

#include <string>

struct Sales_data {
  std::string bookNo;
  unsigned units_sold = 0;
  double revenue = 0.0;
};

#endif
```