```ad-important
This works not by passing arguments but through text substitution, with the X acting as a symbolic label for the “argument”
```

```cpp
#define SQUARE(X) X*X
double a = SQUARE(0.5);

#define PI 3.14159
```

