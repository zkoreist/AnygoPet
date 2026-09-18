# 声明 / NOTICE

## 项目来源

本项目 fork 自 [ayangweb/BongoCat](https://github.com/ayangweb/BongoCat)。

- 原作者：ayangweb
- 原始许可证：MIT License
- 原始版权声明与许可条款完整保留在 [LICENSE](./LICENSE) 中，未作任何修改。

感谢原作者的开源工作。本项目在其基础上进行独立开发与改造。

## 关于默认角色模型

**重要说明**

`src-tauri/assets/models/` 目录下的默认角色模型（standard / keyboard / gamepad 三套，Live2D `.moc3` 格式）继承自上游仓库，其美术授权来源尚待澄清。

已知事实：

- Bongo Cat 这一形象最早由画师 @StrayRogue 于 2018 年创作，配乐改编来自 @DitzyFlama
- 上游仓库代码以 MIT 协议发布，但 MIT 通常不自动覆盖第三方美术作品的著作权
- 上游仓库未单独声明这些模型的美术授权

本项目的处理原则：

1. 现阶段保留默认模型，**仅用于本地开发与功能验证**
2. 在本项目对外正式发布（v1.0）之前，必须替换为可合法分发的原创素材或明确授权的素材
3. 在此之前，不建议将本项目用于任何商业用途
4. 若权利人提出异议，将立即移除相关素材

## 第三方依赖与授权

本项目依赖大量第三方开源库与 Tauri 插件，各自授权以其原始仓库为准。主要项：

| 组件 | 授权 |
|---|---|
| Tauri 2 | MIT / Apache-2.0 |
| Vue 3 / Vite / TypeScript | MIT / Apache-2.0 |
| PixiJS | MIT |
| easy-live2d | 见其仓库声明 |
| **Live2D Cubism Core** | **专有许可（Live2D Inc.）**，商用需遵守其条款，营收超过阈值需付费授权 |

完整依赖清单见 `package.json` 与 `src-tauri/Cargo.toml`。

## 联系

如对本声明内容有任何疑问或权利主张，请通过本仓库 Issue 联系维护者。
