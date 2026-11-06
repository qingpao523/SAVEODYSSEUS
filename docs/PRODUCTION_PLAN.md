# 拯救奥德修斯 - 制作计划

版本：0.1
目标：3周内可玩CrazyGames垂直切片； 8 周内基本启动； 12 周内全面启动。

## 1.团队

最低团队：1 名游戏设计师、1 名游戏工程师、1 名 UI/UX 设计师、1 名 2D 美术师/动画师、1 名音频承包商、1 名 QA/通才。在此范围内，生产者是可选的。设计师拥有 GDD、资产负债表和游戏测试记录。工程师拥有构建、SDK 和性能。

## 2. 范围规则

MVP 是一个岛屿、一个 Boss、五个船员单位、六个祝福、一个英雄、六波、一种单人模式、一场异步决斗、一艘船、三个船体部分和一个风暴计时器。其他任何事情都是发布后的想法。

如果某个功能不能让前 90 秒更清晰，或者 Boss 高潮更好，那就删掉它。

## 3. 里程碑

### M0 - 设计锁（第 1 周）

可交付成果：GDD v1.0、资产负债表 v0.1、美术风格测试、标题/商标搜索、SDK 测试页。

退出：团队可以在一分钟内解释循环；没有任何悬而未决的问题会阻碍工程。

### M1 - 核心原型（第 1-2 周）

可交付成果：5x5 网格、召唤、合成、三线、一名英雄、长矛、一名敌人、一波、船体、风暴计时器、获胜/失败占位符。

退出：10次内部播放；合成和航道移动无需解释即可阅读。

### M2 - 垂直切片（第 3 周）

可交付成果：一次完整的运行，独眼巨人 boss，五个单位，三项技能，两个祝福，教程，结果屏幕，保存/加载，SDK 初始化。

退出：10名外部玩家； 70%能够解释目标和合成规则；首次可玩 <10 秒； Chromebook 上为 60 fps。

### M3 - 内容和平台（第 4-5 周）

可交付成果：六波、六种祝福、六种敌人类型、异步幽灵决斗、英文 UI、设置、静音、分析、封面、15 秒预告片、CrazyGames 构建。

退出：QA 检查表通过；构建15-18MB；没有控制台错误。

### M4 - 基本启动（第 6-8 周）

可交付成果：CrazyGames 提交、启动构建、仪表板、修补程序计划。

退出：500+播放；转化率80%+；首轮完成率 65%+；第二轮开始50%+；崩溃率低于0.5%。

### M5 - 全面启动（第 9-12 周）

可交付成果：三个岛屿、三个Boss、十二名船员、十二个祝福、每日海上、每周排行榜、化妆品套装。

退出：D1 10-15%；平均会话10分钟以上；积极的评论情绪；不卖数值的投诉。

## 4.每周节奏

周一：构建审核和指标审核。周三：游戏测试和平衡性通过。周五：内容冻结和质量检查。发布周的周三之后没有新的范围。

## 5. 完成的定义
当一项功能可播放、本地化、检测、正确保存、在桌面和移动设备上进行测试、记录并具有后备功能时，该功能就完成了。没有分析的功能就没有完成。

## 6. Risks

|风险|概率 |影响 |缓解措施 |
|---|---|---|---|
| Title conflict |中等|高|商标检索；备份名称 NOSTOS，爱琴海誓言 |
|范围蔓延 |高|高| MVP one island;每周剪辑回顾|
| Mobile UI clutter |中等|高| 5x5格子，三种技能，无库存|
|美术品太贵了|中等|中等|分层口音、重复使用轮廓、一个岛屿 |
| Boss too hard |中等|中等| 20 次模拟运行； tune HP and telegraphs |
| SDK/广告拒绝 |低|高| Follow CrazyGames rules; test in iframe early |
| Low second-run rate |中等|高|结果提示、更快重启、每日目标 |

## 7. Launch checklist

- CrazyGames SDK 在 iframe 中初始化和测试。
- 无外部链接，无自定义全屏，战斗中无广告。
- English UI;所有字符串均已外部化。
- PEGI 12内容审查；没有血腥、没有色情内容、没有赌博。
- 15-18MB 构建； 10秒以下的冷负载。
- Desktop and mobile QA;肖像和风景。
- 保存经过测试的迁移和回退。
- 分析事件触发； dashboard live.
- 支持游戏页面中的电子邮件/联系方式，而不是游戏画布中。
- 备份构建和回滚计划。

## 8. Post-launch

第一周：观察崩溃率、教程完成率、首次运行完成率、二次运行率。 Do not add content.
第2周：修复前三个摩擦点； tune one variable.
第 3-4 周：如果保留率良好，则添加一座岛屿和一名 Boss。
仅当异步决斗保留率很高并且后端预算获得批准时才添加真正的 PvP。
## 9. Three release targets

> 三个版本：移动端中文版、移动端英文版、PC 网页英文版。共享核心玩法，不做玩法分叉。

The production plan covers three packages:

1. **PC Web-EN** (P0): CrazyGames validation, weeks 1-8 Basic Launch, weeks 9-12 Full Launch.
2. **Mobile-EN** (P1): Capacitor wrapper, portrait UI, global stores, weeks 9-16.
3. **Mobile-CN** (P2): Simplified Chinese, China Android channels first, compliance, weeks 13-20.

Shared core is frozen before mobile packaging starts. Do not fork gameplay. China compliance (game approval, real-name, anti-addiction, payment, privacy) is the long pole and must start before the mobile build is finished. See `docs/PLATFORM_VERSIONS.md`.
