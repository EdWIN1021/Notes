---
type: synthesis
created: 2026-08-13
last_updated: 2026-08-13
tags:
  - zion
  - knowledge-management
  - information-architecture
  - obsidian
aliases:
  - Zion Vault Reorganization Design
sources:
  - "[[AGENTS]]"
  - "[[index]]"
---

# Zion Vault 全库整理设计

## 目标

在不改变知识正文含义的前提下，对整个 Zion Vault 进行一次可验证、可追踪的目录与命名整理，使每篇笔记位于明确的领域和子领域中，并消除扁平目录、冗余文件名前缀、明显拼写错误和根目录散落页面。

整理完成后应满足：

- 领域边界符合 [[AGENTS]] 定义的 Raw Sources、Wiki / Domain Notes、Control Plane 三层结构。
- 文件夹和文件名延续现有风格：英文技术名、中文正文、编号核心领域目录。
- 每次移动都有确定的旧路径 → 新路径映射。
- Wiki links、附件 embeds、索引和日志同步更新。
- 不丢失笔记或附件，不改变 `raw/` 中已有源文件的正文。

## 当前状态

只读审计基线：

- Markdown 文件：297 篇。
- `01 - C++/`：192 篇，其中 188 篇直接位于目录根部。
- `03 - D3D11/`：48 篇，全部直接位于目录根部。
- `04 - Math/`：32 篇，全部直接位于目录根部。
- Wiki links：277 个。
- 带 frontmatter 的笔记：37 篇；不带 frontmatter 的笔记：260 篇。
- `00 - Assets/`：11 个 PNG 附件，已有 D3D11 与 Math 子目录。

主要问题：

1. C++、D3D11 和 Math 领域过度扁平。
2. `CPP.`、`D3D11.`、`MP.` 等文件名前缀在子目录结构下会重复表达领域。
3. 同类名称混用单复数、空格、点号和连字符。
4. 存在 `Modifires`、`Deferencing` 等明显拼写错误。
5. 根目录存在不属于 Control Plane 的内容笔记。
6. 大多数旧笔记没有 frontmatter，重命名后缺少旧名称 alias。

## 已选择方案

采用“保留领域、整理层级”的中等迁移方案。

不采用以下方案：

- **仅建立文件夹、保留全部旧文件名**：迁移风险低，但冗余前缀和错误命名仍然存在。
- **完全重建顶层领域与命名体系**：表面最统一，但会破坏 Vault 已有风格，并产生不必要的大规模变更。

选定方案保留已有顶层领域，只在领域内部建立稳定子目录，并在能够明确判断语义时修正文件名。

## 顶层结构

```text
Zion/
├─ 00 - Assets/
├─ 01 - C++/
├─ 03 - D3D11/
├─ 04 - Math/
├─ animation/
├─ raw/
├─ rendering/
├─ thesis/
├─ wiki/
├─ AGENTS.md
├─ CLAUDE.md
├─ index.md
└─ log.md
```

规则：

- 保留现有编号领域目录，不重新编号。
- 不创建没有内容的空领域目录。
- 根目录只保留 Control Plane 文件。
- `.obsidian/`、`.git/`、`.trash/`、`.agents/`、`.codex/` 不参与整理。
- `00 - Assets/` 保持附件内容和现有分类；只有在修复 embed 时更新引用文本。
- `raw/` 中已经存在的文件不改写、不删除、不移动。

## C++ 目录设计

```text
01 - C++/
├─ Language Fundamentals/
├─ Functions/
├─ Classes & OOP/
├─ Pointers & References/
├─ Memory Management/
│  └─ Smart Pointers/
├─ Templates/
├─ Build & Preprocessor/
└─ STL/
   ├─ Containers/
   │  ├─ array/
   │  ├─ deque/
   │  ├─ list/
   │  ├─ map/
   │  ├─ priority_queue/
   │  ├─ queue/
   │  ├─ set/
   │  ├─ stack/
   │  ├─ string/
   │  └─ vector/
   ├─ Algorithms/
   └─ Iterators/
```

