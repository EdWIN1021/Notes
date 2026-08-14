- ! Cannot return an array directly from a function.
- ! Never return a pointer to a local variable


## Returning a Pointer

```cpp
int *largest_int(int* int_ptr1, int* int_ptr2){
  if(*int_ptr1 > *int_ptr2)
    return int_ptr1;
  else
    return int_ptr2;
}
```