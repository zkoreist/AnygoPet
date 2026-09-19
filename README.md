# AnygoPet

> 一款桌面宠物应用：用键盘、鼠标和手柄的实时操作驱动屏幕上的 Live2D 角色，它会跟着你一起打字、点鼠标、按手柄。

![AnygoPet](./src-tauri/assets/logo.png)

---

## 关于本项目

本项目是基于 [ayangweb/BongoCat](https://github.com/ayangweb/BongoCat) 的**独立 fork**，正在改造为独立产品。

- 原项目作者：ayangweb
- 原项目以 **MIT** 协议发布，版权归原作者所有。署名、许可与改动范围详见 [NOTICE.md](./NOTICE.md)
- 本项目**不再跟随上游同步**，独立演进

### ⚠️ 发布前必须解决的事项

| 事项         | 状态      | 说明                                                                                                                     |
| ------------ | --------- | ------------------------------------------------------------------------------------------------------------------------ |
| 默认角色模型 | 🔴 待替换 | `src-tauri/assets/models/` 下的三套默认模型继承自上游，**美术授权待澄清，不可商用**。正式发布前必须替换为自有或 CC0 素材 |
| 应用图标     | 🟡 待替换 | 当前图标由 `src-tauri/assets/logo.png` 生成，仍为上游素材                                                                |
| 前端文案     | 🟢 已清理 | 应用内已无上游字样                                                                                                       |

**当前构建仅供开发验证，请勿商用。**

---

## 功能

- 适配 **Windows / macOS / Linux(x11)**
- 键盘、鼠标、手柄操作实时映射为角色动作
- 支持导入自定义模型，可替换为专属形象
- 无独显办公笔记本可运行（渲染基于 WebGL 的 Live2D，非 3D 引擎）
- 应用功能**完全本地运行**，不收集任何用户数据；唯一的联网行为是自动检查更新，可关闭

---

## 下载

> 尚未发布正式版本。构建产物请参考下方「从源码构建」。

---

## 从源码构建

### 环境要求

| 依赖    | 版本要求                                                                                                   | 备注                                                     |
| ------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Node.js | 22 LTS（见 [`.nvmrc`](./.nvmrc)）                                                                          | 必须 ≥ 22，pnpm 11+ 的硬性要求                           |
| pnpm    | **强制**，禁止 npm/yarn                                                                                    | `package.json` 的 `preinstall` 有 `only-allow pnpm` 拦截 |
| Rust    | stable，工具链须为 `x86_64-pc-windows-msvc`（见 [`rust-toolchain.toml`](./src-tauri/rust-toolchain.toml)） | 不能是 `-gnu`                                            |
| MSVC    | Visual Studio「使用 C++ 的桌面开发」工作负载 + Windows SDK                                                 | Windows 平台必需，约 6–8 GB                              |

> **顺序很重要**：先装 MSVC，再装 Rust。rustup 会依据机器上已有的工具链决定默认目标，顺序反了容易装成 `x86_64-pc-windows-gnu` 导致编译失败。
> 若 `rustup show` 显示 `-gnu`，执行 `rustup default stable-x86_64-pc-windows-msvc` 纠正。

### 构建步骤

```bash
git clone https://github.com/zkoreist/AnygoPet.git
cd AnygoPet
pnpm install
pnpm tauri dev      # 开发模式
pnpm tauri build    # 打包
```

`pnpm dev` / `pnpm build` 会先由 `scripts/buildIcon.ts` 调用 `tauri icon` 生成 `src-tauri/icons/`（该目录不入库），再启动 vite。

### pnpm 配置说明

从 pnpm v11 起，依赖的 postinstall 构建脚本默认被拦截，未审核的脚本会让安装以非零码退出（`ERR_PNPM_IGNORED_BUILDS`）。本项目的放行名单写在 [`pnpm-workspace.yaml`](./pnpm-workspace.yaml) 的 `allowBuilds` 字段，**该文件是必读配置，请勿删除**。

---

## 技术栈

| 层       | 选型                                                                                                                                                    |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 应用框架 | [Tauri 2](https://github.com/tauri-apps/tauri)                                                                                                          |
| 前端     | Vue 3 + TypeScript + Vite                                                                                                                               |
| 渲染     | PixiJS 8 + [easy-live2d](https://github.com/Panzer-Jack/easy-live2d)（角色为 Live2D `.moc3`）                                                           |
| 输入捕获 | [rdev](https://github.com/kunkunsh/rdev)（键鼠）、[gilrs](https://gitlab.com/gilrs-project/gilrs)（手柄）、`tauri-plugin-global-shortcut`（全局快捷键） |

---

## 上游资源

以下入口指向上游社区，**非本项目自营服务**，保留仅为方便用户获取和制作模型：

- 📖 [制作模型教程](https://juejin.cn/post/7509872655802269731)
- 🔄 [在线转换工具](https://bongocat.vteamer.cc)
- 📦 [Awesome-BongoCat 模型库](https://github.com/ayangweb/Awesome-BongoCat)

---

## 许可

[MIT](./LICENSE)。原始代码版权归 ayangweb 所有，本项目改动部分版权归 zkoreist 所有。详见 [NOTICE.md](./NOTICE.md)。

## 参与贡献

请先阅读 [贡献指南](./.github/CONTRIBUTING.md)。提交信息遵循 [Conventional Commits](https://www.conventionalcommits.org/)，已由 `commitlint` + `lint-staged` 在 pre-commit 钩子中校验。
