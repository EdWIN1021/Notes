
> [!NOTE]
>i, j basis refers to the two fundamental direction vectors that define a coordinate system.

## Local

- *$\vec{i}$* is a vector of length 1 pointing straight along the positive X-axis. Its value is `(1, 0)`.
    
- *$\vec{j}$* is a vector of length 1 pointing straight along the positive Y-axis. Its value is `(0, 1)`.


$$
P_{world} = T_{world} + ( Local_x * \widehat{i}) + (Local_y * \widehat{j})
$$


## Rotation Degrees & Uniform Scale

```C++
Vec2 iBasis = Vec2::MakeFromPolarDegrees( zRotationDegrees, uniformScaleXY );
Vex2 jBasis = iBasis.GetRotateBy90Degrees();
```

