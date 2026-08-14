
```cpp
Vec2 const AABB2::GetNearestPoint( Vec2 const& referencePosition ) const
{
	float nearestX = GetClamped( referencePosition.x, m_mins.x, m_maxs.x );
	float nearestY = GetClamped( referencePosition.y, m_mins.y, m_maxs.y );

	const Vec2 nearestPoint = Vec2( nearestX, nearestY );

	return nearestPoint;
}
```