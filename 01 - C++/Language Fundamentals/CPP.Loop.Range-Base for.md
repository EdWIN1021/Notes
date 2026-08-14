- ! The body of a range for must not change the size of the sequence over which it is iterating

```cpp
string s{ "some string" };

for(auto c : s) {
  cout << c << endl;
}
```


## 2D array
```cpp
int arr[3][4] = {  
    {1, 2, 3, 4},  
    {5, 6, 7, 8},  
    {9, 10, 11, 12}  
};  
  
for (const auto& row : arr) {              
    for (const auto& elem : row) {          
        std::cout << elem << " ";  
    }    
    std::cout << std::endl;  
}
```