分类规则：

- Fundamental types、casts、value categories、scope 和 language keywords → `Language Fundamentals/`。
- Function declarations、parameters、return types、lambda 和 overload → `Functions/`。
- Classes、constructors、inheritance、polymorphism、struct、union 和 enum → `Classes & OOP/`。
- Raw pointers、references、pointer operations → `Pointers & References/`。
- `new/delete`、copy/move、RAII 和 ownership → `Memory Management/`。
- `unique_ptr`、`shared_ptr`、`weak_ptr` 和总览 → `Memory Management/Smart Pointers/`。
- Function/class templates、type aliases 与 template mechanics → `Templates/`。
- CMake、source/header、preprocessor、macro、pragma → `Build & Preprocessor/`。
- Standard containers、algorithms 和 iterators → `STL/` 对应子目录。

## D3D11 目录设计

```text
03 - D3D11/
├─ Initialization/
├─ Pipeline/
│  ├─ Input Assembler/
│  ├─ Rasterizer/
│  ├─ Output Merger/
│  └─ Stream Output/
├─ Resources/
│  ├─ Buffers/
│  └─ Textures & Views/
├─ Shaders/
├─ Drawing/
└─ HLSL/
```

分类规则：

- Device、device context、swap chain 和初始化流程 → `Initialization/`。
- 固定功能管线阶段与绑定操作 → `Pipeline/` 对应阶段。
- Vertex/index/constant buffer 与通用 resource 操作 → `Resources/Buffers/`。
- Texture、render target、depth buffer、SRV/RTV/DSV → `Resources/Textures & Views/`。
- Vertex/pixel/geometry/hull/domain shader 对象与编译 → `Shaders/`。
- `Draw`、`DrawIndexed`、clear 和 frame-level rendering commands → `Drawing/`。
- HLSL/FX 结构与 shader-language 笔记 → `HLSL/`。

## Math 目录设计

```text
04 - Math/
├─ Vectors/
├─ Geometry/
│  ├─ AABB/
│  ├─ OBB/
│  └─ Disc/
├─ Collision/
├─ Physics/
├─ Rotation & Transforms/
└─ Utilities/
```

分类规则：

- `Vec2`、vector operations、dot/cross product → `Vectors/`。
- AABB、OBB、Disc、Triangle 的表示与最近点查询 → `Geometry/` 对应子目录。
- Overlap、raycast、push-out 与 inside tests → `Collision/`；当笔记主要解释形状自身 API 时保留在 `Geometry/`。
- Position、velocity、speed、displacement → `Physics/`。
- Rotation 和 vertex transform → `Rotation & Transforms/`。
- Range mapping、clamp、rounding 和通用 MathUtils → `Utilities/`。

## 其他领域

- `animation/`、`rendering/`、`thesis/` 和 `wiki/` 已具备明确领域语义，保留顶层位置。
- 这些目录中的页面只在名称明显错误或位置与正文主题明显冲突时移动。
- Source Summary 继续放在 `wiki/summaries/`。
- 通用概念继续放在 `wiki/concepts/`。
- Entity Page 继续放在 `wiki/entities/`。
- 跨领域综合分析继续放在 `wiki/syntheses/`。

## 根目录散落页处理

- 外部文章性质的 `LLM Wiki.md` 移入 `raw/articles/LLM-Wiki.md`，正文不改写。
- 日期命名的 networking 学习记录移入 `raw/notes/Networking-Packets.md`，保留原日期作为 alias 或 frontmatter date。
- 本地 Perforce 配置记录移入 `raw/notes/Perforce-Local-Configuration.md`，不在索引中暴露连接信息，并保持内容不出现在迁移报告中。

## 命名规范

