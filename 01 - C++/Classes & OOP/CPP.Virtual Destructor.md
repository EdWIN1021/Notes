```cpp
#include <iostream>

class Base {
public:
    Base() { std::cout << "Base constructor\n"; }
    virtual ~Base() { std::cout << "Base destructor\n"; } // Virtual destructor
};

class Derived : public Base {
public:
    Derived() { std::cout << "Derived constructor\n"; }
    ~Derived() { std::cout << "Derived destructor\n"; }
};

int main() {
    Base* b = new Derived();
    delete b; // Correctly calls Derived's destructor followed by Base's destructor
    return 0;
}

```