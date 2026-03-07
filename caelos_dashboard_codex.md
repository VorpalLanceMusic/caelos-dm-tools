# CAELOS DM DASHBOARD — DEV CODEX

## FILE
`caelos_dm_dashboard.html` — single-file HTML PWA, ~114kb, 2651 lines. GitHub Pages hosted. Pixel 10 Pro target. No build tools, no external JS.

## DESIGN SYSTEM
```
--blood:   #9b1a1a   --ember:   #c8722a   --gold:    #e8c87a
--bg:      #0a0a0f   --surface: #12121a   --raised:  #1a1a26
--border:  #2a2a3a   --text:    #ddd8cc   --sub:     #6a6860
--nav-h:   62px      chartreuse:#b4ff00   (BF active alert)
```
Fonts: Cinzel (headers) + Crimson Text (body). Milestone leveling — no XP anywhere.

---

## ARCHITECTURE

- Two party datasets `partyData.A` and `partyData.B`, swapped via party badge toggle
- `sessionStorage` for per-date weather keyed as `Y-M-D`
- Nav tabs: **Combat** · **Weather** (replaced Settings)
- `currentWeather` global string drives both Weather tab and Battlefield Effects in sync

### PC Data Shape
```js
{ name, cls, maxHp, curHp, tempHp, deathSaves:{s,f}, conditions:[], customConditions:[] }
// customConditions: Evelyn only — [{name:'Raging',cls:'cond-gold',card:'conditioned-gold'},{name:'Hungry',cls:'cond-blood',card:'conditioned-blood'}]
```

---

## PARTY A — Level 8

| Name | Race · Class | HP | Key Stats | Notes |
|------|-------------|-----|-----------|-------|
| Anauk | Dwarf · Fighter · Rune Knight | 95 | AC 18, Init +1, PP 12, Rune DC 17 | Giant's Might, Stone/Storm/Fire/Frost/Cloud/Hill runes |
| The | Aasimar · Rogue (Assassin/Mastermind hybrid) / Warlock | 68 | AC 15, Init +5, PP 16, Snk 4d6, Spell DC 14 | Patron: The Feintless One (homebrew). Darkness+Devil's Sight combo |
| Evelyn | Dhampir · Cleric / Barbarian (Vampire) | 52 | AC 14, Init +2, PP 14, Spell DC 16, +8 | Blood Rage system. Track Concentration. Custom: Raging + Hungry |
| Lyle | Halfling · Ranger (Beast Master — UA, not 2014 PHB) | 58 | AC 15, Init +2, PP 14, Spell DC 13 | UA companion uses player's bonus action |
| Rooker | Kenku · Rogue (Swashbuckler) | 55 | AC 15, Init +4, PP 14, Snk 4d6 | Rakish Audacity, Fancy Footwork |

**PENDING:** All Party A multiclass level splits (awaiting players). Magic item inventories.

---

## PARTY B — Level 4

| Name | Race · Class | HP | Key Stats | Notes |
|------|-------------|-----|-----------|-------|
| Lorelei | Fire Genasi · Druid (Circle of Midnight) | 32 | AC 13, Init +1, PP 13, Spell DC 13, +5 | Transitioning Wildfire → Circle of Midnight (homebrew) |
| Luella | Changeling · Bard 2 / Rogue 2 | 38 | AC 13, Init +3, PP 13, Snk 1d6, Spell DC 13 | Changeling alter appearance. Bardic Inspiration available |
| Val | Tiefling · Artificer | 28 | AC 14, Init +1, PP 12, Spell DC ? | Subclass TBD. Infusions not documented yet |

**PENDING:** Val's Artificer subclass. Magic item inventories. Lorelei's transition timing.

---

## HOMEBREW MECHANICS

### The — Patron: The Feintless One
- **Passive:** Aberrations Wisdom save or retarget away from The
- **6th:** Stolen Death — reaction kill-swap on dying creature; drain escalates 1d4→1d8→1d10 per use without short rest
- **10th:** Shadow Step — bonus action teleport to darkness within 60ft
- **14th:** Feintless — immune Surprised+Stunned; 1/day action heal 1d10+level; 1/day reaction force disadvantage on incoming attack

### Evelyn — Vampire Barbarian (Blood Rage)
**Blood system:** Fresh (<24h)=1 charge/~10fl oz · Old (24-48h)=double needed · Expired=unusable · Full human=10 charges · No fresh blood=1d12 necrotic on activation

**Rage:** STR/DEX/CON floor 18 · adv STR checks/saves · +2 melee dmg (scales) · resist B/P/S+necrotic · fang attacks heal half piercing dealt · no spellcasting

