# QuotaNotch

<p align="center">
  <img src="assets/brand/app-icon.png" width="128" alt="QuotaNotch 应用图标">
</p>

<p align="center">
  <strong>移动一下指针，即可查看 Codex 用量。</strong><br>
  一款原生 macOS 助手，把额度与可选活动摘要放在 MacBook 刘海旁。
</p>

<p align="center">
  <a href="README.md">English</a> · <strong>简体中文</strong> · <a href="README.zh-Hant.md">繁體中文</a>
</p>

> [!IMPORTANT]
> **公开预览状态：**目前 [GitHub Releases](https://github.com/GuojianSun/QuotaNotch/releases) 中还没有 QuotaNotch 安装包。第一个可下载版本将标记为公开预览版，暂不具备 Apple Developer ID 签名，也未通过 Apple 公证。届时安装前，请先阅读[安全安装公开预览版](#安全安装公开预览版)。

QuotaNotch 可从 MacBook 的物理刘海位置展开，让当前每周 Codex 额度始终靠近你的工作流。没有刘海或使用外接显示器时，也可以通过菜单栏打开。面板既能保持紧凑，也能展开为可配置的 Token、协作和里程碑模块。

## 界面预览

### 展开的活动视图

![QuotaNotch 简体中文深色展开面板](assets/screenshots/zh-Hans/panel-expanded-dark.png)

### 白灰主题

![QuotaNotch 简体中文白灰主题展开面板](assets/screenshots/zh-Hans/panel-expanded-light.png)

### 可配置的数据模块

![QuotaNotch 简体中文数据模块设置](assets/screenshots/zh-Hans/settings-data-modules.png)

<details>
<summary><strong>更多设置</strong></summary>

#### 交互

![QuotaNotch 简体中文交互设置](assets/screenshots/zh-Hans/settings-behavior.png)

#### Codex 连接验证

![QuotaNotch 简体中文 Codex 可执行文件验证](assets/screenshots/zh-Hans/settings-codex-connection.png)

这里验证的是用户选中的 Codex 可执行文件，不代表 OpenAI 审查或背书 QuotaNotch。

</details>

以上图片直接截取自真实的 SwiftUI/AppKit 界面。面板数值是作者在截图时的本机聚合状态，会与每位用户的当前账户数据不同；横线表示当时上游未提供该字段。图片不包含对话内容。

## 核心体验

- 指针悬停在刘海处即可打开，也可随时使用菜单栏入口。
- 查看每周限制、剩余比例、准确重置时间、最近更新状态，以及 Codex 返回的方案名称。
- 展开或收起数据区域时，顶部标题栏保持在固定位置。
- 可选择紧凑、标准、宽松三档宽度，跟随系统、深色、白灰三种主题，并自定义模块顺序。
- 可将面板拖离刘海，也可移动回物理刘海位置。
- 可调整登录时启动、悬停延迟和指针离开后是否自动收起。
- 支持 English、简体中文和繁體中文。
- 遵循 macOS 的“减少动态效果”和“降低透明度”辅助功能设置。

## 模块与数据来源

| 模块 | 展示内容 | 来源与行为 |
| --- | --- | --- |
| **每周用量** | 已用与剩余比例、重置时间、方案名称，以及符合条件时的重置卡提示 | 必选模块。通过本机启动的 Codex app-server，使用用户已有的 Codex 登录请求数据。QuotaNotch 不会使用重置卡；按钮只会打开 Codex 官方用量页面。 |
| **Token 活动** | 近 7 日热力图、累计 Token 和本周 Token | 可选展示模块。读取 Codex `account/usage/read` 返回的聚合数据；能否取得及更新速度取决于已安装的 Codex 版本和账户侧可用性。响应只保留在内存中，不由 QuotaNotch 持久化。 |
| **今日协作** | 今日累计协作时长，以及可选的有效段数 | 默认关闭的本机模块。定期扫描已有 Codex rollout 文件中的终态和计时元数据，不使用实时计时器。 |
| **累计协作** | 本机可追溯历史中的协作时长和有效段数 | 与“今日协作”共用可选的本机台账；只能追溯仍存在且格式兼容的本地 rollout 历史。 |
| **使用里程碑** | 当前连续、最长连续、单日峰值与最长任务 | 可选模块，来源为 `account/usage/read` 返回的聚合摘要；缺失字段显示为空，不进行推算。 |

每周用量始终保留在紧凑面板中。其他模块只在展开后出现，并可在设置中启用和重新排序。

## 本机协作如何计算

协作统计**默认关闭**。开启后，QuotaNotch 会在需要时于启动阶段扫描；运行期间约每 15 分钟扫描一次，并会在电脑唤醒及手动刷新时更新。

- 只导入任务终态标识、完成时间、日志记录的耗时和终态类型。
- 已完成和失败的任务计为一段；被中断的任务只有耗时超过 20 分钟才计为一段，但其已记录耗时仍可计入总时长。
- 跨越午夜的任务会按与每个自然日重叠的部分拆分时长；段数归入任务完成的日期。
- 重复终态事件会去重；冲突或不可用记录会被排除，不会猜测补全。
- 子代理 rollout 文件会被排除。
- 任务标识和来源路径在写入本机台账前会被哈希处理。
- 提示词、回复、任务标题、最终回答和工作目录都会被忽略，也不会写入台账。

这是一项本机效率摘要，不是 OpenAI 官方指标，也不是计费记录。QuotaNotch 关闭时不会扫描或运行实时计时器；重新打开后，可以导入磁盘上仍然存在且格式兼容的历史记录。

## 系统要求

- macOS 14 Sonoma 或更高版本
- Apple 芯片（M1 或更新）；首个预览版暂不计划提供 Intel 或 Universal 2 构建
- 已登录 Codex
- OpenAI 签名的 Codex 或 ChatGPT macOS 应用所提供的 Codex 可执行文件，或由你主动选择并批准的其他 Codex 可执行文件

物理刘海不是必须条件：刘海悬停交互需要物理刘海，菜单栏入口可在其他受支持的显示环境中使用。

## 当前发布状态

QuotaNotch 暂不提交 Mac App Store。计划通过本仓库的 [Releases 页面](https://github.com/GuojianSun/QuotaNotch/releases)发布，后续版本再由应用内 Sparkle 更新。

当前阶段：

- 尚未上传公开 DMG；
- 首个公开预览版计划暂不使用 Developer ID 签名，也不经 Apple 公证；发布前还需要建立并验证专用的公开预览构建链路；
- macOS 因此可能阻止首次打开，或显示无法验证开发者的提示；
- Developer ID 签名和 Apple 公证属于后续发行加固工作。

如果你的安全规范不允许安装未使用 Developer ID 签名、未经 Apple 公证的软件，请等待未来完成签名和公证的版本。

## 安全安装公开预览版

以下步骤仅适用于官方 Releases 页面出现预览版 DMG 之后。

1. 从同一条 GitHub Release 下载 DMG 及其 SHA-256 校验值。不要安装来自镜像站、Issue 附件或网盘链接的副本。
2. 在本机验证下载文件，并替换为真实路径：

   ```bash
   shasum -a 256 "/path/to/QuotaNotch.dmg"
   ```

3. 将完整的 64 位结果与该 Release 公布的校验值逐字比较；不一致就立即停止。
4. 打开 DMG，把 **QuotaNotch** 拖入“应用程序”，然后先按正常方式尝试打开一次。
5. 如果 macOS 因无法验证开发者而阻止打开，请按 [Apple 针对单个 App 的官方步骤](https://support.apple.com/zh-cn/guide/mac-help/mh40617/mac)操作：进入**系统设置 → 隐私与安全性**，找到 QuotaNotch 提示并点击**仍要打开**。只有在确认应用和 Release 来源确实可信后才继续。

请勿全局关闭 Gatekeeper，也不要用 `xattr` 删除隔离属性。使用针对单个应用的“仍要打开”例外，可以保留 macOS 对其他下载内容的正常保护。

### SHA-256 能证明什么，不能证明什么

SHA-256 一致，说明下载的 DMG 与公布校验值所代表的文件逐字节相同，可以发现下载不完整或文件被意外修改。它**不能**证明开发者的法律身份，不能替代 Apple 代码签名或公证，也无法抵御攻击者同时替换 Release 文件和校验值的情况。因此还必须核对仓库所有者和 Release 地址。

## 更新安全

QuotaNotch 的发行流程按 [Sparkle](https://github.com/sparkle-project/Sparkle)、HTTPS appcast 和固定 EdDSA 公钥进行配置。只有在公开构建中内置更新地址和公钥后，自动更新才会启用。此时，只有更新包的 EdDSA 签名能通过该内置公钥验证，Sparkle 才会接受它。更新请求已关闭系统信息画像。

在初始应用及其内置公钥可信的前提下，EdDSA 可以阻止自动更新器接受未由对应私钥签名的软件包。它与 SHA-256、Apple Developer ID 是三套不同机制：EdDSA 不会让应用获得 Apple 公证，不会向 macOS 证明开发者身份，也不能消除缺少 Developer ID 签名时的 Gatekeeper 提示。

在首个 Release 和生产 appcast 发布前，更新服务还没有可安装内容。

## 隐私

QuotaNotch 不含分析或广告 SDK，不进行账户跟踪，也不会自动上传崩溃报告。

| 数据 | 处理方式 |
| --- | --- |
| Codex 身份验证 | 隔离的本机 Codex home 会链接到已有 `auth.json`，供选定的 Codex 进程使用当前登录。QuotaNotch 不解析、不复制、也不传输令牌。 |
| 每周额度与重置数据 | 由本机启动的 Codex 进程向 OpenAI 请求，并以私有文件权限缓存在本机，用于显示最近一次有效数据。 |
| Token 活动 | 仅在内存中展示聚合账户数值，不由 QuotaNotch 持久化。 |
| 协作历史 | 默认关闭。开启后，本机台账只保存哈希标识、计时和终态元数据，不保存对话文本；可随时在设置中清除。 |
| 更新 | Sparkle 请求 HTTPS feed 时不附带 Codex 令牌、用量数据、账户标识、分析标识或系统画像。 |

QuotaNotch 本身不会上传本机协作元数据。Codex app-server 子进程仍会作为用户现有 Codex 会话的一部分与 OpenAI 通信。详见完整的[隐私政策](PRIVACY.md)。

## 已知限制

- 首个预览版没有 Developer ID 签名或 Apple 公证。
- 初期仅支持 Apple 芯片上的 macOS 14 或更高版本。
- 额度和 Token 字段依赖本机 Codex app-server 及账户侧数据契约；Codex 或服务更新可能导致字段暂时不可用。
- Token 活动可能晚于最新对话，旧版 Codex 也可能不提供该数据。
- 协作总量只能从这台 Mac 上仍存在且兼容的 rollout 文件中恢复；已删除、缺失、截断或旧格式不兼容的历史无法重建。
- 协作时长来自元数据，不代表窗口前台时间、键盘操作时间或模型实际计算时间。
- QuotaNotch 未启用 App Sandbox，因为它需要启动本机 Codex 可执行文件，并在用户主动开启后读取本机 Codex 任务元数据；发行构建仍启用 Hardened Runtime。
- 当前支持移动面板，但完整的四边停靠和边缘唤出不属于首个预览版。

## 常见问题

<details>
<summary><strong>Mac 必须有刘海吗？</strong></summary>

不需要。物理刘海用于顶部悬停打开；在没有刘海的受支持 Mac 或外接显示器上，仍可使用菜单栏入口。

</details>

<details>
<summary><strong>为什么 macOS 提示无法验证开发者？</strong></summary>

首个公开预览版计划在尚未加入 Apple Developer Program 时发布，因此不具备 Developer ID 签名，也未经 Apple 公证。请核对官方 Release 地址和 SHA-256 后，再按上文使用单应用的“仍要打开”。如果提示内容不同、校验失败或文件来自其他来源，请停止安装并报告。

</details>

<details>
<summary><strong>QuotaNotch 会使用重置卡吗？</strong></summary>

不会。Codex 返回相关数据时，它可以显示可用或即将过期的重置卡数量；点击标签只会在浏览器中打开 Codex 官方用量页面。

</details>

<details>
<summary><strong>为什么 Token 或里程碑数据显示为空？</strong></summary>

这些模块使用补充的 Codex 账户用量接口。旧版 Codex、特定账户或上游数据仍在更新时，接口或某个字段可能不可用；QuotaNotch 会显示横线，不会虚构数值。

</details>

<details>
<summary><strong>关闭 QuotaNotch 后，协作统计还会继续运行吗？</strong></summary>

不会。它采用定期元数据扫描，不是实时后台计时器。重新打开后，可以导入 Codex 在关闭期间保留的兼容 rollout 记录，并通过已知任务标识避免重复计入。

</details>

<details>
<summary><strong>这个仓库是开源项目吗？</strong></summary>

不是。本仓库用于公开分发、文档和支持；QuotaNotch 的实现源码保持私有，官方二进制适用专有的 [QuotaNotch 二进制许可](BINARY-LICENSE.md)。

</details>

## 路线图

- 发布首个 Apple 芯片公开预览版 DMG，并附发行说明和 SHA-256 校验值。
- 根据公开预览反馈验证安装、数据兼容性、本地化和 Sparkle 更新。
- 将测试通过的同一构建从测试通道晋升到正式通道，不重新构建被晋升的安装包。
- 在项目条件合适时补充 Developer ID 签名和 Apple 公证。
- 根据实际需求重新评估 Intel/Universal 2 支持和完整边缘停靠。

路线图仅表示方向，不承诺日期；已经交付的行为以[更新日志](CHANGELOG.md)为准。

## 支持与报告

- 可通过 [GitHub Issues](https://github.com/GuojianSun/QuotaNotch/issues)提交可复现错误、兼容性报告和功能建议。
- 请附上 macOS 版本、Mac 型号、QuotaNotch 版本、Codex 安装类型和复现步骤。
- 请勿公开 `auth.json`、访问令牌、私有路径、对话内容或原始 rollout 文件。
- 安全漏洞请按照[安全政策](SECURITY.md)私下报告。

## 许可与归属

QuotaNotch 是以编译后二进制形式分发的专有软件，实现源码保持私有且不包含在本仓库中。官方二进制受 [QuotaNotch 二进制许可](BINARY-LICENSE.md)约束。

文档、截图、美术素材和品牌标识受[版权声明](COPYRIGHT.md)保护；内嵌第三方组件继续适用各自许可证，详见[第三方声明](THIRD_PARTY_NOTICES.md)。

Copyright © 2026 Guojian Sun. All rights reserved.

---

QuotaNotch 是独立项目，**并非 OpenAI 官方产品，与 OpenAI 不存在隶属、赞助、背书或其他关联关系**。OpenAI、Codex 与 ChatGPT 是 OpenAI 的商标。
