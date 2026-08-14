
> [!NOTE]
> Returns the largest integer less than or equal to the given floating-point value.

```cpp
int RoundDownToInt(float value)
{
	return static_cast<int>(floorf(value));
}
```

```
RoundDownToInt( 3.7f );   // return   3
RoundDownToInt( -2.3f );  // return -3
RoundDownToInt( 5.0f );   //  return  5
```

