```cpp
vector<int> old_v { 88, 50, 90, 60, 70 };
vector<int> new_v { 1, 2, 3, 4, 5 };

old_v.insert(old_v.begin(), new_v.begin() + 1, new_v.end()); // 2, 3, 4, 5, 88, 50, 90, 60, 70,
```