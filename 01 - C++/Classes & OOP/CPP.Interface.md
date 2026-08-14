```cpp
class Printable {
  friend ostream &operator<<(ostream &, const Printable &obj);
public:
  virtual void print(ostream &os) const = 0;
  virtual ~Printable(){};
}
ostream &operator<<(ostream &os, const Printable &obj){
  obj.print(os);
  return os;
}
```