**Paths (unlock sequentially within path):**
- Brutal: Vicious Fangs · Brute Hurl · Crimson Claws · Blood Lash · Intimidating Stance · Sanguine Devour · Ichor Bile · Eviscerate
- Supernatural: Spiderclimb · Transform Bat/Giant Bat · Flight/Misty Step · Teleportation · Pyrokinesis
- Psionic: Hypnotic Gaze · Telepathy · Fear · Telekinesis · Mind Control

### Lorelei — Circle of Midnight (homebrew Druid)
- **2nd:** Midnight Spirit — expend Wild Shape → summon spirit (5×druid level + WIS + CON mod HP); 10ft AoE 2d10 necrotic on appear; lasts 1hr
- **6th:** Deathly Magic — +1d8 necrotic to necrotic/force/poison/psychic spell damage
- **10th:** Soul Bound — immunity disease+poison dmg+poisoned condition; ignore necrotic resistance
- **14th:** Midnight Master — summon as bonus action (was action); temp HP = WIS mod + druid level when spirit/possessed creature drops enemy to 0

---

## DASHBOARD FEATURES BUILT

### Combat Tab
- **Round tracker** — +/- buttons, round number bumps with scale animation on change, round note label (Opening/Early/Mid/Climax etc.)
- **Section headers** — color-coded (red/orange/teal/blue/purple/gray), tap to expand with slide-unfurl animation
- **Sections:** Start of Combat · During Combat · End of Combat · Stakes · Active Conditions — Enemy
- **Checklist items** — cb.checked pops with scale burst; item text strikes through
- **Active Conditions — Enemy** — full tag set with color families (see below); `#enemy-cond-wrap` block animates matching dominant condition; `clearEnemyConditions()` button

### PC Party Cards
- `#pccard-{party}-{idx}` — each card has id for direct DOM targeting
- **HP system:** current HP display · max HP · temp HP input · damage/heal apply buttons with fixed-width 64px number input · HP bar with scanline sweep + healed flash + damaged flash
- **Stat pills** — AC, Init, PP, plus class-specific (Sneak Attack, Spell DC, etc.)
- **Condition tracker** — tap-to-toggle preset list per card; Clear button appears only when conditions active
- **Downed tips** — appear inside card only at 0HP (narrate it, ask for memory, press advantage, stakes reminder)
- **Death saves** — pips hidden from table view (inner ring glow only DM can read); success pip blooms green, fail pip jitters
- **Edit mode** — inline editing of max HP and stat values
- **Long Rest** — restores all HP, clears temp HP, death saves, and conditions

### Condition Color System
| Color | Class | Conditions | Card Animation |
|-------|-------|------------|----------------|
| Purple | cond-purple / on-purple | Frightened, Charmed, Stunned | Hue-shift + blur pulse (psychic drift) |
| Green | cond-green / on-green | Poisoned, Exhaustion, Blinded, Petrified | Uneven brightness flicker (sickly) |
| Blue | cond-blue / on-blue | Grappled, Restrained, Prone, Invisible | Slow vertical bob (weighted down) |
| Gold | cond-gold / on-gold | Concentrating, Cursed, Blessed | Radar sweep clockwise (magical) |
| Blood | cond-blood / on-blood | Hungry (Evelyn only), Bloodied (enemy) | Double heartbeat scale pulse |
| Gray | cond-gray / on-gray | Incapacitated | Desaturate + dim flicker (dying signal) |

- **Downed (0HP):** red overrides all — thick bright red border, dim red bg, slow heavy pulse
- **Danger HP (≤25%):** irregular torch-flicker, red border, two quick pulses then rest
- Priority order (downed wins over all): downed > purple > green > blue > gold > blood > gray

### Section Active Glows
`has-active` class on section triggers per-color animation on `c-head`:
- `.s-wrap-orange` → amber pulse · `.s-wrap-teal` → teal pulse · `.s-wrap-blue` → blue pulse · `.s-wrap-purple` → purple pulse

