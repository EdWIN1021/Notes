- ! We cannot initialize an array as a copy of another array, nor is it legal to assign one array to anothercpp


```cpp
int scores[]  { 100, 95, 89 };
int scores[3] { 100, 95, 89 };

// int *scores_ptr = &scores[0];
int *scores_ptr { scores }; 

// subscript notion
scores[0];     // 100
scores_ptr[0]; //100

// offset notion
*scores;       // 100
*(scores + 1)  // 95

*scores_ptr;      //100
*(scores_ptr + 1) // 95
```

## char
```cpp
char str[] = "C++"; 
```

## pointer
```cpp
int *ptrs[10];          // ptrs is an array of 10 pointers to int
int (*ptr)[10] = &arr;  // ptr points to an array of 10 ints 
int *(&array)[10] = ptrs; // array is a reference to an array of ten poinrs
```

## reference
```cpp
int(&arrRef)[10] = arr; // arrRef refers to an array of 10 ints
```
