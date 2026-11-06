# SAVE ODYSSEUS - 三个版本

> 中文摘要。详细矩阵下方保留英文术语。

- 移动端中文版：竖屏触控，简体中文，中国区合规。
- 移动端英文版：竖屏触控，英文，全球商店。
- PC 网页英文版：横屏鼠标键盘，英文，CrazyGames。
- 三个版本共享同一套核心玩法、内容、数值、存档和埋点，不做玩法分叉。

---

# SAVE ODYSSEUS - Three Release Targets

Version: 0.1
Status: design decision for the web-first build and the two mobile builds.

## 1. Decision

SAVE ODYSSEUS ships as three release packages, not three separate games:

1. Mobile-CN: iOS and Android, Simplified Chinese, portrait.
2. Mobile-EN: iOS and Android, English, portrait.
3. PC Web-EN: desktop browser and CrazyGames, English, landscape.

All three share the same core gameplay, content, balance, save schema, and live-ops rules. They differ in UI shell, input, language, distribution, SDK, monetization, compliance, and performance budget.

If the team later merges Mobile-CN and Mobile-EN into one multilingual mobile app, the product becomes two packages. The design and QA matrix still track three target configurations.

## 2. Release matrix

| Target | Platform | Language | Orientation | Input | Distribution | Monetization | Priority |
|---|---|---|---|---|---|---|---|
| Mobile-CN | iOS and Android; Android first | Simplified Chinese | Portrait 9:16 | Touch | China App Store and Android channels | Ads plus cosmetic IAP; no pay-to-win; China compliance | P2 |
| Mobile-EN | iOS and Android | English | Portrait 9:16 | Touch | Global App Store and Google Play | Ads plus cosmetic IAP; remove-ads purchase | P1 |
| PC Web-EN | Desktop browser; CrazyGames first | English | Landscape 16:9 | Mouse and keyboard | CrazyGames iframe; static web | CrazyGames ads only | P0 |

Recommended release order: PC Web-EN first, Mobile-EN second, Mobile-CN third. PC Web-EN is the fastest way to validate the loop and the CrazyGames metrics. Mobile-EN reuses the core with a portrait UI and app-store packaging. Mobile-CN adds language, payment, privacy, and game-approval work.

## 3. Shared core

The shared core contains:

- Battle simulation, grid, merge, waves, boss, boons, economy.
- Content data: islands, crew, enemies, gods, UI strings.
- Balance data and save schema.
- Analytics event schema.
- Ghost recording and replay format.
- Progression and reward rules.

The platform layer contains:

- Input mapping: mouse/keyboard vs touch.
- UI shell: landscape desktop vs portrait mobile.
- SDK and lifecycle: CrazyGames, iOS, Android, China channels.
- Monetization adapters: CrazyGames ads, global mobile ads/IAP, China mobile ads/IAP.
- Localization bundles: en and zh-CN.
- Safe area, status bar, app pause/resume, haptics, and store review prompts.

No gameplay fork. If a platform needs a different value, use a config flag, not a copy of the combat code.

## 4. Mobile assumption

Default assumption: mobile means a packaged iOS/Android app built with Capacitor around the same web core. If mobile means mobile web or PWA only, the same matrix applies but app-store, IAP, and China game-approval items are removed.

Mobile v1 is portrait only. One-thumb play is preferred: drag units on the 5x5 grid, tap a lane to move Odysseus, tap three skill buttons. Minimum touch target is 48px. Do not require hover, right-click, or keyboard.

PC Web-EN is landscape only. Mouse drag, click, and keyboard 1/2/3 for skills. The game must fit the CrazyGames iframe sizes and never show a scrollbar.

## 5. Localization