### Battlefield Effects Section
- `#bf-section` — chartreuse (#b4ff00) active glow + pulsing border + scanline crawl when any effect ON
- Weather dropdown syncs bidirectionally with Weather tab
- Other Effect: free-text name + details textarea
- Both toggles breathe when ON

### Weather Tab
- **Caelos calendar:** month (10 named months) + day 1–30 + year (default 1219 I.D.)
- **7-day week strip** — tap any day to jump; current date highlighted
- **Weather stored** in sessionStorage keyed `Y-M-D`
- **Roll input** (2d100+1d20, range 3–220) auto-resolves to weather type; 🎲 random roll button
- **Manual dropdown** — full weather table
- **Weather card** — icon, name, description, combat effects
- **Push to Battlefield** button — jumps to combat tab, opens BF section, syncs weather
- **Reset Weather button** — sets both tabs to `clear` (Sunny)
- **Seasonal bias:** Joy/Kindred/Medial=heat · Solace/Sacrifice/Silence=cold · shoulders=random

### Weather Roll Table (2d100+1d20)
- 1–10: Acid Rain / Petrifying Rain (Supernatural) / Freezing Rain / Hurricane (1d4)
- 11–40: Blizzard / Thunderstorm / Tornado (1d3)
- 41–80: Overcast / Foggy (1d2)
- 81–130: Clear/Sunny
- 131–165: Hazy / Foggy / Moderate Rain (1d3)
- 166–180: Extreme Heat (summer) or Extreme Cold (winter)
- 181–220: Sleeping Fog / Veil of Stars (1d2)

### Reset Buttons
- **Long Rest** — full HP restore + clear temp HP + clear death saves + clear conditions (all PCs in current party)
- **Reset Checks** — clears all checklist checks, enemy tags, PC conditions, battlefield effects (weather/other)
- **Reset Round** — round back to 1
- **Full Reset** — calls resetChecks + resetRound + clears weather in both tabs

---

## ANIMATION INVENTORY

| Element | Animation | Notes |
|---------|-----------|-------|
| App title "CAELOS DM" | `title-breathe` 6s | Gold glow expands/contracts |
| Party badge | `badge-breathe` 5s | Blood-red border pulse |
| Active nav icon | `nav-icon-settle` 4s | Gold drop-shadow breathe |
| Nav tap | `nav-tap` 0.25s | Compress then spring |
| Round number (on change) | `round-bump` 0.35s | Scale bloom to white then settle |
| Round button tap | `btn-thud` 0.2s | Quick compress |
| Section body open | `section-unfurl` 0.22s | Fade + translateY slide down |
| Checkbox check | `cb-pop` 0.25s | Scale burst |
| Tag activate | `tag-ignite` 0.3s | Scale pop |
| HP bar | `hp-scan` 3s infinite | Scanline sweep |
| Weather card — Clear | `wx-anim-clear` 5s | Warm gold breathe, slow and peaceful |
| Weather card — Overcast | `wx-anim-overcast` 6s | Cool blue-gray listless pulse |
| Weather card — Moderate Rain | `wx-anim-modrain` 3s | Vivid deep blue downward drip pulse, distinct from overcast |
| Weather card — Hazy | `wx-anim-hazy` 7s | Warm amber shimmer drift, murky |
| Weather card — Fog | `wx-anim-fog` 8s | White-gray drift, saturate fades in/out |
| Weather card — Extreme Heat | `wx-anim-exheat` 2.8s | Aggressive amber-red throb, uncomfortable |
| Weather card — Extreme Cold | `wx-anim-excold` 3.2s | Icy blue crystalline flicker, brittle |
| Weather card — Thunderstorm | `wx-anim-thunder` 4.5s | 89% quiet, then sharp electric white crack + afterglow |
| Weather card — Tornado | `wx-anim-tornado` 2.6s | Erratic lurching float ±5px, uneven keyframes, clockwise border sweep |
| Weather card — Blizzard | `wx-anim-blizzard` 2.2s | Cold strobe + micro translateX shudder |
| Weather card — Acid Rain | `wx-anim-acid` 2.5s | Sickly green hue-rotate corrosive pulse |
| Weather card — Petrifying Rain | `wx-anim-petrify` 6s | Slow stone-gray creeping desaturation — severity: Supernatural (purple badge) |
| Weather card — Freezing Rain | `wx-anim-freeze` 3.8s | Two sharp brittle ice spikes at irregular intervals |
| Weather card — Hurricane | `wx-anim-hurricane` 1.8s linear | Gentle vertical float ±3px, rotating border light source |
| Weather card — Sleeping Fog | `wx-anim-sleepfog` 9s | Eerie slow purple-white drift, dreamy and wrong |
| Weather card — Veil of Stars | `wx-anim-veilstars` 10s | Cosmic hue-rotate drift, shifting corner light sources |
| HP damage | `hp-damage-flash` 0.4s | Dim + desaturate |
| HP heal | `hp-healed-flash` 0.5s | Brightness flash |
| Death save success | `pip-success` 0.4s | Green bloom + outward flare |
| Death save fail | `pip-fail` 0.35s | Horizontal jitter |
| BF toggle ON | `toggle-on-breathe` 2s | Chartreuse glow breathe |
| BF active box | `bf-active-pulse` 3s + `bf-scanline` 2.5s | Border pulse + crawling scanline |
| Danger HP | `danger-flicker` 2.8s | Irregular torch flicker |
| Downed (0HP) | `downed-pulse` 2s | Heavy red border heartbeat + red bg |
| Purple condition | `cond-psychic` 4s | Hue-shift + micro-blur |
| Green condition | `cond-poison` 3s | Uneven brightness flicker |
| Blue condition | `cond-restrained` 3.5s | Vertical bob |
| Gold condition | `cond-magical` 3s | Clockwise radar sweep |
| Blood condition | `cond-hunger` 1.6s | Double heartbeat (10%→rest→35%→long rest) |
| Gray condition | `cond-incap` 2.5s | Desaturate + dim irregular flicker |
| Section has-active | `glow-orange/teal/blue/purple/red` 2s | Per-color header glow pulse |

---

## KEY FUNCTIONS

```
renderPCs()               — full re-render of party grid
updateHPDisplay(p,idx)    — targeted HP update without re-render
applyDmg/applyHeal        — apply + flash HP bar
togglePCCondition(p,idx,name) — toggle + update card glow
updateEnemyCondWrap()     — recompute dominant condition animation on enemy block
toggleTag(tag,cls)        — enemy condition tag toggle + calls updateEnemyCondWrap
getCondDef(name,pc)       — looks up condition def, checks customConditions first
updateSectionGlows()      — sweep all sections for has-active class
updateBFSectionGlow()     — chartreuse BF section glow logic
clearPCConditions(p,idx)  — wipe conditions array + re-render
clearEnemyConditions()    — wipe all enemy tags + update wrap
resetWeather()            — set both tabs to 'clear'
resetChecks()             — full combat state wipe (checks/tags/conditions/BF/weather)
resetAll()                — resetChecks + resetRound + clear weather
longRest()                — full HP restore + wipe conditions/death saves
renderWeatherCard()       — render weather card in wx-card-area; applies wx-anim-{key} class from WX_ANIM map for per-weather animation
syncBFWeather()           — push currentWeather to BF dropdown
pushWeatherToBattlefield()— push + switch to combat tab + open BF section
```

---

## TEXTURE SYSTEM

All patterns are CSS-only via `background-image` layering. No images. All rgba, never competes with text or animations. Current opacity range: 6–14%.

| Surface | Pattern Type | Color | Notes |
|---------|-------------|-------|-------|
| `.app::before` | Dual offset dot grid | white | Fixed pseudo-element, `z-index:0`, `pointer-events:none` — does NOT touch layout |
| `.app-header` | 135° diagonal etch | white | Layered over existing blood→dark gradient via separate `background-image` |
| `.bottom-nav` | Tight 8px chainmail dot grid | white | |
| `.c-head` | 120° brushed metal hairlines | white | Different angle from header = distinct material feel |
| `.c-body` | Gold crosshatch (0° + 90°) | gold | Aged parchment — two gradient layers |
| `.pc-card` | Corner radial depth + 45° diagonal grain | white | Top-left lighter, bottom-right darker = carved stone block |
| `.hp-action-row` | Horizontal ruled lines 12px | white | Ledger / surgeon's chart feel |
| `.cond-wrap` | 12px gold dot grid | gold | Old tactical map |
| `#enemy-cond-section .c-body` | 150° red hatch + 0° gold rules | red + gold | Dangerous territory read |
| `.party-sec-head` | 135° gold diagonal | gold | |
| `.reset-bar` | Tight 10px square grid (0° + 90°) | white | Forge floor grating |
| `#tab-weather` | Three offset star-field dot layers | gold + white | Prime-number sizes (47/23/71px) so pattern never repeats predictably |
| `.wx-card-head` | 90° vertical column lines | white | Weather station readout feel |
| `.wx-card-body` | 160° diagonal grain | white | |
| `.wx-effects` | 0° horizontal rules 8px | blood red | Reinforces blood left-border |
| `.wx-pre-cell` | 90° vertical hairlines | white | |

**Critical:** `.app` must have `position: relative` for `::before` to work. Body/html get NO background-image — layout lives there and must stay clean.

- **Bestiary tab** — HIGH PRIORITY (referenced in codex)
- **Encounter tab** — MEDIUM
- **Prep tab** — MEDIUM
- **Party A multiclass level splits** — need from players
- **Val Artificer subclass** — need from player
- **Full magic item inventories** — both parties
- **Evelyn's second Sceplture Supreme item** — TBD in codex
- **Codex cross-reference** — caelos_codex_condensed_v4c.md not yet integrated
