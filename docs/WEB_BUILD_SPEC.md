# SAVE ODYSSEUS - Web 构建规范

版本：0.1
目标：CrazyGames 网页、桌面和移动浏览器
引擎推荐：Phaser 3 + TypeScript + Vite，带有 DOM/CSS HUD 叠加

## 1. 为什么要使用这个堆栈

Phaser 3 很成熟，足够小，首包只需 15-18MB，与 sprite 图集和对象池配合良好，并通过 Canvas 后备支持 WebGL。 TypeScript 保持战斗、经济并保存键入的数据。 Vite 提供快速迭代和小型生产包。战场是用 Phaser 渲染的； HUD、菜单、设置和结果屏幕使用 DOM/CSS。这种混合使游戏在移动设备上具有可读性，并使本地化和响应式布局变得更容易。

首包时请勿使用 Unity WebGL。对于 CrazyGames 的第一个可玩目标来说，加载程序太重，并且 iOS 内存压力是一个风险。不要在战场上使用完整的 React 应用程序；仅 UI DOM 就足够了。

## 2. 浏览器和设备矩阵

支持：Chrome、Edge、Firefox、Safari 16+、三星互联网、Android Chrome、iOS Safari。

iframe 内的测试尺寸：907x510、1216x684、821x462、1366x768、1920x1080、800x450、1080x607、390x844、844x390、768x1024。

目标硬件：4GB Chromebook、Intel Celeron N4020、集成显卡、1280x720 时 60fps。移动中端：720p 时 60fps，颗粒减少；可接受的最低 30fps。

游戏不得需要 WebGL2。使用 Phaser AUTO 和 Canvas 后备。检测设备内存并减少低端设备上的粒子数、阴影质量和同步补间。

## 3.响应式布局

横屏是主要布局。战场以中心为中心，上方是大海，下方是船只。 HUD 使用安全区域插图。竖屏是受支持的布局，而不是旋转的画布。

风景：船体左上，风暴计时器中上，波浪右上，德拉克马左下，召唤下中，技能右下，暂停和静音右上。

肖像：海洋和航道填充上部55％，船体下部15％，中间5x5网格，底部动作栏有召唤和三个技能。在独奏期间，对手的幽灵面板隐藏在竖屏中；在异步决斗中，它变成一个紧凑的顶条。

HUD使用CSS网格；使用 Phaser Scale.RESIZE 并在调整大小时重新计算世界位置。切勿对 UI 使用固定像素坐标。保持触摸目标至少 44 像素。避免仅悬停交互。

## 4. 绩效预算
- 首包：15-18MB 压缩传输。
- 总包：v1 小于 40MB； CrazyGames 总限制 250MB。
- 文件：1500 个以下。
- 首次可玩：正常连接时不到 10 秒。
- 互动时间：12 秒以下。
- 4GB Chromebook 上的目标为 60fps；低端移动设备上的帧速率为 30fps。
- 绘制调用：每帧低于 120。
- 活跃精灵：250个以下；颗粒：桌面上小于 180，移动设备上小于 80。
- 音频：流音乐，仅预加载第一个岛屿的音乐和 SFX。
- 纹理图集：每个岛屿一个图集，加上一个共享的 UI 图集。
- 没有运行时 3D 模型。没有大的视频文件。网页字体的总大小不得超过 120KB。

## 5. 资产管道

源美术为 PNG 或 SVG；使用TexturePacker 或free-tex-packer 导出到纹理图集。将 WebP 用于大背景并以 PNG 为后备；使用 OGG 播放音乐和 SFX，并在 Safari 中使用 M4A 后备。为 SFX 保留单声道音频，为音乐保留立体声。将音乐的响度标准化为 -16 LUFS，将 SFX 的响度标准化为 -12 dBFS 峰值。

命名：`bg_<island>`、`unit_<type>_t<tier>`、`enemy_<type>_<anim>`、`boss_<name>_<anim>`、`ui_<component>_<state>`、`sfx_<event>`、`mus_<layer>`。

在路线图期间延迟加载岛屿资产；在主屏幕上预加载第一个岛屿。切勿阻挡所有岛屿上的第一道油漆。

## 6. 架构

使用六个场景：启动、预加载、主页、地图、战斗、结果。保持战斗场景的确定性和数据驱动。

