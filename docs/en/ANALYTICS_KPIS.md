# SAVE ODYSSEUS - Analytics and KPIs

Version: 0.1

## 1. North-star metric

**Second-run start rate**: the percentage of players who finish one run and start another within the same session. It measures whether the core loop is fun, not whether the store page is attractive.

Secondary north star: **average session length**. Target 10-15 minutes.

## 2. Launch KPIs

| KPI | Target | Kill threshold |
|---|---:|---:|
| CrazyGames conversion | 80%+ | under 65% |
| First playable | under 10s | over 15s |
| Tutorial completion | 85%+ | under 70% |
| First-run completion | 65%+ | under 50% |
| Second-run start | 55%+ | under 35% |
| Average session | 10+ min | under 5 min |
| D1 retention | 10-15% | under 6% |
| Crash-free sessions | 99.5%+ | under 98% |
| Ad opt-in (rewarded) | 20-35% | under 10% |

## 3. Event schema

All events include `session_id`, `run_id`, `player_id` (anonymous), `build`, `locale`, `device`, `orientation`, `timestamp`.

Core events:

- `app_open`
- `home_play_click`
- `map_enter`
- `run_start` (island, patron, loadout)
- `tutorial_step` (step, skipped)
- `summon` (count, cost, unit_types)
- `merge` (unit_type, from_tier, to_tier, tile)
- `hero_move` (lane)
- `skill_cast` (skill, lane, target_count)
- `wave_start` / `wave_end` (wave, hull, drachma, time)
- `boon_offer` / `boon_pick` (god, boon, alternatives)
- `boss_start` / `boss_phase` / `boss_end`
- `hull_hit` (lane, source, hull_left)
- `run_end` (result, duration, wave, boss_hp, hull, drachma, cause)
- `result_next_click`
- `ad_request` / `ad_started` / `ad_completed` / `ad_error` (placement)
- `upgrade_purchase` (upgrade, level, cost)
- `codex_unlock`
- `settings_change`

Do not log personal data. Do not log full user agent strings if not needed.

## 4. Funnels

Funnel A: app_open -> home_play_click -> tutorial_step_1 -> tutorial_step_2 -> first_summon -> first_merge -> first_win -> result_next_click -> second_run_start.

Funnel B: run_start -> wave_5 -> boss_start -> boss_end_win -> result_next_click.

Funnel C: result -> rewarded_ad_offer -> ad_started -> ad_completed -> next_run.

## 5. Diagnostic questions

- If tutorial completion is low, is the first merge prompt unclear or is the button too small?
- If first-run completion is low, is the loss cause visible? Check `run_end.cause` and hull hits by lane.
- If second-run start is low, is the result screen too slow, the next button unclear, or the first island too punishing?
- If boss start is high but boss win is low, is the storm timer too tight or the eye window unclear?
- If mobile session is much lower than desktop, check orientation, touch target size, and frame rate.

## 6. A/B tests

Test one variable at a time, minimum 500 runs per arm.

1. Tutorial length: 15s vs 25s.
2. First summon: free 3 units vs free 2 units + 10 drachma.
3. Boss timing: 2:20 vs 2:35.
4. Result screen: one-click "Next" vs map return.
5. Cover art: Odysseus portrait vs Cyclops/ship composition.

Do not test monetization before retention is stable.

## 7. Dashboard

Daily: conversion, first playable, tutorial completion, first-run completion, second-run rate, session length, crash rate.
Weekly: island completion, boss win rate, unit pick rate, merge tier distribution, boon pick rate, ad opt-in, D1.
Cohort: first-time vs returning, desktop vs mobile, landscape vs portrait.

## 8. Instrumentation QA

Every event must be visible in the debug console in development. In staging, verify the event fires once and only once. Use a typed event map; a typo is a build error. Never send an event with undefined fields.

## 9. Release-target dimensions

Every event includes `release_target`:

- `web_en` - PC Web-EN / CrazyGames.
- `mobile_en` - global iOS/Android.
- `mobile_cn` - China iOS/Android.

Also include `locale`, `platform`, `app_version`, `orientation`, and `build_channel`. Report the core funnel separately for each release target. Do not blend China mobile activation with CrazyGames conversion; the traffic sources and definitions differ.
