```cpp
int arr[3][4];  
int arr [10][20][30] = {0}; // initialize all element to 0

int arr {
	{0, 1, 2, 3},
	{4, 5, 6, 7},
	{8, 9, 10, 11}
};

/* 
	1. row as a reference to an array of 4 ints
	2. bind the reference to the second row in ia
*/
int (&row)[4] = ia[1]; 
```