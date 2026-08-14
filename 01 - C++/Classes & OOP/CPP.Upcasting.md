```ad-important
Upcasting refers to the conversion of a derived class pointer or reference to a base class pointer or reference.
```


```cpp
void fr(Brass &rb);
void fp(Brass *pb);
void fv(Brass b);

Brass b {"Billy Bee", 123432, 10000.0};
BrassPlus bp {"Betty Beep", 232313, 12345.0};

fr(b);
fr(bp);

fp(b);
fp(bp);

fv(b);
fv(bp);
```