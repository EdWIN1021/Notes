```ad-important
Points to an object of type T on the heap
It is not unique
Can be assigned and copied
can be moved
Does not support managing arrays by default
When use count is zero, the managed object on the heap is destroyed
```


```cpp
#include <memory> 
using namespace std;

shared_ptr<int> p1 {new int {100}};
shared_ptr<int> p2 { p1 }; // shared ownership

shared_ptr<int> p1 = make_shared<int>(100);

p1.use_count(); // 2
p1.reset(); // 0
```
