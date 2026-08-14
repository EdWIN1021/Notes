
$$
\frac{\text{MaxLength}}{\text{Length}} = \frac{\text{MaxVec2}}{\text{Vec2}}
$$
```cpp
Vec2 const Vec2::GetClamped( float maxLength ) const
{
	float length = GetLength();
	float scaleFactor = maxLength / length;

	return length <= maxLength ? Vec2( x, y ) : Vec2( x, y ) * scaleFactor;
}
```