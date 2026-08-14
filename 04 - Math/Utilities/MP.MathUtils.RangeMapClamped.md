
> [!NOTE]
> A function that **linearly maps a value from one range to another** and **clamps the result within the output range**, ensuring it never goes below the minimum or above the maximum of the output range.


```cpp
float RangeMapClamped( float inValue, float inStart, float inEnd, float outStart, float outEnd )
{
	float fraction = GetFractionWithinRange( inValue, inStart, inEnd );
	float lerp = Interpolate( outStart, outEnd, fraction );

	if ( outStart > outEnd ) 
	{
		return GetClamped( lerp, outEnd, outStart );
	}

	return GetClamped( lerp, outStart, outEnd );
}
```

- If `outStart > outEnd` (the range goes from high to low)  
    → the minimum should be `outEnd` and the maximum should be `outStart`.
    
    
- If `outStart <= outEnd` (the range goes from low to high)  
    → the minimum should be `outStart` and the maximum should be `outEnd`.