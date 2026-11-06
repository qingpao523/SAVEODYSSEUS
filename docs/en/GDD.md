# SAVE ODYSSEUS - Game Design Document

Version: 0.1 (web-first pre-production)
Owner: Game Design
Platform: CrazyGames web, desktop and mobile browser
Status: design baseline for the two-week vertical slice

## 1. Vision

SAVE ODYSSEUS is a three-to-four minute merge and lane-defense game. The player is Odysseus, the last captain between the crew and the sea. Merge survivors, place them on the ship deck, call on the gods, and bring the ship home to Ithaca.

The emotional promise is not "kill monsters". It is "the sea takes everything; you bring them home". Every run is one leg of the voyage. Ithaca is always in the distance.

## 2. Pillars

1. Three-second clarity. A ship, three lanes, a hero, and monsters must be readable at a glance.
2. Merge decisions with spatial cost. Merging makes a unit stronger but frees or consumes grid space; placement matters.
3. A hero you can feel. Odysseus moves between lanes and has three readable skills, not a stat block.
4. A climax every run. The storm timer turns red near the end; the boss must die before the sea wins.
5. A fair web game. No energy, no login wall, no pay-to-win, no ads during battle.

## 3. Audience and platform

Primary audience: players who enjoy tower defense, merge, Greek myth, and short browser sessions. Secondary audience: players who like light roguelite boons and score chasing.

Platform: CrazyGames web first. Desktop and mobile browser. Landscape is the primary layout; portrait is a supported reflow. The game must work inside an iframe at 907x510, 1216x684, 821x462, 1366x768, 1920x1080, 800x450, and 1080x607. English UI at launch. PEGI12.

## 4. Macro loop

Home -> Route Map -> Prep (patron, crew, ship) -> Battle -> Result -> Route Map -> next island.

A full voyage has nine islands. Each island is one run. The player unlocks the next island by winning, earns drachma and crew shards, and spends them on capped ship upgrades and codex unlocks. Failure is diegetic: the Fates reweave the route, and the player tries again with permanent progress intact.

## 5. Micro loop

1. Summon three crew units with drachma.
2. Units auto-place on empty tiles; the player can drag to rearrange.
3. Merge two identical units of the same tier into the next tier.
4. Monsters enter from the sea in three lanes and walk toward the ship.
5. Crew auto-attack. Odysseus auto-attacks and the player triggers three skills.
6. Kills grant drachma and charge the ultimate.
7. After waves 2 and 4, choose one of three god boons.
8. At wave 6, defeat the boss before the storm destroys the ship.
9. Win or lose, collect rewards and return to the map.

## 6. Match structure

Match length target: 3:00-3:40.

| Time | Event |
|---|---|
| 0:00-0:15 | In-game tutorial: summon, merge, move hero, use spear |
| 0:15-1:55 | Waves 1-5, one wave every 25 seconds, 5 seconds of preparation between waves |
| 1:55-2:20 | Boss warning, final boon, storm timer enters warning state |
| 2:20-3:00 | Boss fight; storm turns red at 2:30 |
| 3:00-3:40 | If boss is alive, storm deals hull damage every 6 seconds; run ends when hull reaches zero or boss dies |

The storm timer is 180 seconds. It starts at 3:00. At 2:30 it turns red: enemy speed and damage increase by 20%. At 3:00 the storm deals one hull damage every 6 seconds. This gives the player a final comeback window instead of an instant loss.

## 7. Battlefield

The battlefield is a 5x5 grid between the sea and the ship. Enemies enter from the top in three lanes: left, center, right. The ship sits at the bottom with three hull sections. Buildable tiles cover the 5x5 grid. Units can be placed on any empty tile; blockers are most useful on lane tiles.

The grid is intentionally small. A 5x5 board is readable on mobile, creates real space pressure, and keeps merge decisions meaningful. There is no grid expansion in the MVP. Expansion can be tested later as a "beachhead" mechanic.

## 8. Crew units

All crew units are data-driven. Base stats are tier 1. Merging two identical units of the same type and tier creates tier 2, then tier 3, then tier 4. A merged unit occupies one tile, so merging frees space but reduces coverage.

| Unit | Role | Targeting | Notes |
|---|---|---|---|
| Hoplite | Melee blocker | Ground, same lane | High HP, low range, stops enemies |
| Archer | Ranged damage | Air and ground, 3 tiles | Low HP, single target |
| Sailor | Support | Allies and ship | Repairs ship once per wave and gives a small attack-speed aura |
| Shield Wall | Blocker | Ground, adjacent | High HP, low damage, slows enemies |
| Ballista | Area damage | Ground, 4 tiles | Slow attack, splash damage, good against groups |

