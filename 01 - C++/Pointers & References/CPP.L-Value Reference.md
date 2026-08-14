## Reference to Int

```cpp
int x { 100 };
int &l_ref = x;
l_ref = 10;
```

## Reference to Pointer

```cpp
int i = 42;
int *p = &i;
int *&r = p;

/*
This is allowed. Here, 'r' and 'p' are essentially the same, 
so this sets 'p' to point to 'i'.
*/
r = &i;

*r = 0; // i = 0
```