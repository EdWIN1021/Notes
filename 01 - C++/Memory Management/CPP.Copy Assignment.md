```cpp
Mystring &operator=(const Mystring &rhs);

Mystring &Mystring::operator=(const Mystring &rhs){
 if(this == &rhs){
   return *this;
 }

 delete [] this->str;
 str = new char[strlen(rhs.str) + 1];
 strcpy(this->str, rhs.str);

 return *this;
}

Mystring a {"Hello"};
Mystring b;

b = a;
b = "This is a test";
```