```ad-important
Points to an object of type T on the heap
Does not participate in owning relationship
Always created from a shared_ptr
Does Not increment or decrement reference use count
Used to prevent strong reference cycles which could prevent object s from being delete
```
