```cpp
void AABB2::SetDimensions( Vec2 const& newDimensions )
{
	const Vec2 center = GetCenter();
	const Vec2 halfNewDimensions = newDimensions * 0.5f;

	m_mins = Vec2( center.x - halfNewDimensions.x, center.y - halfNewDimensions.y );
	m_maxs = Vec2( center.x + halfNewDimensions.x, center.y + halfNewDimensions.y );
}
```