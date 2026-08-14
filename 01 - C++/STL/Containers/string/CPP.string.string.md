```cpp
#include <string> 
using namespace std;

std::string book("0-201-78345-X");

string s1;               // empty string
string s2 {"Edwin"};     // s2 = "Edwin"
string s3 {s2};          // s3 = s2
string s4 {"Edwin", 3}; 
string s5 {s3, 0, 2}; 
string s6 (3, 'X');      // XXX
```
