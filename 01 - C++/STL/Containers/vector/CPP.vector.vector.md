## size_type
```cpp
vector<int>::size_type;
```


```cpp
#include <vector> 
using namespace std; 

vector<char> vowels; 
vector<char> vowels(5); // 5 0s
vector<char> vowels {'a', 'e', 'i', 'o', 'u'};
vector<Shape*> shapes;
vector<vector<string>> file;
vector<int> vec (10, -1); // 10 int elements, each initialized to -1
```

## Copy Vector
```cpp
vector<int> vec1;
vector<int> vec2(vec);
vector<int> vec3 = vec;
```