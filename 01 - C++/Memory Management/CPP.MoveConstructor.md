```cpp
Move::Move(Move &&source):data{source.data} {
  source.data = nullptr;
}
```