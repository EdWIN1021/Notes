```ad-important
A program creates only one copy of a static class variable, regardless of the number of objects created.
```

```cpp
//.h
private: 
	static int num_strings;
	
//.cpp
int StringBad::num_strings = 0;
```

