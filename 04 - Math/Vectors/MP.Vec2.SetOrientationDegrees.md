
> [!NOTE]
> This method rotates the vector to a new orientation, specified in degrees, while preserving its original length (magnitude).

```c++
void Vec2::SetOrientationDegrees( float newOrientationDegrees )
{
	float length = GetLength();
	const Vec2 rotatedVector = MakeFromPolarDegrees( newOrientationDegrees, length );

	x = rotatedVector.x;
	y = rotatedVector.y;
}
```
