# SAVE ODYSSEUS - 平衡与经济

版本：0.1
目的：垂直切片的初始调整目标，而不是最终实时值。

## 1. 设计目标

- 第一个教程运行：75-85% 胜率。
- 第 1 岛首次尝试：55-65%。
- 第 3 岛首次尝试：40-50%。
- 第 6 岛首次尝试：30-40%。
- 第 9 岛首次尝试：20-30%。
- 平均运行时长：3:00-3:40。
- 平均训练时间：10-15 分钟，3-4 次对局。
- 二轮启动率：55%+。

损失应该可以用一句话来解释。如果玩家不能说出为什么他们输了，那么平衡或反馈就是错误的。

> Sections 2-11 use English terms and tables to avoid translation ambiguity. The Chinese design targets are above.

## 2. Match clock

| Time | Event |
|---|---|
| 0:00 | Run starts; tutorial prompts |
| 0:15 | Wave 1 |
| 0:40 | Wave 2; boon choice after clear |
| 1:05 | Wave 3 |
| 1:30 | Wave 4; boon choice after clear |
| 1:55 | Wave 5; boss warning |
| 2:20 | Boss spawns |
| 2:30 | Storm turns red: enemy speed +20%, damage +20% |
| 3:00 | Storm hull damage: 1 section every 6s |
| 3:00-3:40 | Final boss window; run ends at hull 0 or boss death |

## 3. Crew base stats

Tier 1 values. Tier 2 = 2.2x HP and DPS. Tier 3 = 4.8x. Tier 4 = 10x. Merging consumes both units and occupies one tile.

| Unit | HP | DPS | Range | Attack interval | Cost weight | Notes |
|---|---:|---:|---:|---:|---:|---|
| Hoplite | 120 | 12 | 1 tile | 1.0s | 1.0 | Blocks one ground enemy |
| Archer | 70 | 18 | 3 tiles | 1.1s | 1.1 | Targets air and ground |
| Sailor | 80 | 6 | 2 tiles | 1.4s | 1.0 | Repairs 1 hull once per wave; +10% attack speed aura |
| Shield Wall | 200 | 4 | 1 tile | 1.6s | 1.0 | Slows contact enemies 15% |
| Ballista | 90 | 25 | 4 tiles | 2.2s | 1.3 | Splash radius 1 tile |

Summon pool weights at start: Hoplite 30%, Archer 30%, Sailor 15%, Shield Wall 15%, Ballista 10%. After wave 3, Ballista weight rises to 15% and Hoplite falls to 25%.

## 4. Enemy base stats

| Enemy | HP | Speed | Damage | Bounty | Notes |
|---|---:|---:|---:|---:|---|
| Raider | 60 | 28 | 10 | 1 | Basic |
| Brute | 180 | 18 | 25 | 3 | Breaks shields |
| Harpy | 70 | 40 | 8 | 1 | Flying; ignores blockers |
| Siren | 90 | 22 | 0 | 2 | Charms a crew unit for 3s |
| Rock Thrower | 110 | 20 | 15 | 2 | Ranged; stops at 3 tiles |
| Elite Raider | 320 | 24 | 20 | 5 | Shielded; 20% damage reduction |
| Cyclops | 6000 | 12 | 40 | 50 | Boss; armor 20% |

Wave HP multiplier: wave 1 = 1.0, wave 2 = 1.15, wave 3 = 1.35, wave 4 = 1.6, wave 5 = 1.9, boss = 1.0 on base 6000. Island difficulty adds +15% HP and +8% speed per island after the first.

## 5. Wave table (MVP)

| Wave | Composition | Spawn interval | Clear reward |
|---|---|---|---:|
| 1 | 6 Raiders | 1.6s | 10 |
| 2 | 8 Raiders, 2 Brutes | 1.4s | 10 |
| 3 | 8 Raiders, 2 Harpies, 1 Siren | 1.2s | 10 |
| 4 | 10 Raiders, 3 Brutes, 2 Harpies | 1.1s | 10 |
| 5 | 10 Raiders, 2 Brutes, 2 Harpies, 1 Siren, 1 Elite | 1.0s | 15 |
| Boss | Cyclops + 2 add waves | scripted | 50 |

