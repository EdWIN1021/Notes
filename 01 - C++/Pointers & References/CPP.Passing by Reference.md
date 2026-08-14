```cpp
int a {10};
int b {20};

swap(a, b);

void swap(int &a, int &b){ 
  int temp = a;
  a = b;
  b = temp;
}
```

## const reference
```cpp
double refcube(const double &ra);
```
