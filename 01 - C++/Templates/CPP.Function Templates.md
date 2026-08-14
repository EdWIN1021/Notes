
```cpp title:
template <typename T>
T max(T a, T b){
  return (a > b) ? a : b; 
}

template <class T>
T max(T a, T b){
  return (a > b) ? a : b; 
}

int a {10};
int b {20};
max<int>(a,b);
```
