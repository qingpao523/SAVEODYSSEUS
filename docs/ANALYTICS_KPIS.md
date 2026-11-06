# SAVE ODYSSEUS - 分析和 KPI

版本：0.1

## 1. 北极星指标

**第二局开始率**：在同一会话中完成一轮并开始另一轮的玩家的百分比。它衡量核心循环是否有趣，而不是商店页面是否有吸引力。

次要北极星：**平均会话长度**。目标 10-15 分钟。

## 2. 启动 KPI

|关键绩效指标|目标|淘汰阈值|
|---|---:|---:|
|CrazyGames转换| 80%+ |低于 65% |
|首次可玩| 10 秒以内 | 15 秒以上 |
|教程完成 | 85%+ |低于 70% |
|首局完成 | 65%+ |低于 50% |
|第二局开始 | 55%+ |低于 35% |
|平均会话时长 | 10 分钟以上 | 5 分钟内 |
| D1 留存 | 10-15% |低于 6% |
|无崩溃会话 | 99.5%+ | 98%以下|
|激励广告选择率| 20-35% |低于 10% |

## 3. 事件架构

所有事件包括`session_id`、`run_id`、`player_id`（匿名）、`build`、`locale`、`device`、`orientation`、`timestamp`。

核心事件：

- `app_open`
- `home_play_click`
- `map_enter`
- `run_start`（岛屿、守护神、配置）
- `tutorial_step`（步骤，跳过）
- `summon`（数量、成本、单位类型）
- `merge`（单位类型、from_tier、to_tier、tile）
- `hero_move`（航道）
- `skill_cast`（技能、航道、目标计数）
- `wave_start` / `wave_end`（波次、船体、德拉克马、时间）
- `boon_offer` / `boon_pick`（神明，祝福，替代品）
- `boss_start` / `boss_phase` / `boss_end`
- `hull_hit`（航道、源、hull_left）
- `run_end`（结果、持续时间、波次、boss_hp、船体、德拉克马、原因）
- `result_next_click`
- `ad_request` / `ad_started` / `ad_completed` / `ad_error` (放置)
- `upgrade_purchase`（升级、等级、费用）
- `codex_unlock`
- `settings_change`

不要记录个人数据。如果不需要，请勿记录完整的用户代理字符串。

## 4. 漏斗

漏斗A：app_open -> home_play_click ->tutorial_step_1 ->tutorial_step_2 ->first_summon ->first_merge ->first_win ->result_next_click ->second_run_start。

漏斗B：run_start -> wave_5 -> boss_start -> boss_end_win -> result_next_click。

漏斗C：结果->rewarded_ad_offer->ad_started->ad_completed->next_run。

## 5. 诊断问题
- 如果教程完成度较低，是第一个合成提示不清楚还是按钮太小？
- 如果首局完成率较低，损失原因是否明显？按航道检查 `run_end.cause` 和船体命中情况。
- 如果第二局开始很低，结果屏幕是否太慢，下一个按钮不清楚，或者第一个岛太难受？
- 如果boss起始较高但boss获胜较低，是否是风暴计时器太紧或眼睛窗口不清晰？
- 如果移动会话远低于桌面，请检查方向、触摸目标大小和帧速率。

## 6. A/B 测试

一次测试一个变量，每组至少对局 500 次。

1. 教程长度：15s vs 25s。
2. 首次召唤：免费3单位 vs 免费2单位+10德拉克马。
3. Boss时间：2:20 vs 2:35。
4、结果界面：一键“下一步”vs地图返回。
5. 封面美术：奥德修斯肖像与独眼巨人/船舶构图。

在留存率稳定之前，请勿测试货币化。

## 7. 仪表板

每日：转化、首次可玩、教程完成、首次对局完成、第二次对局率、会话时长、崩溃率。
每周：岛屿完成度、Boss 获胜率、单位选择率、合成层分布、祝福选择率、广告选择加入、D1。
同类群组：首次使用与回访、桌面与移动、横向与竖屏。

## 8. 仪器质量检查

每个事件都必须在开发中的调试控制台中可见。在暂存过程中，验证事件触发一次且仅触发一次。使用类型化的事件图；拼写错误是构建错误。切勿发送带有未定义字段的事件。
## 9. Release-target dimensions

Every event includes `release_target`:

- `web_en` - PC Web-EN / CrazyGames.
- `mobile_en` - global iOS/Android.
- `mobile_cn` - China iOS/Android.

Also include `locale`, `platform`, `app_version`, `orientation`, and `build_channel`. Report the core funnel separately for each release target. Do not blend China mobile activation with CrazyGames conversion; the traffic sources and definitions differ.

Primary comparison: tutorial completion, first-run completion, second-run start, session length, crash rate, and ad/IAP opt-in by target. Cross-target cloud save and leaderboards are out of scope for launch.
