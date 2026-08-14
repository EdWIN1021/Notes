# Raycast vs AABB3 (Slab Method)

3D Raycast vs Axis-Aligned Bounding Box (AABB3) collision detection using **Parametric Time T** logic, also known as the **"Slab Method"**.

## 1. Core Mathematical Principle
An AABB3 can be viewed as the intersection of three pairs of parallel planes (Slabs) in space. A ray hits the box only if it is **simultaneously** within the boundaries of all three slabs at the same point in time.

### Ray Equation
$$P(t) = Start + t \times (End - Start)$$
Where $t \in [0, 1]$ represents the percentage of the distance from the start to the end of the ray.

---

## 2. Algorithm Steps

### Step 1: Calculate Time Intervals for Each Axis
For each axis (X, Y, Z), calculate the time it takes for the ray to enter and exit the pair of planes:
- $t_{nearX} = (minX - startX) / dirX$
- $t_{farX} = (maxX - startX) / dirX$
- **Note**: If $t_{near} > t_{far}$, swap them to ensure `rangeX = [min(t_near, t_far), max(t_near, t_far)]`.

### Step 2: Find Global Overlap
- **Entry Time (impactT)**: The **maximum** of all entry times (the last door you enter).
  `impactT = max(rangeX.min, rangeY.min, rangeZ.min)`
- **Exit Time (exitT)**: The **minimum** of all exit times (the first door you leave).
  `exitT = min(rangeX.max, rangeY.max, rangeZ.max)`

### Step 3: Final Judgment
1. **Overlap Check**: If `impactT > exitT`, the ray leaves one slab before entering another. **Miss**.
2. **Range Check**: If `impactT < 0` or `impactT > 1`, the hit point is before the ray starts or after it ends. **Miss**.
3. **Success**: If the above conditions are met and there is an overlap, it is a **Hit**.

---

## 3. The "Divide by Zero" Trap
If the ray has no movement on a specific axis (e.g., a perfectly vertical shot where `dirX == 0`), the denominator in the formula becomes zero. This must be handled explicitly:

- **If the Start Position is outside the axis range** (e.g., $startX < minX$ or $startX > maxX$):
  Since there is no displacement, the ray will never enter this axis's slab. Return **Miss**.
- **If the Start Position is inside the axis range**:
  This axis does not restrict the collision. Grant this axis a **"Full Pass"**: `range = [-infinity, +infinity]` (often represented as `[0, 1]` or larger in code).

---

## 4. Determining the Impact Normal
"The last door that blocked your path is the face you hit."

After finding `impactT`, check which axis it originated from:
- If `impactT == rangeX.min`:
  - If `dirX > 0`, the normal is `(-1, 0, 0)` (hit the left face).
  - If `dirX < 0`, the normal is `(1, 0, 0)` (hit the right face).
- The same logic applies to the Y and Z axes.

---
