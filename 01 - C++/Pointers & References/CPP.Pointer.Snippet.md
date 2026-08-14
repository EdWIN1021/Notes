- Always initialize pointer
- It is an error to copy or otherwise try to access the value of an invalid pointer.

## nullptr
```cpp
int *p { nullptr } // int *p = 0;

```


## int 
```cpp
int ival = 42;
int *p = &ival;
```


## array
```cpp
int scores[] {100, 95, 89};
int* scores_ptr { scores };
```


## void

- The type void* is a special pointer type that can hold the address of any object.
- We cannot use a void* to operate on the object it addresses

```cpp
double obj = 3.14;
void *p = &obj;
```

## Pointer to Pointer

```cpp
int ival = 1024;
int *p = &ival;
int **pp = &p;
```

## 2D array
```cpp
int ia[3][4];           // 2D array of integers: 3 rows, each with 4 columns
int (*p)[4] = ia;       // p is a pointer to an array of 4 integers, initialized to point to ia
p = &ia[2];             // p is now pointing to the third row of ia

```

### Concept
[[Concept.CPP.Pointer]]
