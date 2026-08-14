---
type: concept
last_updated: 2026-08-13
tags:
  - cpp
  - memory-management
  - smart-pointer
aliases:
  - std::unique_ptr
  - Unique Pointer
---

# `std::unique_ptr`

## 核心直觉

> [!important]
> `std::unique_ptr<T>` 表示一个对象只有一个 owner（所有者）。owner 离开作用域或被重置时，所管理的对象会被自动销毁。

它适合表达“这个对象由谁负责销毁”。因为所有权是唯一的，所以 `unique_ptr`：

- 不能复制（copy）。
- 可以移动（move），把所有权交给另一个 `unique_ptr`。
- 销毁时自动调用对应的 `delete`，符合 RAII。
- 不支持普通指针的算术运算，如 `++`、`--`。

需要包含：

```cpp
#include <memory>
```

## 创建对象

优先使用 `std::make_unique`：

```cpp
auto health = std::make_unique<int>(100);
auto player = std::make_unique<Player>("Hero", 100, 100);
```

也可以直接接管 `new` 返回的指针，但一般不推荐：

```cpp
std::unique_ptr<Player> player{
    new Player("Hero", 100, 100)
};
```

`make_unique` 更短，也能避免复杂表达式中裸 `new` 带来的资源管理问题。

## 访问对象

```cpp
player->Attack();      // Access a member through the pointer
player->health = 80;

Player& ref = *player; // Dereference and obtain the object
```

使用前可以检查它是否持有对象：

```cpp
if (player) {
    player->Update();
}
```

默认构造或移动后的 `unique_ptr` 为空：

```cpp
std::unique_ptr<Player> player;

if (!player) {
    // player == nullptr
}
```

## 唯一所有权：不能复制，只能移动

```cpp
auto player1 = std::make_unique<Player>("Hero", 100, 100);

// Error: unique_ptr cannot be copied.
// auto player2 = player1;

// Transfer ownership from player1 to player2.
auto player2 = std::move(player1);
```

移动之后：

```cpp
player1 == nullptr; // true
player2 != nullptr; // true
```

`std::move` 在这里表示：把“负责销毁 `Player` 的责任”从 `player1` 转交给 `player2`。底层 `Player` 对象通常不会因为这次转移而被复制或移动。

相关机制见 [[01 - C++/CPP.MoveConstructor|Move Constructor]] 和 [[01 - C++/CPP.MoveAssignment|Move Assignment]]。

## 函数参数如何设计

### 函数接管所有权

参数按值接收 `unique_ptr`，调用者必须显式转移所有权：

```cpp
void SetPlayer(std::unique_ptr<Player> player)
{
    // This function now owns the Player.
}

auto player = std::make_unique<Player>("Hero", 100, 100);
SetPlayer(std::move(player));

// player is now nullptr.
```

### 函数只使用对象，不接管所有权

优先传对象的引用或普通指针，直接表达 non-owning access（非所有权访问）：

```cpp
void UpdatePlayer(Player& player)
{
    player.Update();
}

UpdatePlayer(*player);
```

如果允许不传对象，可以使用普通指针：

```cpp
void TryUpdatePlayer(Player* player)
{
    if (player) {
        player->Update();
    }
}

TryUpdatePlayer(player.get());
```

### 函数需要替换调用者持有的指针

只有函数确实要修改这个 `unique_ptr` 本身时，才传非常量引用：

```cpp
void ReplacePlayer(std::unique_ptr<Player>& player)
{
    player = std::make_unique<Player>("Mage", 80, 120);
}
```

## 从函数返回

工厂函数通常直接返回 `unique_ptr`：

```cpp
std::unique_ptr<Player> CreatePlayer()
{
    return std::make_unique<Player>("Hero", 100, 100);
}

auto player = CreatePlayer();
```

这里通常不要写 `return std::move(player);`。直接返回即可，由 copy elision（复制消除）或 move 完成所有权转移。

## 常用成员函数

### `get()`：取得非所有权指针

```cpp
Player* rawPlayer = player.get();
```

`rawPlayer`：

