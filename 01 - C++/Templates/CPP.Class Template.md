```cpp
template <typename T>
class Item {

private:
  std::string name;
  T value;
  
public:
  Item(std::string name, T value):name{name}, value{value}{
  }

  std::string get_name() const { return name; }
  T get_value() const { return value; }

} 

Item<int> item1 {"Larry", 1};
Item<double> item2 {"House", 1000,0};

```