核心模块：

- `GameState`：运行状态、元状态、设置、保存版本。
- `GridSystem`：5x5 格子、占用、放置、合成分辨率。
- `WaveSystem`：波定义、生成队列、精英修改器、Boss 阶段触发器。
- `CombatSystem`：瞄准、伤害、护甲、状态效果、射弹。
- `HeroSystem`：对线位置、自动攻击、三技能、冷却时间、终极充能。
- `EconomySystem`：德拉克马、召唤费用、波浪奖励、修复费用。
- `BoonSystem`：池子、权重、三项选择、运行修改器。
- `GhostSystem`：记录和重放异步运行。
- `SaveService`：CrazyGames 数据适配器加上本地回退。
- `AnalyticsService`：带SDK转发的类型化事件队列。
- `AudioService`：音乐层、SFX 池、静音状态、SDK 同步。
- `LocalizationService`：JSON 字典、SDK 语言环境、英语后备。

使用简单的事件总线来发送 UI 到游戏的消息。不要让 DOM UI 直接改变战斗状态；发送键入的命令，例如 `SUMMON`、`MERGE`、`MOVE_HERO`、`CAST_SKILL`、`CHOOSE_BOON`。

## 7. 数据驱动的内容

所有单位、敌人、波浪、祝福、boss、岛屿和奖励都存在于版本化的 JSON 或 TypeScript 数据文件中。场景中没有硬编码的余额数字。
每个内容记录都有一个 `id`、`displayNameKey`、`descriptionKey`、`tier`、`stats`、`tags` 和 `assetKey`。在构建时验证所有 ID。由于缺少本地化密钥或缺少资产而导致构建失败。

使用种子伪随机生成器进行波浪组合和福利优惠。将种子存储在运行记录中，以便可以重现运行以进行调试和重放。

## 8. 决定论和幽灵重放

幽灵是一条记录的时间线，而不是重新模拟的AI对手。以 10Hz 记录：时间戳、船体、波浪、boss HP、单位放置、合成、英雄航道、技能施放和祝福选择。通过在快照之间进行插值并触发相同的视觉事件来重播。这避免了跨设备的物理差异。

幽灵记录的上限为 120KB。将玩家最好的幽灵存储在本地，仅在稍后添加后端时才上传。首发时，使用精心策划的机器人幽灵和本地玩家幽灵；无需服务器。

## 9. 保存数据

CrazyGames 数据模块是主要的保存模块。 LocalStorage 是一种后备方案。每次运行、升级、codex 解锁和设置更改后保存； debounce 每 2 秒写入一次。

保存架构：

- `version`：整数。
- `meta`：德拉克马、解锁岛屿、星星、船员碎片、船员解锁、上帝奉献、船舶升级、化妆品、成就。
- `settings`：语言、音乐、音效、简化动作、色盲模式。
- `records`：每个岛屿的最佳运行次数、最佳幽灵、总运行次数、总胜利次数。
- `flags`：教程完成，首次获胜，决斗解锁。

迁移按版本保存。如果迁移失败，请在 localStorage 中保留备份并启动安全默认值而不是崩溃。切勿存储个人数据。

## 10.CrazyGames SDK 集成

在第一个场景之前初始化SDK。如果SDK不可用，请以本地回退模式运行并禁用广告和云保存。

所需调用：

- `SDK.init()`
- `SDK.game.loadingStart()` 和 `SDK.game.loadingStop()`
- `SDK.game.gameplayStart()` 在运行中第一个玩家输入
- `SDK.game.gameplayStop()` 暂停、结果、选项卡隐藏或对局结束
- `SDK.data` 用于保存/加载
- `SDK.ad.requestAd('midgame')` 仅针对结果/下一个
- `SDK.ad.requestAd('rewarded')` 仅在明确的用户操作后
- `SDK.game.happytime()`上的boss杀和第一个清除
- `SDK.game.muteAudio()` / 取消静音
- `SDK.user`可选；永远不需要登录

战斗中请勿调用广告。不要自动播放激励广告。不要使用自定义全屏。请勿链接到外部商店或网站。尊重 iframe 焦点和音频权限。

