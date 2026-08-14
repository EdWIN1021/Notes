```cpp
vector<double> scores{ 88, 50, 90, 60, 70 };

vector<double>::iterator pd = scores.end();
auto pd = scores.end();

*(pd - 1) // 70
```