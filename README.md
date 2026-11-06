# 拯救奥德修斯

**奥德赛防御** - 一款网页优先的合成、航道防御、英雄动作游戏，具有轻度 Roguelike 神赐祝福。

> 你是奥德修斯。合成船员，召唤神明，守住船只，然后将所有人带回伊萨卡岛。

## 状态

前期制作设计包 v0.1。该仓库当前包含设计基线，而不是最终的游戏版本。

## 核心体验

- 3-4 分钟的对局：六波、一个 Boss、一个风暴计时器。
- 在 5x5 甲板/海滩网格上合成船员。
- 在三个航道之间移动奥德修斯；使用长矛、圣盾和雅典娜之光。
- 在第 2 波和第 4 波之后选择神赐祝福。
- 在风暴中撑住，并在船体破裂前击败 Boss。
- 首发时异步1v1；没有真 PvP。

## 设计文档

|文件|目的|
|---|---|
| [docs/GDD.md](docs/GDD.md) |完整的游戏设计：循环、系统、模式、用户体验、内容范围 |
| [docs/WEB_BUILD_SPEC.md](docs/WEB_BUILD_SPEC.md) | Web 架构、CrazyGames SDK、性能、响应式布局 |
| [docs/CONTENT_BIBLE.md](docs/CONTENT_BIBLE.md) |世界、岛屿、Boss、船员、神明、祝福、敌人 |
| [docs/BALANCE.md](docs/BALANCE.md) |波表、单位统计、经济、难度、奖励曲线 |
| [docs/ART_AUDIO_BIBLE.md](docs/ART_AUDIO_BIBLE.md) |黑陶美术指导、UI、动画、音频 |
| [docs/PRODUCTION_PLAN.md](docs/PRODUCTION_PLAN.md) | 12 周计划、团队、里程碑、风险、启动清单 |
| [docs/ANALYTICS_KPIS.md](docs/ANALYTICS_KPIS.md) |事件模式、漏斗、KPI、A/B 测试 |
| [docs/LEGAL_IP.md](docs/LEGAL_IP.md) |公有领域边界、商标风险、原创性规则 |

## 平台目标

- CrazyGames 网页优先、桌面和移动浏览器。
- 首包 15-18MB；首次可玩 <10 秒。
- 英文 UI，PEGI 12，无血腥。
- 仅在对局结束时显示广告；不卖数值；不做体力；没有登录墙。

## 版权与 IP

仅使用荷马的《奥德赛》和希腊神话。没有现代改编，没有*刺客信条奥德赛*，没有*超级马里奥奥德赛*，没有复制的美术，音乐，UI或文本。

工作英文标题是**SAVE ODYSSEUS**。 《奥德赛》是旅程； “奥德修斯”就是这个人。发布前需要进行商标检索。

## 后续步骤

1.在`docs/GDD.md`中审核并冻结MVP。
2. 从 `docs/PRODUCTION_PLAN.md` 构建两周垂直切片。
3. 使用 `docs/ANALYTICS_KPIS.md` 运行第一个 10 人理解测试。
4、垂直切片通过后申请CrazyGames Basic Launch。
## Language note

The Chinese documents are the working translation for review. The English source drafts are in [`docs/en/`](docs/en/) and are the source of truth for terminology, UI copy, and implementation details.