- Mobile-CN uses `zh-CN`; Mobile-EN and PC Web-EN use `en`.
- All strings live in `locales/en.json` and `locales/zh-CN.json`. No hard-coded UI text.
- Chinese uses a subset of Source Han Sans SC or Noto Sans SC. Keep the font payload under 3MB on mobile and do not load it on PC Web-EN.
- English UI must be tested at 130% string expansion. Chinese UI must be tested with the longest boss and boon names.
- PC Web-EN locks English at launch. A language selector is not required.
- Mobile-CN uses localized store text, privacy policy, and customer support. Do not machine-translate legal or store text without review.

## 6. Build targets and budgets

Use one repository with three build commands:

- `pnpm build:web-en`
- `pnpm build:mobile-en`
- `pnpm build:mobile-cn`

Build flags: `RELEASE_TARGET`, `LOCALE`, `PLATFORM`, `MONETIZATION`, `SDK`.

| Target | First build | Total assets | First playable | FPS target | Notes |
|---|---:|---:|---:|---:|---|
| PC Web-EN | 15-18MB | under 40MB | under 10s | 60fps on 4GB Chromebook | Lazy-load islands; CrazyGames SDK |
| Mobile-EN | 40-60MB install | under 100MB | under 15s on 4G | 60fps target, 30fps floor | Bundled core assets; Capacitor |
| Mobile-CN | 40-60MB install | under 100MB | under 15s on 4G | 60fps target, 30fps floor | Android first; channel SDKs |

Use the same asset IDs. Mobile may prebundle the first three islands; PC Web loads them on demand. Do not ship a separate art set per language.

## 7. Monetization by target

- PC Web-EN: CrazyGames ads only. Midgame ad on result/next. Rewarded ad for double drachma. No IAP.
- Mobile-EN: ads plus cosmetic IAP. Remove-ads purchase. Rewarded ad optional. No pay-to-win, no loot boxes, no energy.
- Mobile-CN: ads plus cosmetic IAP through compliant China channels. Real-name and minor-protection rules apply. No gambling-style random paid boxes.

Cosmetics are shared: sails, figureheads, Odysseus armor, ship trails. Stats never change.

## 8. Compliance and platform notes

PC Web-EN: CrazyGames rules, PEGI12, English, no external links, no custom fullscreen, no ads during battle.

Mobile-EN: App Store and Google Play privacy labels, ATT where required, GDPR/CCPA, age rating, restore purchases, account deletion path if accounts are added.

Mobile-CN: game approval number, real-name verification, anti-addiction, privacy policy, data handling, payment compliance, ad review, and Android channel requirements. China commercial release is blocked until these are confirmed. Do not treat the English mobile build as a compliance shortcut.

## 9. QA matrix

PC Web-EN: Chrome, Edge, Firefox, Safari 16+; iframe sizes 907x510, 1216x684, 1920x1080; 4GB Chromebook; mouse and keyboard.

Mobile-EN: iPhone 12/13/15, Pixel 6/7, Samsung mid-tier, low-end Android; iOS 16+, Android 10+; safe areas; app background and resume; 4G; touch only.

Mobile-CN: Huawei, Xiaomi, OPPO, vivo, Honor; Android 12+; no Google services; China store packages; real-name and anti-addiction flows; payment sandbox.

## 10. Release sequence

- Weeks 1-3: shared core and PC Web-EN vertical slice.
- Weeks 4-8: PC Web-EN Basic Launch.
- Weeks 9-12: PC Web-EN Full Launch and Mobile-EN feature complete.
- Weeks 13-16: Mobile-EN store submission and soft launch.
- Weeks 13-20: Mobile-CN compliance, Android channels, then iOS.
- Weeks 21-24: content parity pass across all three targets.

Dates are indicative. China compliance is the long pole; start it before the mobile build is finished.

## 11. Open decisions

1. Is mobile a native app or mobile web/PWA? Default: native wrapper.
2. Does Mobile-CN ship Android first, or Android and iOS together?
3. Is Mobile-EN one global app with English only, or a multilingual app with English default?
4. Do we need cross-platform cloud save at launch? Default: no.
5. Do we need cross-platform leaderboards at launch? Default: no.
