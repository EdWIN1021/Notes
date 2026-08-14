- An abstract class typically contains at least one pure virtual function
- Cannot be instantiated

```cpp
class Shape {

public:
  virtual draw()   = 0;
  virtual rotate() = 0;
  virtual ~Shape(){}  
};
```