1. 文件夹使用英文技术名称；顶层领域保留当前大小写与编号。
2. 文件名优先使用标准类型、函数、API 或概念名称。
3. 类型页使用类型名：`unique_ptr.md`、`ID3D11Device.md`、`AABB2.md`。
4. 成员 API 页使用 `Type.Member.md`：`vector.push_back.md`、`AABB2.GetNearestPoint.md`。
5. 非 API 概念使用可读的 Title Case 或现有领域的连字符风格，不为了形式统一而改动语义正确的成熟页面。
6. 放入明确子目录后移除冗余领域前缀：`CPP.`、`D3D11.`、`MP.`。
7. Smart Pointer 文件使用标准库类型名：`unique_ptr.md`、`shared_ptr.md`、`weak_ptr.md`。
8. 修正能够确定的拼写错误；不确定的术语保留原名并进入人工复核清单。
9. Windows 不允许的文件名字符不使用；`std::unique_ptr` 只能作为标题或 alias，不能作为文件名。
10. 每个更改 basename 的笔记保留旧 basename 作为 alias，避免搜索习惯立即失效。

## 迁移数据流

```text
Inventory
  → Classification
  → Collision Check
  → Old-to-New Mapping
  → Move/Rename
  → Rewrite Wiki Links and Embeds
  → Update Frontmatter Aliases
  → Rebuild index.md
  → Append log.md
  → Verification
```

迁移前先生成完整 mapping。任何文件只有在以下条件同时满足时才移动：

- 目标分类明确。
- 目标路径不存在。
- Windows 大小写不敏感比较下不存在冲突。
- 所有指向旧路径的 Wiki links 和 embeds 已进入更新集合。

## 冲突与异常处理

- **同名冲突**：停止该文件的移动，使用更具体的类型/API 名区分；不覆盖目标文件。
- **分类不确定**：保留原位并写入复核报告；不做猜测性移动。
- **链接目标歧义**：改为完整路径 Wiki link。
- **附件引用**：附件本体不移动，只修复引用路径。
- **敏感配置页**：允许移动但不在日志中复制其正文或连接信息。
- **已有工作区改动**：只修改本次整理涉及的路径，不覆盖 `.obsidian/` 或其他用户改动。
- **失败恢复**：mapping 同时作为回滚映射；验证失败时只回退本批次迁移，不进行全库 destructive reset。

## 分批执行

为了降低一次性迁移风险，按以下批次执行，每批独立验证：

1. C++ Language、Functions、Classes & OOP。
2. C++ Pointers、Memory Management、Templates、Build。
3. C++ STL Containers。
4. C++ STL Algorithms 与 Iterators。
5. D3D11。
6. Math。
7. Root loose notes、animation、rendering、thesis、wiki 的异常项。
8. 全库索引重建与最终 lint。

每批验证通过后才进入下一批。

## 验证标准

完成必须同时满足：

- Markdown 文件总数等于迁移前基线，加上本设计页和迁移产物中明确新增的控制文档数量。
- 11 个现有 PNG 附件全部存在，内容哈希不变。
- `raw/` 中迁移前已经存在的源文件路径与内容哈希不变。
- 所有 mapping 源路径消失，目标路径存在。
- 没有未登记的目标路径覆盖。
- 没有因为本次迁移新增的死链、歧义链接或断开的 embeds。
- `index.md` 的领域和页面路径与最终目录一致。
- `log.md` 记录各批次移动范围，但不复制敏感笔记正文。
- Markdown code fences 成对，新增 frontmatter 可以被解析。
- `git diff --check` 无 whitespace error。

## 非目标

本次整理不执行以下工作：

- 不重写 297 篇笔记的正文。
- 不把所有旧笔记强制改造成统一模板。
- 不合并疑似重复概念页；重复项只进入后续 lint 报告。
- 不删除空白或低质量笔记。
- 不移动或重命名附件本体。
- 不修改 Obsidian 插件、主题或 workspace 配置。

## 成功定义

用户可以从领域 → 子领域 → 类型/API 的目录路径直接找到笔记；搜索旧名称仍能通过 alias 找到被重命名页面；Vault 中没有因整理造成的内容丢失或新增断链。
