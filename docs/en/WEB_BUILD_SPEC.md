# SAVE ODYSSEUS - Web Build Specification

Version: 0.1
Target: CrazyGames web, desktop and mobile browser
Engine recommendation: Phaser 3 + TypeScript + Vite, with a DOM/CSS HUD overlay

## 1. Why this stack

Phaser 3 is mature, small enough for a 15-18MB first build, works well with sprite atlases and object pools, and supports WebGL with a Canvas fallback. TypeScript keeps combat, economy, and save data typed. Vite gives fast iteration and a small production bundle. The battlefield is rendered in Phaser; HUD, menus, settings, and result screens use DOM/CSS. This hybrid keeps the game readable on mobile and makes localization and responsive layout easier.

Do not use Unity WebGL for the first build. The loader is too heavy for the CrazyGames first-playable target, and iOS memory pressure is a risk. Do not use a full React app for the battlefield; UI-only DOM is enough.

## 2. Browser and device matrix

Supported: Chrome, Edge, Firefox, Safari 16+, Samsung Internet, Android Chrome, iOS Safari.

Test sizes inside an iframe: 907x510, 1216x684, 821x462, 1366x768, 1920x1080, 800x450, 1080x607, 390x844, 844x390, 768x1024.

Target hardware: 4GB Chromebook, Intel Celeron N4020, integrated graphics, 60fps at 1280x720. Mobile mid-tier: 60fps at 720p with reduced particles; minimum acceptable 30fps.

The game must not require WebGL2. Use Phaser AUTO and a Canvas fallback. Detect device memory and reduce particle count, shadow quality, and simultaneous tweens on low-end devices.

## 3. Responsive layout

Landscape is the primary layout. The battlefield is centered, with the sea at the top and the ship at the bottom. The HUD uses safe-area insets. Portrait is a supported reflow, not a rotated canvas.

Landscape: hull top-left, storm timer top-center, wave top-right, drachma bottom-left, summon bottom-center, skills bottom-right, pause and mute top-right.

Portrait: sea and lanes fill the upper 55%, ship hull lower 15%, 5x5 grid in the middle, summon and three skills in a bottom action bar. The rival ghost panel is hidden in portrait during solo; in async duel it becomes a compact top strip.

Use CSS grid for HUD; use Phaser Scale.RESIZE and recompute world positions on resize. Never use fixed pixel coordinates for UI. Keep touch targets at least 44px. Avoid hover-only interactions.

## 4. Performance budgets

- First build: 15-18MB compressed transfer.
- Total package: under 40MB for v1; CrazyGames total limit 250MB.
- Files: under 1500.
- First playable: under 10 seconds on a normal connection.
- Time to interactive: under 12 seconds.
- 60fps target on 4GB Chromebook; 30fps floor on low-end mobile.
- Draw calls: under 120 per frame.
- Active sprites: under 250; particles: under 180 on desktop, 80 on mobile.
- Audio: stream music, preload only the first island's music and SFX.
- Texture atlas: one atlas per island, plus a shared UI atlas.
- No runtime 3D models. No large video files. No web fonts above 120KB total.

## 5. Asset pipeline

Source art in PNG or SVG; export to texture atlases with TexturePacker or free-tex-packer. Use WebP for large backgrounds with PNG fallback; use OGG for music and SFX with M4A fallback for Safari. Keep audio mono for SFX, stereo for music. Normalize loudness to -16 LUFS for music and -12 dBFS peak for SFX.

Naming: `bg_<island>`, `unit_<type>_t<tier>`, `enemy_<type>_<anim>`, `boss_<name>_<anim>`, `ui_<component>_<state>`, `sfx_<event>`, `mus_<layer>`.

Lazy-load island assets during the route map; preload the first island during the home screen. Never block the first paint on all islands.

## 6. Architecture

Use six scenes: Boot, Preload, Home, Map, Battle, Result. Keep the Battle scene deterministic and data-driven.

Core modules:

- `GameState`: run state, meta state, settings, save version.
- `GridSystem`: 5x5 tiles, occupancy, placement, merge resolution.
- `WaveSystem`: wave definitions, spawn queue, elite modifiers, boss phase triggers.
- `CombatSystem`: targeting, damage, armor, status effects, projectiles.
- `HeroSystem`: lane position, auto-attack, three skills, cooldowns, ultimate charge.
- `EconomySystem`: drachma, summon cost, wave rewards, repair costs.
- `BoonSystem`: pool, weighting, three-choice offer, run modifiers.
- `GhostSystem`: record and replay asynchronous runs.
- `SaveService`: CrazyGames Data adapter plus local fallback.
- `AnalyticsService`: typed event queue with SDK forwarding.
- `AudioService`: music layers, SFX pool, mute state, SDK sync.
- `LocalizationService`: JSON dictionaries, locale from SDK, English fallback.

Use a simple event bus for UI-to-game messages. Do not let DOM UI mutate combat state directly; send typed commands such as `SUMMON`, `MERGE`, `MOVE_HERO`, `CAST_SKILL`, `CHOOSE_BOON`.

## 7. Data-driven content

