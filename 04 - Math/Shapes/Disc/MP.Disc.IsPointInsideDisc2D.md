
## sectorTip

The **sectorTip** is the **vertex/origin of the sector**

## sectorApertureDegrees

The total angular with of the sector, in degrees.

## sectorFwdDegrees

| sectorFwdDegrees | Sector Direction | Explanation       |
| ---------------- | ---------------- | ----------------- |
| 0°               | Right →          | Along the +x axis |
| 90°              | Up ↑             | Along the +y axis |
| 180°             | Left ←           | Along the -x axis |
| 270° or -90°     | Down ↓           | Along the -y axis |

### Calculate distance from point to sector tip
```cpp
float distanceToTip = GetDistance2D(point, sectorTip);
if (distanceToTip > sectorRadius)
{
    return false;
}
```

### Get the sector’s forward direction as a vector

```cpp
Vec2 sectorForward = Vec2::MakeFromPolarDegrees(sectorFwdDegrees);
```


### Compute the vector from tip to the point
```cpp
Vec2 vectorToPoint = point - sectorTip;
```


### Compute the angle between the point vector and the sector’s forward vector
```cpp
float angle = GetAngleDegreesBetweenVectors2D(vectorToPoint, sectorForward);
```


### Check if the point is within the sector’s aperture

```cpp
return angle <= sectorApertureDegrees * 0.5f;
```