Merge tiers use color borders and pips, not text. Tier 1 is bronze, tier 2 silver, tier 3 gold, tier 4 divine blue. A tier 4 unit has a unique silhouette accent.

## 9. Odysseus

Odysseus stands at the helm. He can move between left, center, and right lanes on a 4-second cooldown. He auto-attacks the nearest enemy in his lane. He cannot be targeted or killed; the ship is the fail state.

| Skill | Input | Effect | Cooldown |
|---|---|---|---|
| Spear Throw | 1 or button | Line damage and knockback in the current lane | 8s |
| Aegis | 2 or button | Shield the current lane and ship for 3s | 18s |
| Athena's Light | 3 or button | Ultimate beam; consumes full charge; heavy lane and boss damage | charge 100 |

The ultimate charges from kills. Common enemies grant 4 charge, elites 10, boss phases 20. The player should get one ultimate in waves 3-4 and one during the boss.

## 10. Ship and storm

The ship has three hull sections. Any enemy that reaches the bottom of a lane removes one section. At zero, the run ends. A Sailor can repair one section once per wave if the ship survived the wave. Some boons can restore hull.

The storm timer creates the climax. It is visible at the top of the screen as a bronze ring. At 2:30 it turns red and pulses. At 3:00 it starts dealing hull damage. The timer is not a hard fail; it is a pressure clock.

## 11. Economy

Currency: drachma. Sources: kills, wave clear, boss, first-clear bonus. Sinks: summon, reroll, ship repair, meta upgrades.

Summoning costs 10 drachma for the first call, then increases by 2 each call: 10, 12, 14, 16, and so on. Each call summons three units. The first tutorial call is free and guarantees two Hoplites and one Archer so the player can merge immediately.

Kill bounty: common 1, elite 3, boss 50. Wave clear: 10. First clear of an island: 60. A typical run earns 50-80 drachma before meta bonuses.

## 12. God boons

After waves 2 and 4, the player chooses one of three boons. Boons are run-only. The pool is weighted by the chosen patron.

| God | Boon examples | Fantasy |
|---|---|---|
| Athena | Aegis duration +2s; reveal boss weak point; +1 ultimate charge per kill | Wisdom and protection |
| Poseidon | Wave pushes enemies back; storm damage reduced; risky extra drachma | Sea and risk |
| Hermes | Hero move cooldown -40%; summon cost -2; extra reroll | Speed and economy |
| Hephaestus | Ballista splash +50%; ship repairs one section; tier 3 upgrade discount | Craft and fire |
| Apollo | Archer range +1; heal one hull section; +20% attack speed for 10s after a wave | Light and recovery |
| Circe | Transform one enemy per wave; charm duration +50%; risky boon | Witchcraft and control |

Each god has at least six boons in the full version. The MVP uses six total boons, one per god.

## 13. Boss

The MVP boss is the Cyclops. It walks slowly down the center lane, throws rocks at a telegraphed lane, and slams the deck when it reaches the ship. The eye is the weak point. The player must use Aegis on the rock lane and save Athena's Light for the eye.

Boss phases:
1. 100%-70% HP: rock throw every 8 seconds.
2. 70%-40% HP: adds two waves of small enemies and a wider slam.
3. 40%-0% HP: enraged; rock throw every 5 seconds; eye opens for 3 seconds after each slam.

Future bosses: Sirens (charm), Scylla (multi-lane heads), Laestrygonians (horde), Poseidon storm (environmental).

## 14. Win and loss

Win: boss defeated. The ship may be damaged; the run still counts. Bonus stars for no hull damage and for finishing before the storm red state.

Loss: hull reaches zero. The result screen shows the wave reached, boss HP remaining, and one suggested improvement. No shaming, no forced ad.

## 15. Modes

Voyage: solo, default. One island per run, nine islands in the full version. The MVP has one island.

Trial of the Sea: asynchronous 1v1. Two ships, the same wave seed. First ship destroyed loses. If both survive, compare hull sections, then remaining storm time. The opponent is a recorded timeline; no real-time server needed. Unlock after two solo runs.

Daily Sea: one fixed seed per day, global leaderboard. Post-launch.

## 16. Meta progression

The meta exists to create a reason to return, not to sell power.

- Route Map: nine islands, three stars each. Stars unlock cosmetics and codex entries.
- Crew Codex: twelve named crew. Each has one small passive. Unlock with crew shards from runs; no paid random boxes.
- God Favor: six patrons. Devotion unlocks boons and one patron ability. No stat pay-to-win.
- Ship Upgrades: hull, sails, ballista, repair. Five levels each, capped. Costs 100-1400 drachma.
- Cosmetics: sails, figureheads, Odysseus armor. Earned through stars and achievements.
- Daily quests: three light tasks. No energy system.
- Weekly leaderboard: farthest island, fastest boss, highest hull.

