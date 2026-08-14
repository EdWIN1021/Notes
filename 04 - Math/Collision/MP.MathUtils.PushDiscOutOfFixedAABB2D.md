```c++
bool PushDiscOutOfFixedAABB2D( Vec2& mobileDiscCenter, float discRadius, AABB2 const& fixedBox )
{
	const Vec2 nearestPoint = fixedBox.GetNearestPoint( mobileDiscCenter );
	
	const Vec2 nearestToMobileVec = mobileDiscCenter - nearestPoint;
	float distance = nearestToMobileVec.GetLength();

	if ( distance < discRadius )
	{
		Vec2 pushDirection;
		if ( distance > 0.f )
		{
			pushDirection = nearestToMobileVec.GetNormalized();
		}
		else
		{
			float left  = mobileDiscCenter.x - fixedBox.m_mins.x;
			float right = fixedBox.m_maxs.x  - mobileDiscCenter.x;
			float up    = fixedBox.m_maxs.y  - mobileDiscCenter.y;
			float down  = mobileDiscCenter.y - fixedBox.m_mins.y;

			float minDistance = left;
			pushDirection = Vec2(-1.f, 0.f);

			if ( right <  minDistance ) { minDistance = right;  pushDirection = Vec2( 1.f,  0.f );  }
			if ( up    <  minDistance ) { minDistance = up;     pushDirection = Vec2( 0.f,  1.f );  }
			if ( down  <  minDistance ) { minDistance = down;   pushDirection = Vec2( 0.f, -1.f ); }
		}

		float overlapDistance = discRadius - distance;
		mobileDiscCenter += pushDirection * overlapDistance;

		return true;
	}

	return false; 
}
```