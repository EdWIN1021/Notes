```ad-important
A friend function is a function that is not a member of a class but has the ability to access the private and protected members of that class
```


```cpp
friend void Remote::set_chan(Tv & t, int c);
```