Total enemies in a normal run: about 50-60. Target active enemies: under 25.

## 6. Economy

Starting drachma: 30. First tutorial summon is free.

Summon cost: 10, then +2 per call: 10, 12, 14, 16, 18, 20. Each call gives three units. The player can afford about six to eight calls in a good run.

Income: kills 1-5, wave clear 10-15, boss 50, first clear 60. A normal win earns 70-110 drachma before multipliers. A loss earns 30-60.

Repair cost during a run: 15 drachma for one hull section, maximum twice. This is a comeback lever, not a mandatory sink.

Meta upgrade costs:

| Upgrade | L1 | L2 | L3 | L4 | L5 |
|---|---:|---:|---:|---:|---:|
| Hull | 100 | 250 | 500 | 900 | 1400 |
| Sails | 100 | 250 | 500 | 900 | 1400 |
| Ballista | 120 | 300 | 600 | 1000 | 1500 |
| Repair | 80 | 200 | 450 | 800 | 1200 |

Upgrades are capped and never sold for real money. A new player should unlock the first upgrade after 2-3 runs.

## 7. Odysseus skills

| Skill | Damage/effect | Cooldown | Notes |
|---|---|---|---|
| Auto-attack | 25 DPS | - | Targets nearest enemy in lane |
| Spear Throw | 120 line damage + 1 tile knockback | 8s | Good for clumps |
| Aegis | 3s lane/ship shield | 18s | Blocks one rock or one leak |
| Athena's Light | 500 lane damage, 1500 boss damage | 100 charge | One use in waves 3-4, one in boss |

Ultimate charge: common kill +4, elite +10, boss phase +20. The player should reach 100 around wave 3 if they are clearing normally.

## 8. Boon budget

A boon should be worth roughly 10-20% of a run's power, or change one decision. Avoid stacking multipliers that break the storm timer.

Examples:

- Athena: Aegis +2s, reflect 20% blocked damage.
- Poseidon: push enemies back one tile; storm damage +10% as a risk.
- Hermes: hero move cooldown -40%; +1 reroll.
- Hephaestus: ballista splash +50%; repair one hull.
- Apollo: archer range +1; +20% attack speed for 10s after a wave.
- Circe: transform one non-boss enemy per wave for 6s.

Boon offer rules: three distinct gods, no duplicate boon in one run, at least one offensive and one defensive option when possible. Patron weighting: 40% patron, 60% others.

## 9. Boss tuning

Cyclops HP 6000. Expected player DPS at boss: 180-260. Fight length target: 35-50 seconds. If the fight is over 60 seconds, the storm will likely win; if under 25 seconds, the boss is too weak.

Rock throw: 1 hull damage unless blocked. Aegis can block one rock. Shield Wall can absorb one rock and dies. Slam: 2 hull damage if the player ignores the telegraph.

Phase thresholds: 70% and 40%. Add waves at each threshold. Eye window: 3 seconds, double damage, armor removed. Show a clear gold eye icon.

## 10. Difficulty scaling

Island multiplier: HP +15%, speed +8%, bounty +10% per island after the first. Boss HP +25% per island after the first. Do not increase enemy count more than 15% per island; increase composition difficulty instead.

Catch-up mechanics: if the player has lost two runs in a row on an island, offer a free +1 hull blessing at prep. If the player wins with three stars, offer a harder modifier for extra rewards.

## 11. Tuning process

1. Build a spreadsheet model for wave DPS versus enemy HP.
2. Run 20 automated simulations per configuration with fixed seeds.
3. Playtest with 10 external players; record first loss cause.
4. Adjust one variable per build: economy, enemy HP, or boss HP.
5. Freeze values for the CrazyGames Basic Launch; do not chase daily balance during the first week.

Kill criteria for a build: tutorial completion under 70%, first-run completion under 50%, or second-run start under 35%. Fix onboarding before adding content.
