```ad-important
A union is a data format that can hold different data types but only one type at a time
The size of the union is the size of its largest membe
```

```cpp
union one4all {
	int int_val;
	long long_val;
	double double_val;
};
```