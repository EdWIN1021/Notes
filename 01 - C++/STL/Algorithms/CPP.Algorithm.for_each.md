## function pointer
```cpp
void square(int x) {
  std::cout << x * x << "";
}

std::vector<int> vec {1, 2, 3, 4};
std::for_each( vec.begin(), vec.end(), square );
```

## lambda expression
```cpp
std::vector<int> vec {1, 2, 3, 4};

std::for_each( vec.begin(), vec.end(), []( int x ){
  std::cout << x * x << " ";
})
```
