```ad-warning
using delete to deallocate storage
```

```cpp
StringBad::~StringBad(){ 
  delete[] str; 
}
```