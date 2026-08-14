---
tags:
---
```cpp
typedef double wages;      // wages is a synonym for double 
wages hourly, weekly;      // same as double hourly, weekly 
typedef wages base, *p     // base is a synonym for double, p for double*

typedef char *pstring;
const pstring cstr = 0;
const pstring *ps; //ps is a pointer to a constant pointer to char
```