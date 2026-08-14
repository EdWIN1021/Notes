# Reference to a Pointer (`*&`)

In C++, `*&` is a **reference to a pointer**. It allows you to modify the original pointer's address inside a function or a block, rather than just copying it.

### 1. Usage Scenario

In your code snippet, you use it to find an empty slot in an array and allocate a new object directly into that slot:

```cpp
Wasp* Game::SpawnNewRandomWasp()
{
	for ( int waspIndex = 0; waspIndex < MAX_WASPS; ++ waspIndex )
	{
		// 'wasp' is a reference to the pointer stored in the m_wasps array.
		// Modifying 'wasp' below will directly update m_wasps[waspIndex].
		Wasp *& wasp = m_wasps[waspIndex];
		
		if ( !wasp ) // If this slot is null
		{
			// Directly assigns the new object to m_wasps[waspIndex]
			wasp = new Wasp( this, Rgba8(255, 255, 0), WASP_HEALTH );
			wasp->m_position = GetRandomOffscreenPositionForObject( wasp->m_cosmeticRadius );
			return wasp;
		}
	}
	return nullptr;
}
```

### 2. Why use `*&`?

If you had used `Wasp* wasp = m_wasps[waspIndex];` (without the `&`):
- `wasp` would be a **copy** of the pointer in the array.
- When you set `wasp = new Wasp(...)`, you would only be changing where the local variable `wasp` points.
- The pointer in the `m_wasps` array would **still be null**, leading to a memory leak.

### 3. Comparison

| Type | Syntax | Meaning | Can change original pointer? |
| :--- | :--- | :--- | :--- |
| Pointer | `T* p` | Pass-by-value of the pointer | **No** (only changes local copy) |
| Reference to Pointer | `T*& p` | Pass-by-reference of the pointer | **Yes** (directly modifies original) |

### 4. Reading Tip
Read from right to left:
`int * &` 
`&` (Reference) to `*` (Pointer) to `int`.
