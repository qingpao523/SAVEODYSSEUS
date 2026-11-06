# SAVE ODYSSEUS - Art and Audio Bible

Version: 0.1

## 1. Visual thesis

A black-figure Greek pottery diorama that comes alive. The game should look like a museum artifact painted with a story, then animated with wit and weight. Not generic marble-and-gold fantasy. Not a somber historical simulation. A bold, graphic, readable web game.

Three references in spirit, not in copying: Greek black-figure vases, bronze-age Aegean frescoes, and tabletop dioramas. Every asset must be original.

## 2. Palette

| Role | Color | Hex |
|---|---|---|
| Terracotta base | warm red-orange | #B84A32 |
| Bronze | metal gold | #C99A4A |
| Wine-dark sea | deep purple-blue | #2B1B2F |
| Marble | warm off-white | #F0E6D2 |
| Olive | muted green | #6B7A3A |
| Storm | cold navy | #1E2A44 |
| Divine | pale gold | #F2D27A |
| Danger | vermilion | #D9442E |

Use no more than three dominant colors per screen. The sea is not bright blue; it is wine-dark. The UI is bronze on marble, not neon.

## 3. Shape language

- Odysseus: tall, angular, tired posture, bronze helmet, red cloak. Readable silhouette with a spear.
- Crew: small, distinct helmets and tools. Hoplite round shield; Archer bow; Sailor rope; Shield Wall large rectangular shield; Ballista wooden frame.
- Gods: large, flat, iconic; appear only in boon cards and rare cut-ins. Do not put gods on the battlefield as units.
- Monsters: oversized, asymmetric silhouettes. Cyclops has a single huge eye and a club; Sirens are bird-like, not mermaids; Scylla is a mass of heads, not a generic kraken.
- Ship: a black-hulled Aegean ship with a single sail, rope lines, and a painted eye on the bow. It is the emotional anchor.

## 4. Scale and camera

2D/2.5D orthographic view. The ship is small enough to read the deck, but large enough to feel like home. Monsters are 2-4x the ship height for scale. Use foreground waves, spray, and dust to sell weight.

Camera: mostly fixed, with a slight push-in on boss spawn and a shake on slam. No free camera. No zoom controls.

## 5. Animation

- Crew: 4-6 frame loops, snappy attacks, one hit reaction.
- Odysseus: 8-frame run cycle, spear thrust, shield raise, ultimate pose.
- Monsters: slow anticipation, fast impact. Telegraph attacks with a gold outline on the ground.
- Merging: two units slide together, a pottery-pattern burst, and a new silhouette. 0.35s, no long lock.
- Storm: screen-edge darkening, rain streaks, sail flutter; not a full-screen blur.

Reduced motion: disable screen shake, reduce rain and particles by 50%, keep telegraphs.

## 6. UI direction

The HUD is a captain's map crossed with a bronze shield. Parchment panels, bronze borders, hand-painted icons. No glassmorphism, no neon gradients.

- Hull: three ship icons, not a bar.
- Storm: bronze ring with a sun/sea motif; turns red.
- Wave: numeral in a laurel frame.
- Drachma: small owl coin.
- Skills: three large round buttons with clear icons and cooldown sweep.
- Boons: three tall cards with god symbol, name, one-line effect, and a small risk tag.

All text is English at launch. Use a readable serif for headings and a humanist sans for body. Avoid faux-Greek fonts that hurt readability.

## 7. VFX

- Hit: small black-figure shard burst.
- Merge: terracotta and gold ring.
- Ultimate: a vertical column of pale gold light with Greek key pattern.
- Boss rock: dust trail and a gold target ring.
- Ship damage: splinter burst and a red hull icon flash.
- Victory: olive leaves and a clear horizon.

No blood. No photoreal fire. No loot-box sparkle.

## 8. Audio thesis

Original music only. No film or game soundtracks. The score is built from lyre, aulos, frame drums, low strings, and sea ambience. It should feel ancient but not museum-stiff.

Music layers: home/map (calm lyre), battle (drums and low strings), boss (faster frame drum and horn), victory (open chord and voice-like synth). Crossfade in 0.6s. Duck under boss intro.

SFX: wood impact, bronze ring, arrow release, spear whoosh, rock crash, wave surge, ship creak, owl hoot for reward, UI tick. Keep SFX under 1.5s except boss roar.

## 9. Asset budget

MVP: 1 island background, 1 ship, 5 crew types x 4 tiers, 6 enemy types, 1 boss, 6 god cards, 1 UI atlas, 2 music tracks, 40 SFX. Target 12-15MB of art/audio before code.

Do not add a new island until the first island is under budget and runs at 60fps. Reuse silhouettes and palette across tiers; upgrade accents rather than full redraws.

## 10. Accessibility

- Colorblind mode adds shape badges to tier and lane.
- All critical text has a 4.5:1 contrast ratio.
- Telegraphs use shape and sound, not color alone.
- Text size setting: 100%, 115%, 130%.
- No flashing above 3Hz.
- Subtitles for any voiced barks; launch can be text-only.
