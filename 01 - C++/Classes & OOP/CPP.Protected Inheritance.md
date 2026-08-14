```ad-important
With protected inheritance, public and protected members of a base class become protected members of the derived class
```

```cpp
class Student:protected std::string, protected std::valarray<double>
```