## 11. 本地化
英语是启动语言。所有字符串都使用键；场景中没有硬编码的英语。使用 ICU-lite 进行多元化。保持 UI 字符串简短；德语/俄语/葡萄牙语版本可能会扩展30%。

CrazyGames 区域设置只是一个提示，而不是保证。回到英语。请勿在未经审查的情况下对启动版本进行机器翻译。只有在数据显示需求后，第一批增加的语言应该是葡萄牙语（巴西）、西班牙语（拉丁美洲）、德语、法语和印地语/孟加拉语。

## 12. 音频和焦点

浏览器阻止自动播放。仅在第一个用户手势后才开始音乐。隐藏选项卡时静音。尊重 CrazyGames 静音状态。在Boss介绍和结果期间鸭子音乐。使用单个主增益节点；没有每帧音量变化。

## 13. 错误处理

- 如果SDK失败：本地模式，无广告，无云保存，不向播放器显示错误。
- 如果资产失败：重试一次，然后显示紧凑的“重新加载”面板。
- 如果保存失败：保留内存状态，在下一个检查点重试，绝不擦除进度。
- 如果WebGL上下文丢失：暂停运行，恢复并从最后一个快照恢复；如果恢复失败，请重新启动。
- 如果帧速率低于 30fps 持续 5 秒：减少颗粒、禁用屏幕抖动并将目标分辨率降低 15%。

## 14. 质量检查清单

- 在没有滚动条的 907x510 iframe 中加载。
- 首次在冷缓存上可在 10 秒内播放。
- 所有按钮均只能通过触摸操作；没有悬停要求。
- 竖屏和横向不会剪辑 HUD。
- 暂停会停止游戏并调用 `gameplayStop`。
- 仅在结果/下一个上显示广告；仅在点击后才会获得奖励广告。
- 保存在重新加载和第二个选项卡后仍然存在。
- 静音持续并尊重 SDK。
- 无控制台错误；没有未处理的承诺拒绝。
- 4GB Chromebook 上为 60fps；在中端手机上为 30fps。
- 30 分钟浸泡后无崩溃率超过 99.5%。

## 15. 构建和部署

使用 Vite 生产构建。通过 Brotli 压缩从静态主机提供服务。将哈希资产缓存一年； HTML 无缓存。使用暂存 URL 进行 CrazyGames 审核。保留 `build-info.json` 的版本、提交和资产哈希值。

CI：安装、类型检查、lint、战斗/经济单位测试、构建、包大小检查、907x510 和 390x844 的 Playwright 冒烟测试。拒绝超过 18MB 或缺少资产的构建。

## 16. 垂直切片验收
- 一次完整的对局，从主场到结果再到主场。
- 召唤、合成、英雄移动、三技能、两恩赐、独眼巨人boss、胜败。
- 5x5 网格，三航道，三个船体部分，风暴红色状态。
- 英文 UI，15 秒教程，保存/加载。
- SDK 初始化、加载、游戏开始/停止、静音、结果中的一则游戏中广告。
- 15-18MB 构建；首次播放时间不超过 10 秒； Chromebook 上为 60 fps。
## 17. Three build targets

> 三个版本：移动端中文版、移动端英文版、PC 网页英文版。共享核心玩法，不做玩法分叉。

Build one core with three release configurations:

- `build:web-en` - desktop browser, CrazyGames, English, landscape.
- `build:mobile-en` - iOS/Android Capacitor wrapper, English, portrait.
- `build:mobile-cn` - iOS/Android China package, Simplified Chinese, portrait, China SDK/payment/compliance adapters.

Shared: combat, grid, merge, waves, bosses, boons, economy, save schema, ghost format, analytics events.

Platform-specific: UI shell, input, language bundle, SDK adapter, lifecycle, safe areas, app-store or channel packaging, ads, IAP, privacy, and performance caps.

Build flags: `RELEASE_TARGET`, `LOCALE`, `PLATFORM`, `MONETIZATION`, `SDK`. Never fork combat code for a platform. If a platform needs a different value, use a config flag.

Budgets: PC Web-EN 15-18MB first build; Mobile-EN/Mobile-CN 40-60MB install, under 100MB total; mobile first launch under 15 seconds on 4G. PC Web 60fps on a 4GB Chromebook; mobile 60fps target and 30fps floor.

See `docs/PLATFORM_VERSIONS.md`.
