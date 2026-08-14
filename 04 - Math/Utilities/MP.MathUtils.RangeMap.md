> [!NOTE]
>A function that **linearly maps a value from one range to another**, preserving its relative position within the original range.

```cpp
float RangeMap( float inValue, float inStart, float inEnd, float outStart, float outEnd )
{
	float fraction = GetFractionWithinRange( inValue, inStart, inEnd );

	return Interpolate( outStart, outEnd, fraction );
}
```

