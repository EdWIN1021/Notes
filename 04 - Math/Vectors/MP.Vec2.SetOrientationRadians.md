
> [!NOTE]
> This method rotates the vector to a new orientation, specified in radians, while preserving its original length (magnitude).

```c++
void Vec2::SetOrientationRadians( float newOrientationRadians )
{
	float length = GetLength();
	const Vec2 rotatedVector = MakeFromPolarRadians( newOrientationRadians, length );
	 
	x = rotatedVector.x;
	y = rotatedVector.y;
}
```
