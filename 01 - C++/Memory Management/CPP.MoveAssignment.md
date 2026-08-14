```cpp
Mystring &operator=(Mystring &&rhs);

Mystring &Mystring::operator=(Mystring &&rhs){
  if(this == &rhs){
    return *this;
  }
  
  delete[] str;
  str = rhs.str;
  rhs.str = nullptr;
  
  return *this;
}

Mystring a {"Hello"};
a = Mystring{"Hola"};
a = "Bonjour";
```