All units, enemies, waves, boons, bosses, islands, and rewards live in versioned JSON or TypeScript data files. No balance number is hard-coded in a scene.

Each content record has an `id`, `displayNameKey`, `descriptionKey`, `tier`, `stats`, `tags`, and `assetKey`. Validate all IDs at build time. Fail the build on missing localization keys or missing assets.

Use a seeded pseudo-random generator for wave composition and boon offers. Store the seed in the run record so a run can be reproduced for debugging and ghost replay.

## 8. Determinism and ghost replay

The ghost is a recorded timeline, not a re-simulated AI opponent. Record at 10Hz: timestamp, hull, wave, boss HP, unit placements, merges, hero lane, skill casts, and boon choices. Replay by interpolating between snapshots and triggering the same visual events. This avoids cross-device physics differences.

A ghost record is capped at 120KB. Store the player's best ghost locally and upload only if a backend is added later. For launch, use curated bot ghosts and local player ghosts; no server required.

## 9. Save data

CrazyGames Data module is the primary save. LocalStorage is a fallback. Save after every run, upgrade, codex unlock, and settings change; debounce writes to once per 2 seconds.

Save schema:

- `version`: integer.
- `meta`: drachma, unlockedIslands, stars, crewShards, crewUnlocks, godDevotion, shipUpgrades, cosmetics, achievements.
- `settings`: language, music, sfx, reducedMotion, colorblindMode.
- `records`: bestRun per island, bestGhost, totalRuns, totalWins.
- `flags`: tutorialComplete, firstWin, duelUnlocked.

Migrate saves by version. If migration fails, keep a backup in localStorage and start a safe default rather than crashing. Never store personal data.

## 10. CrazyGames SDK integration

Initialize the SDK before the first scene. If the SDK is unavailable, run in local fallback mode and disable ads and cloud save.

Required calls:

- `SDK.init()`
- `SDK.game.loadingStart()` and `SDK.game.loadingStop()`
- `SDK.game.gameplayStart()` at the first player input in a run
- `SDK.game.gameplayStop()` on pause, result, tab hidden, or run end
- `SDK.data` for save/load
- `SDK.ad.requestAd('midgame')` only on result/next
- `SDK.ad.requestAd('rewarded')` only after explicit user action
- `SDK.game.happytime()` on boss kill and first clear
- `SDK.game.muteAudio()` / unmute
- `SDK.user` optional; never require login

Do not call ads during battle. Do not auto-play a rewarded ad. Do not use custom fullscreen. Do not link to external stores or websites. Respect iframe focus and audio permissions.

## 11. Localization

English is the launch language. All strings use keys; no hard-coded English in scenes. Use ICU-lite for pluralization. Keep UI strings short; the German/Russian/Portuguese versions may expand by 30%.

CrazyGames locale is a hint, not a guarantee. Fall back to English. Do not machine-translate the launch build without review. The first additional languages should be Portuguese (Brazil), Spanish (LatAm), German, French, and Hindi/Bengali only after data shows demand.

## 12. Audio and focus

Browsers block autoplay. Start music only after the first user gesture. Mute when the tab is hidden. Respect CrazyGames mute state. Duck music during boss intro and result. Use a single master gain node; no per-frame volume changes.

## 13. Error handling

- If the SDK fails: local mode, no ads, no cloud save, show no error to the player.
- If an asset fails: retry once, then show a compact "Reload" panel.
- If save fails: keep in-memory state, retry on next checkpoint, never wipe progress.
- If WebGL context is lost: pause the run, restore, and resume from the last snapshot; if restore fails, offer restart.
- If frame rate drops below 30fps for 5 seconds: reduce particles, disable screen shake, and lower target resolution by 15%.

## 14. QA checklist

- Load inside a 907x510 iframe with no scrollbars.
- First playable under 10 seconds on a cold cache.
- All buttons work with touch only; no hover requirement.
- Portrait and landscape do not clip the HUD.
- Pause stops gameplay and calls `gameplayStop`.
- Ads only on result/next; rewarded ad only after a click.
- Save survives reload and a second tab.
- Mute persists and respects SDK.
- No console errors; no unhandled promise rejections.
- 60fps on a 4GB Chromebook; 30fps on a mid-tier phone.
- Crash-free session above 99.5% in a 30-minute soak.

## 15. Build and deployment

Use Vite production build. Serve from a static host with Brotli compression. Cache hashed assets for one year; HTML no-cache. Use a staging URL for CrazyGames review. Keep a `build-info.json` with version, commit, and asset hash.

CI: install, typecheck, lint, unit tests for combat/economy, build, bundle-size check, Playwright smoke test at 907x510 and 390x844. Reject builds above 18MB or with missing assets.

## 16. Vertical-slice acceptance

- One complete run, Home to Result to Home.
- Summon, merge, hero move, three skills, two boons, Cyclops boss, win and loss.
- 5x5 grid, three lanes, three hull sections, storm red state.
- English UI, 15-second tutorial, save/load.
- SDK init, loading, gameplayStart/Stop, mute, one midgame ad on result.
- 15-18MB build; under 10 seconds first playable; 60fps on Chromebook.
