> [!info] if c is a lowercase letter, returns its uppercase equivalent

```cpp
#include <cctype>

string str = "Hello World!";  
for(auto &c : str) {  
    c = toupper(c);  
}

// str: HELLO WORLD!
```