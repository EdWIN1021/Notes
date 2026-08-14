## Default Constructor Parameters
```cpp
Player(string name_val = "None", int health_val = 0, int xp_val = 0);
```

## Copy Constructor
```cpp
Player::Player(const Player &source) : name(source.name), health(source.health), xp(source.xp){}
```