## 17. UX flow

Screens: Boot -> Home -> Route Map -> Prep -> Battle -> Result -> Home.

Home: big Play button, current island, daily quest, codex, ship, settings. One click to the map; the map's current node has one clear Play button. First-time users go directly into the tutorial run after one Play click.

Prep: choose patron, one crew passive, and inspect ship upgrades. Keep this screen under 15 seconds. A "Quick Start" button remembers the last loadout.

Battle HUD: hull sections top left, storm timer top center, wave top right, drachma bottom left, summon bottom center, three skills bottom right, pause and sound top right. No banner ads during battle.

Result: victory or defeat, stars, drachma, crew shards, one suggested improvement, next button. Ads only when the player taps Next.

## 18. Tutorial

The tutorial is diegetic and skippable. It uses three prompts and no text walls:

1. "Call three crew." The summon button pulses; the first call is free.
2. "Drag two Hoplites together." The two Hoplites glow; merging triggers a burst.
3. "Move Odysseus and throw the spear." The lane highlights; the spear kills the first enemy.

The tutorial ends at the first wave clear. The player can skip at any time from the pause menu. The tutorial should take 15-25 seconds.

## 19. Content scope

MVP: one island (Cyclops cave), one hero, five crew units, six waves, one boss, six boons, twelve enemy variants, one solo mode, one async duel, one ship, three hull sections, one storm timer.

Full v1: nine islands, five bosses, twelve crew, six gods, thirty-six boons, twelve enemy families, three ship skins, daily sea, weekly leaderboard.

Post-launch: new islands, challenge modifiers, cosmetic seasons, true PvP only if retention justifies the server cost.

## 20. Balance principles

- The first run should be winnable by a new player in 60-70% of attempts after the tutorial.
- Every loss should have a visible cause: a leaked lane, a bad merge, an unspent ultimate, or a missed Aegis.
- Merging should be tempting but not always correct. Wide low-tier coverage is good for swarms; tall high-tier units are good for elites.
- The storm timer should create urgency, not panic. The player should understand the red state.
- Boons should change decisions, not just add percentages. At least half of the boons should add a new behavior.

## 21. Accessibility and tone

- Color is never the only signal; use icons, pips, and shapes.
- All buttons are at least 44px on touch.
- Text is English at launch; all strings are externalized.
- Reduced motion setting lowers screen shake and particle density.
- Audio can be muted through the CrazyGames SDK; the in-game mute respects it.
- Tone: heroic, wry, and humane. No gore, no sexual content, no gambling aesthetics. PEGI12.

## 22. Live operations

Season 1: nine islands, daily sea, weekly leaderboard, one cosmetic set.
Season 2: new island, new boss, two crew, one god.
Season 3: challenge modifiers and a cosmetic battle pass (no stat rewards).

No energy, no login wall, no pay-to-win. Ads are end-of-run only. Cosmetic purchases only if CrazyGames invites the game to in-game purchases.

## 23. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Odyssey title is crowded | Trademark search; prepare NOSTOS and Aegean Oath as backups |
| Greek skin feels generic | Black-figure pottery art direction and ship-defense structure |
| Mobile UI too busy | One screen, 5x5 grid, three skills, no inventory |
| Scope creep | MVP one island, one boss, five units |
| Ads hurt retention | End-of-run only, rewarded optional, measure second-run rate |
| True PvP cost | Async ghost only at launch |
| Modern adaptation overlap | Use Homer and Greek myth only; original art, music, and text |

## 24. Acceptance criteria for the vertical slice

- One complete run from Home to Result to Home.
- Summon, merge, move hero, three skills, two boons, boss, win and loss states.
- 5x5 grid with three lanes and three hull sections.
- Storm timer with red state and hull damage.
- English UI and 15-second tutorial.
- 15-18MB first build, first playable under 10 seconds, 60fps on a 4GB Chromebook.
- Ten external playtesters can explain the goal and the merge rule after one run.

## 25. Three release targets

The design supports three release packages, not three gameplay forks:

- **Mobile-CN**: iOS/Android, Simplified Chinese, portrait, touch, China distribution, compliance, ads + cosmetic IAP.
- **Mobile-EN**: iOS/Android, English, portrait, touch, global stores, ads + cosmetic IAP.
- **PC Web-EN**: desktop browser and CrazyGames, English, landscape, mouse + keyboard, CrazyGames ads only.

The shared core contains battle simulation, content data, balance, save schema, ghost format, and analytics. The platform layer contains UI shell, input, language, SDK, lifecycle, monetization, compliance, and performance budget. No combat fork.

Mobile is portrait-only for v1. PC Web is landscape-only. See `docs/PLATFORM_VERSIONS.md`.