- 不拥有对象，不能对它执行 `delete`。
- 不能比 `player` 活得更久。
- 当 `player` 销毁、`reset()` 或被移动后，可能成为 dangling pointer（悬空指针）。

### `reset()`：销毁或替换当前对象

```cpp
player.reset(); // Destroy the Player and become nullptr.

player = std::make_unique<Player>("Mage", 80, 120);
```

给 `unique_ptr` 赋一个新的 `unique_ptr` 时，旧对象会先被自动销毁。

### `release()`：放弃所有权但不销毁对象

```cpp
Player* rawPlayer = player.release();

// Ownership is now manual.
delete rawPlayer;
```

> [!warning]
> `release()` 不会销毁对象。调用后必须立刻明确新的 owner，否则非常容易造成 memory leak（内存泄漏）。普通业务代码通常不需要它。

## 放入容器

```cpp
#include <memory>
#include <vector>

std::vector<std::unique_ptr<Enemy>> enemies;

enemies.push_back(std::make_unique<Enemy>("Goblin"));
enemies.push_back(std::make_unique<Enemy>("Boss"));

for (const auto& enemy : enemies) {
    enemy->Update();
}
```

容器移动的是 `unique_ptr`，不会复制它所管理的 `Enemy` 对象。

如果已经有一个具名 `unique_ptr`，放入容器时需要转移所有权：

```cpp
auto enemy = std::make_unique<Enemy>("Goblin");
enemies.push_back(std::move(enemy));

// enemy is now nullptr.
```

## 多态

`unique_ptr` 很适合管理多态对象：

```cpp
std::unique_ptr<Character> character =
    std::make_unique<Player>();

character->Update();
```

通过基类指针销毁派生类对象时，基类必须有 virtual destructor（虚析构函数）：

```cpp
class Character {
public:
    virtual ~Character() = default;
    virtual void Update() = 0;
};
```

否则通过 `Character*` 销毁 `Player` 会产生 undefined behavior（未定义行为）。

## 数组

如果确实需要拥有动态数组，可以使用数组特化：

```cpp
auto values = std::make_unique<int[]>(100);
values[0] = 42;
```

但大多数动态数组场景更适合使用 `std::vector<T>`，因为它同时管理长度并提供更完整的容器操作。

## 常见坑

### 对 `get()` 的结果执行 `delete`

```cpp
auto player = std::make_unique<Player>();
Player* rawPlayer = player.get();

// Wrong: player will try to delete the same object again.
// delete rawPlayer;
```

这会造成 double delete（重复释放）。

### 从同一个裸指针创建两个 `unique_ptr`

```cpp
Player* rawPlayer = new Player();

std::unique_ptr<Player> first(rawPlayer);

// Wrong: two owners believe they uniquely own the same object.
// std::unique_ptr<Player> second(rawPlayer);
```

两个 owner 最终都会尝试销毁同一个对象。

### 忘记移动

```cpp
void TakeOwnership(std::unique_ptr<Player> player);

auto player = std::make_unique<Player>();

// Error: copying is disabled.
// TakeOwnership(player);

TakeOwnership(std::move(player));
```

### 移动后继续解引用

```cpp
auto player2 = std::move(player1);

// Wrong: player1 is now nullptr.
// player1->Update();
```

## 选择规则

- 对象只有一个明确 owner：使用 `std::unique_ptr`。
- 函数只借用对象：传 `T&` 或 `T*`。
- 对象需要共享所有权：才考虑 `std::shared_ptr`。
- 对象生命周期天然由作用域或容器管理：优先直接存储对象，不必使用智能指针。

相关页面：

- [[01 - C++/CPP.SmartPointers.Smart Pointer|Smart Pointer]]
- [[01 - C++/CPP.SmartPointers.Shared Pointers|Shared Pointer]]
- [[01 - C++/CPP.SmartPointers.Weak Pointers|Weak Pointer]]
- [[01 - C++/CPP.MoveConstructor|Move Constructor]]
- [[01 - C++/CPP.MoveAssignment|Move Assignment]]
