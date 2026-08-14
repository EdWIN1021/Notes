
$$
\hat{V} = \frac{\vec{V}}{|\vec{V}|}

$$

```cpp
Vec2 const Vec2::GetNormalized() const
{
	float length = GetLength();

	return Vec2( x, y ) / length;
}
```
 