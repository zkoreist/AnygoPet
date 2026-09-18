# MVP 执行计划（AnygoPet）

> 版本：v1.0 ｜ 日期：2026-09-18 ｜ 项目名：AnygoPet ｜ 仓库：github.com/zkoreist/AnygoPet

## 0. 一个必须先说的认知调整

原计划是按"从零开发"估的。但既然 fork 的是一个**已经能跑的应用**，MVP 的性质变了：

| docs/02 中的需求 | 上游是否已实现 |
|---|---|
| FR-01 透明/无边框/置顶/穿透 | ✅ 已实现，需验证 |
| FR-02 待机动画 | ✅ 已实现（Live2D idle motion） |
| FR-03 拖拽 + 位置记忆 | ✅ 已实现，需验证 |
| FR-04 点击交互 | ✅ 已实现 |
| FR-05 右键菜单 / 系统托盘 | ✅ 已实现，需精简 |
| FR-06 外观包 | ✅ 已实现（导入自定义模型） |
| FR-07 高 DPI | ⚠️ 待验证 |
| FR-08 多显示器 | ⚠️ 待验证 |

**结论：FR-01～FR-06 上游基本都做好了。MVP 的工作量主要在「剥离、改名、验证、替换素材」，而不是从零写功能。**

这会让第一版快很多，但有一个代价：**你们得先读懂别人的代码，才知道该删哪里、改哪里。** 这是 23k star 项目，代码量不小。

---

## 1. Wave 0 —— 阻塞项（过不去就全部停摆）

唯一目标：**本地能跑起来**。

```bash
# 1. 装环境
Node.js 22 LTS
npm i -g pnpm
rustup（Rust stable）
Visual Studio Build Tools → 勾选「C++ 桌面开发工作负载」（约 5GB）

# 2. 拉代码
git clone https://github.com/zkoreist/AnygoPet.git
cd AnygoPet

# 3. 跑起来（注意必须是 pnpm）
pnpm install
pnpm tauri dev
```

**验收**：桌面出现透明窗口，角色会动，能拖动。

> 建议顺序：先把 MSVC 装上。它最慢、最容易卡，装完其他都是分钟级。
> 常见坑见 `docs/03-开发环境与换机迁移手册.md`（第 7 节）。

---

## 2. Wave 1 —— MVP 骨架改造

| # | 任务 | 要点 | 对应需求 |
|---|---|---|---|
| **W1-1** | 剥离键鼠映射 | 阶段一不需要。**禁用而非删除**——隐藏相关设置项、不启用 global-shortcut，代码留着给阶段二 | Out of Scope |
| **W1-2** | 品牌改造 | `tauri.conf.json` 的 productName / identifier、package.json name、图标、托盘图标统一为 AnygoPet；删除原项目 QQ 群与网盘引流 | — |
| **W1-3** | 高 DPI 验证 | 125% / 150% 缩放下：渲染是否清晰、拖拽坐标是否错位 | FR-07 |
| **W1-4** | 多显示器验证 | 位置记忆、拖出屏幕拉回、目标显示器消失时回退主屏 | FR-08 |
| **W1-5** | 交互最小集收口 | 右键菜单与托盘精简为：置顶开关、退出 | FR-05 |

### W1-1 的注意事项

上游的核心卖点就是键鼠映射，它渗透在设置面板、模型加载逻辑和 Rust 插件里。剥离时**千万别直接删文件**——先定位，再置灰/旁路，否则阶段二恢复会很痛。

建议做法：先做一份"键鼠映射涉及哪些文件"的源码笔记，再动手。

---

## 3. Wave 2 —— MVP 收口与出包

| # | 任务 | 说明 |
|---|---|---|
| **W2-1** | 形象方案定档 | 已选"沿用 Live2D，以后再改"。MVP 阶段锁定一个可商用的替代模型（Booth 成品 ¥350–1400 或免费样例）。**开发期可继续沿用原模型，但 v1.0 发布前必须替换** |
| **W2-2** | MSIX 打包 | 为上架做准备，顺带拿到微软的免费签名 |
| **W2-3** | 逐条过验收 | `docs/02` 第 6 节的 14 条 checklist |
| **W2-4** | 同步文档 | NOTICE / README 中的项目名与授权状态同步为 AnygoPet |

---

## 4. 建议节奏（按每天 2-3 小时）

