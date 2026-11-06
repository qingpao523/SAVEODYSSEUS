# SAVE ODYSSEUS - Production Plan

Version: 0.1
Target: playable CrazyGames vertical slice in 3 weeks; Basic Launch in 8 weeks; Full Launch in 12 weeks.

## 1. Team

Minimum team: 1 game designer, 1 gameplay engineer, 1 UI/UX designer, 1 2D artist/animator, 1 audio contractor, 1 QA/generalist. A producer is optional at this scope. The designer owns the GDD, balance sheet, and playtest notes. The engineer owns the build, SDK, and performance.

## 2. Scope rule

The MVP is one island, one boss, five crew units, six boons, one hero, six waves, one solo mode, one async duel, one ship, three hull sections, and one storm timer. Anything else is a post-launch idea.

If a feature does not make the first 90 seconds clearer or the boss climax better, cut it.

## 3. Milestones

### M0 - Design lock (week 1)

Deliverables: GDD v1.0, balance sheet v0.1, art style test, title/trademark search, SDK test page.

Exit: the team can explain the loop in one minute; no open question blocks engineering.

### M1 - Core prototype (week 1-2)

Deliverables: 5x5 grid, summon, merge, three lanes, one hero, spear, one enemy, one wave, hull, storm timer, win/loss placeholder.

Exit: 10 internal plays; the merge and lane movement are readable without explanation.

### M2 - Vertical slice (week 3)

Deliverables: one complete run, Cyclops boss, five units, three skills, two boons, tutorial, result screen, save/load, SDK init.

Exit: 10 external players; 70% can explain the goal and merge rule; first playable under 10 seconds; 60fps on Chromebook.

### M3 - Content and platform (week 4-5)

Deliverables: six waves, six boons, six enemy types, async ghost duel, English UI, settings, mute, analytics, covers, 15-second trailer, CrazyGames build.

Exit: QA checklist passes; build 15-18MB; no console errors.

### M4 - Basic Launch (week 6-8)

Deliverables: CrazyGames submission, launch build, dashboard, hotfix plan.

Exit: 500+ plays; conversion 80%+; first-run completion 65%+; second-run start 50%+; crash rate under 0.5%.

### M5 - Full Launch (week 9-12)

Deliverables: three islands, three bosses, twelve crew, twelve boons, daily sea, weekly leaderboard, cosmetic set.

Exit: D1 10-15%; average session 10+ minutes; positive comment sentiment; no pay-to-win complaints.

## 4. Weekly cadence

Monday: build review and metric review. Wednesday: playtest and balance pass. Friday: content freeze and QA. No new scope after Wednesday of a launch week.

## 5. Definition of done

A feature is done when it is playable, localized, instrumented, saved correctly, tested on desktop and mobile, documented, and has a fallback. A feature without analytics is not done.

## 6. Risks

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Title conflict | Medium | High | Trademark search; backup names NOSTOS, Aegean Oath |
| Scope creep | High | High | MVP one island; weekly cut review |
| Mobile UI clutter | Medium | High | 5x5 grid, three skills, no inventory |
| Art too expensive | Medium | Medium | Tier accents, reuse silhouettes, one island |
| Boss too hard | Medium | Medium | 20 simulated runs; tune HP and telegraphs |
| SDK/ad rejection | Low | High | Follow CrazyGames rules; test in iframe early |
| Low second-run rate | Medium | High | Result tip, faster restart, daily goal |

## 7. Launch checklist

- CrazyGames SDK initialized and tested in iframe.
- No external links, no custom fullscreen, no ads during battle.
- English UI; all strings externalized.
- PEGI12 content review; no gore, no sexual content, no gambling.
- 15-18MB build; cold load under 10 seconds.
- Desktop and mobile QA; portrait and landscape.
- Save migration and fallback tested.
- Analytics events firing; dashboard live.
- Support email/contact in the game page, not in the game canvas.
- Backup build and rollback plan.

## 8. Post-launch

Week 1: watch crash rate, tutorial completion, first-run completion, second-run rate. Do not add content.
Week 2: fix the top three friction points; tune one variable.
Week 3-4: add one island and one boss if retention is healthy.
Only add true PvP if async duel retention is strong and a backend budget is approved.

## 9. Three release targets

The production plan covers three packages:

1. **PC Web-EN** (P0): CrazyGames validation, weeks 1-8 Basic Launch, weeks 9-12 Full Launch.
2. **Mobile-EN** (P1): Capacitor wrapper, portrait UI, global stores, weeks 9-16.
3. **Mobile-CN** (P2): Simplified Chinese, China Android channels first, compliance, weeks 13-20.

Shared core is frozen before mobile packaging starts. Do not fork gameplay. China compliance is the long pole. See `docs/PLATFORM_VERSIONS.md`.
