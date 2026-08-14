```cpp
#include <array>

std::array<int, 5> arr1 { {1, 2, 3, 4, 5} };
std::array<std::string, 3> stooges {
  std::string("Larry"),
  "Moe",
  std::string("Curly")
};

arr1 = { 2, 4, 6, 8, 10 };

```