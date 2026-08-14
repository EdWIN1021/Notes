Yaw Major - Rot about skyward
Pitch Middle - Rot about Left/Right
Roll Minor - Rot about Forward

X -> Y -> Z ->X -> Y -> Z

## Yaw

$$R_{z} = \begin{bmatrix} CY & -SY & 0 & 0 \\ SY & CY & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

## Pitch
$$R_{y} = \begin{bmatrix} CP & 0 & SP & 0 \\ 0 & 1 & 0 & 0 \\ -SP & 0 & CP & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$
## Roll
$$R_{x} = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & Cr & -Sr & 0 \\ 0 & Sr & Cr & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$
