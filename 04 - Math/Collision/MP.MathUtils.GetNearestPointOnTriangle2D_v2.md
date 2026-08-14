```cpp
Vec2 GetNearestPointOnTriangle2D_v2( Vec2 referencePos, Vec2 ccw0, Vec2 ccw1, Vec2 ccw2 )
{
	Vec2 directionAtoP = referencePos - ccw0;
	Vec2 directionAtoB = ccw1 - ccw0;
	Vec2 directionCtoA = ccw0 - ccw2;
	Vec2 directionBtoP = referencePos - ccw1;
	Vec2 directionBtoC = ccw2 - ccw1;
	Vec2 directionCtoP = referencePos - ccw2;


	bool isPointBeforeB = DotProduct2D( directionBtoP, directionAtoB ) > 0.f;
	bool isPointAfterB = DotProduct2D( directionBtoP, directionBtoC ) <= 0.f;
	if ( isPointBeforeB && isPointAfterB )
	{
		return ccw1;
	}

	bool isPointAfterC = DotProduct2D( directionCtoP, directionBtoC ) > 0.f;
	bool isPointbeforeC = DotProduct2D( directionCtoP, directionCtoA ) <= 0.f;
	if ( isPointAfterC && isPointbeforeC )
	{
		return ccw2;
	}

	Vec2 rotated90DegreesCtoA = directionCtoA.GetRotatedBy90Degrees();
	bool isPointDuringAC = DotProduct2D( directionCtoP, rotated90DegreesCtoA ) < 0.f;
	if ( isPointDuringAC )
	{
		return ccw2 + GetProjectedVector2D( directionCtoP, directionCtoA );
	}

	Vec2 rotated90DegreesAtoB = directionAtoB.GetRotatedBy90Degrees();
	bool isPointDuringAB = DotProduct2D( directionAtoP, rotated90DegreesAtoB ) < 0.f;
	if ( isPointDuringAB )
	{
		return ccw0 + GetProjectedVector2D( directionAtoP, directionAtoB );
	}

	Vec2 rotated90DegreesBtoC = directionBtoC.GetRotatedBy90Degrees();
	bool isPointDuringBC = DotProduct2D( directionBtoP, rotated90DegreesBtoC ) < 0.f;
	if ( isPointDuringBC )
	{
		return ccw1 + GetProjectedVector2D( directionBtoP, directionBtoC );
	}

	Vec2 nearestPointOnAB = ccw0 + GetProjectedVector2D( directionAtoP, directionAtoB );
	Vec2 nearestPointOnBC = ccw1 + GetProjectedVector2D( directionBtoP, directionBtoC );
	Vec2 nearestPointOnCA = ccw2 + GetProjectedVector2D( directionCtoP, directionCtoA );


	return Vec2();
}

```