| 时间 | 内容 |
|---|---|
| 第 1 周 | Wave 0 环境 + **读代码**（重点：窗口配置、模型加载、键鼠监听在哪一层） |
| 第 2–3 周 | Wave 1 五项改造 |
| 第 4 周 | Wave 2 与验收 |

第 1 周"读代码"不是摸鱼——fork 一个 23k star 项目，读不懂就改不动，改错了排查成本更高。

---

## 5. 风险与对策

| 风险 | 概率 | 对策 |
|---|---|---|
| MSVC 装不上 / 编译失败 | 高 | 先验证这一项；实在不行回退 Electron 路线（docs/01 第 6 节） |
| 读不懂上游代码，改不动 | 中高 | 先只读三处：窗口配置、模型加载、键鼠监听；其余先不动 |
| 高 DPI 有问题 | 中 | 办公本普遍 125%/150%，早验证早发现 |
| 剥离键鼠映射时误删 | 中 | 禁用而非删除，且先出源码笔记 |
| 形象替换卡住 | 中 | 已选"先沿用"，不阻塞 MVP；但 v1.0 前是硬门槛 |

---

## 6. 分工与协作方式

**本机环境限制**（已在 `docs/03` 记录）：当前工作机的 git 网络通道不通，无法执行 clone / push。因此：

- **K 在自己机器上**：clone、装环境、实际编码、push
- **我这边能做的**：写文档、改配置文件（通过 GitHub API 提交）、技术答疑、方案评审、验收清单核对

也就是说——**MVP 的代码推进需要你来动手，我负责把路标和检查点立好。**

---

## 7. 现在的状态

| 项 | 状态 |
|---|---|
| 项目名 | ✅ AnygoPet |
| 仓库 | ✅ github.com/zkoreist/AnygoPet |
| fork 声明与授权说明 | ✅ NOTICE.md 已提交 |
| 需求与验收标准 | ✅ docs/02 |
| 技术选型 | ✅ docs/01 |
| 环境与迁移 | ✅ docs/03 |
| 商业化合规 | ✅ docs/04 |
| 本地环境 | ✅ 已装完并跑通（首次编译约 56 秒） |
| git 历史 | ✅ 已接上（本地 master = `f7fbcae`） |
| **应用标识改名** | ✅ 已提交（`18c4205` / `beaee5f` / `9cd4aed`） |
| **上游更新服务器回连** | 🟡 **已切断**（`f7fbcae`，端点改为指向自己仓库）；完整拆除见事项 r2jR6j |
| 图标与前端文案改名 | ⬜ 待做 |
| 剥离键鼠映射 | ⬜ 待做（Wave 1 主项，改动点已定位） |
| 高 DPI / 多屏验证 | ⬜ 待做 |
| MSIX 打包 | ⬜ 待做（Wave 2） |

## 8. 换机继续工作

代码和本文档都在云端，换机不用手动拷贝。

| 内容 | 在哪 | 换机怎么办 |
|---|---|---|
| 源码 + 全部历史 | GitHub `zkoreist/AnygoPet` | `git clone`，无需拷贝 |
| 本文档（docs/ 01–05） | **已随代码一起在仓库里** | clone 下来就有 |
| `node_modules`、`target/` | 本地，**不要拷贝** | 新机重装：pnpm install + 编译约 1 分钟 |
| 模型素材 | 仓库内 `src-tauri/assets/models/` | 跟随仓库 |

**新机器三步**：

```bash
git clone https://github.com/zkoreist/AnygoPet.git
cd AnygoPet
pnpm install && pnpm tauri dev
```

⚠️ 换机必读 `docs/03` 第 9 节——尤其是代理和 pnpm 缓存那两个坑，大概率会重演。

### 已完成的改名（2026-09-18）

| 文件 | 改动 | commit |
|---|---|---|
| `src-tauri/tauri.conf.json` | productName / identifier / 窗口 title / shortDescription → AnygoPet，`com.ayangweb.BongoCat` → `com.anygo.pet` | `18c4205` |
| `src-tauri/Cargo.toml` | package name `bongo-cat` → `anygo-pet` | `beaee5f` |
| `package.json` | name `bongo-cat` → `anygo-pet` | `9cd4aed` |

**刻意保留**：Cargo 的 `[lib] name = "bongo_cat_lib"` 未改。因为 `main.rs` 里是 `bongo_cat_lib::run()`，单独改会编译失败；等能同步改代码时一并处理。
