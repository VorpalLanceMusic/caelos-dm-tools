# Caelos DM Dashboard — App Codex v2
*Load this at the start of any session to continue development.*
*Paste this doc + the current `caelos_dm_dashboard.html` contents into a new chat.*

---

## What This Is

A single-file HTML mobile web app — the Caelos DM Dashboard. Full DM toolkit for the Caelos campaign, hosted on GitHub Pages. Designed for a Pixel 10 Pro (Android), pinned to home screen as a PWA shortcut. Dark fantasy aesthetic throughout.

**GitHub Pages URL:** `https://YOURUSERNAME.github.io/caelos-dm-tools/caelos_dm_dashboard.html`
**File:** `caelos_dm_dashboard.html`

---

## Design System

**Fonts:** Cinzel (headers/labels/nav/buttons) + Crimson Text (body/notes) via Google Fonts
**Color Variables:**
```
--blood:   #9b1a1a   (accent red, danger states, borders)
--ember:   #c8722a   (warm orange, edit mode)
--gold:    #e8c87a   (highlights, active nav, PC names, titles)
--bg:      #0a0a0f   (page background)
--surface: #12121a   (section backgrounds, nav bar)
--raised:  #1a1a26   (elevated elements, card backgrounds)
--border:  #2a2a3a   (borders, dividers)
--text:    #ddd8cc   (primary text)
--sub:     #6a6860   (secondary text, notes, inactive labels)
--nav-h:   62px      (bottom nav height)
```

**Touch targets:** Minimum 44-56px height throughout
**Layout:** Fixed full-height app shell. Header + scrollable tab content + fixed bottom nav.
**No XP anywhere — milestone leveling campaign.**

---

## App Structure

```
.app (flex column, full viewport height)
├── .app-header         — Global header: title + Party A/B toggle badge
├── .tab-content        — Scrollable, contains all tab panels
│   ├── #tab-combat     — Combat flow cheat sheet + PC party tracker (COMPLETE)
│   ├── #tab-bestiary   — Creature statblock viewer (STUB)
│   ├── #tab-encounter  — CR calculator + initiative tracker (STUB)
│   ├── #tab-prep       — Session prep checklist (STUB)
│   └── #tab-settings   — Preferences + party selector (STUB)
└── .bottom-nav         — Five tab buttons: ⚔️ 🐉 🎲 📋 ⚙️
```

---

## Completed Features

### Global Header
- Title: "⚔ Caelos DM" (Cinzel, gold, 22px)
- Subtitle: "Dungeon Master Dashboard"
- **Party badge** (top right) — tapping toggles between "Party A" and "Party B"
  - Switches the entire PC section to the correct party
- Blood red radial glow, 2px blood red bottom border

### Bottom Nav
- Five tabs: Combat ⚔️ / Bestiary 🐉 / Encounter 🎲 / Prep 📋 / Settings ⚙️
- Active tab: gold label + scaled icon + red top accent bar
- Tab switch resets scroll to top
- Cinzel, 9px, uppercase, letter-spacing 1px

---

### Tab: Combat ⚔️ — COMPLETE

#### Round Counter
- − / + buttons (42px, turns blood red on press)
- Round number (Cinzel 36px, gold, subtle glow)
- Contextual note updates automatically:
  - Round 1: "Opening"
  - Round 3: "Intensifying"
  - Round 5: "Escalate"
  - Round 7: "Desperate"
  - Round 10: "End this"

#### Seven Collapsible Sections
| Section | Accent Color | Default State |
|---|---|---|
| 🔴 Every Round — Never Skip | #9b1a1a red | OPEN |
| 🟠 Start of Combat | #c8722a orange | Closed |
| 🟢 During Combat | #2a9a72 teal | Closed |
| 🔵 End of Combat | #2a6aaa blue | Closed |
| 🟣 Stakes — Keep It Real | #7a2aaa purple | Closed |
| 🟡 Player Goes Down | #aaaa2a yellow | Closed |
| ⬡ Active Conditions | #484848 gray | Closed |

Item types: default (var(--text)) / `.warn` red #e06868 / `.gold` gold var(--gold)

#### Active Conditions
- Enemy conditions toggle red (`on-red`)
- Party conditions toggle blue (`on-blue`)
- Crimson Text 15px pill tags

#### Reset Bar (bottom of combat tab)
- ↺ Checks — clears all checkboxes and condition tags
- ↺ Round — resets round counter to 1
- ↺ Full Reset — both of the above

---

### Party Combat Stats Section (inside Combat tab) — COMPLETE

**Gold accent bar** distinguishes it from enemy-focused sections.

#### Toolbar (three buttons)
- **✎ Edit Stats** — unlocks all stat fields and Max HP for editing. Card border turns amber. Button turns orange. Tap "✔ Lock Stats" to re-lock.
- **↺ Reset HP** — restores full HP and clears Temp HP for all PCs in current party. Does NOT clear death saves.
- **🌙 Long Rest** — restores full HP, clears Temp HP, AND clears all death saves for entire current party. (Homebrew rule: death saves reset on long rest.)

#### PC Card Structure (per player)
Each card contains, top to bottom:

1. **Name** (Cinzel gold) + **Class** (italic, muted)
2. **HP Section:**
   - Large Current HP display — color coded:
     - Normal: var(--text) white
     - ≤50% max: var(--ember) orange
     - ≤25% max: #e06868 red
   - Max HP (static display / editable in edit mode)
   - Temp HP (always editable inline, blue color #68a8d8)
   - HP bar — green → orange → red as HP drops
   - **Damage input** + red Apply button (damage hits Temp HP first, then Current HP)
   - **Healing input** + green Apply button (cannot exceed max HP)
   - Apply buttons work via typed number — no prompt() dialogs
3. **Stat Pills** (consistent order across all PCs):
   AC (gold highlight) → Init → PP → class-specific stats
   - Editable text inputs in edit mode
4. **Note** (italic, muted — turns orange/red if contains ⚠)
5. **Death Saves row:**
   - Three green pip buttons = Successes
   - Three red pip buttons = Failures
   - Tap pip to fill up to that pip; tap filled pip to clear back down
   - **✕ Clear** button clears just that PC's death saves
   - Card border turns blood red when PC is at ≤25% HP

---

## Current PC Data (Party A — Level 8)

**IMPORTANT: All stats below are placeholder estimates. Update with real character sheet values.**

| Name | Class | Max HP | AC | Init | PP | Special |
|---|---|---|---|---|---|---|
| Anauk | Dwarf · Fighter · Rune Knight | 95 | 17 | +1 | 13 | STR Save +8, Prof +3 |
| The | Aasimar · Rogue · Assassin | 68 | 17 | +4 | 16 | Sneak Attack ?d6, Prof +3 |
| Evelyn | Dhampir · Cleric/Barbarian | 52 | 14 | +2 | 14 | Spell DC 16, Spell +8 |
| Lyle | Halfling · Ranger | 58 | 15 | +2 | 14 | Prof +3 |
| Rooker | Kenku · Rogue | 55 | 15 | +4 | 15 | Sneak Attack 4d6, Prof +3 |

**PC Notes:**
- **Anauk:** CLOUD RUNE — redirect attack to another target (this thing is a bitch, don't forget it)
- **The:** Assassinate: auto-crit on surprised target. Face never visible. (Sneak Attack dice need confirming)
- **Evelyn:** Rage: resist B/P/S dmg, adv STR checks · ⚠ Track Concentration spells
- **Lyle:** Lucky: reroll 1s on attack/ability/save
- **Rooker:** Sneak Attack needs advantage or adjacent ally

## Current PC Data (Party B — Level 4)

| Name | Class | Max HP | AC | Init | PP | Special |
|---|---|---|---|---|---|---|
| Lorelei | Fire Genasi · Druid | 32 | 14 | +2 | 12 | Spell DC ?, Prof +2 |
| Val | Tiefling · Artificer | 38 | 18 | +0 | 11 | Spell DC 13, Spell +5 |
| Luella | Changeling · Bard/Rogue | 28 | 14 | +3 | 14 | Sneak Attack 2d6, Spell DC ? |

**PC Notes:**
- **Lorelei:** Wild Shape available. Fire resistance. Darkvision 60ft. (Spell DC needs confirming)
- **Val:** Artificer — check active Infusions, they may affect other party members' stats
- **Luella:** Bardic Inspiration available. Changeling: alter appearance at will. (Spell DC needs confirming)

---

## Planned Features (build in future sessions)

### Tab: Bestiary 🐉 — HIGH PRIORITY
Searchable creature statblock browser.

**Required features:**
- Scrollable creature list — name, CR badge, type
- Tap → full statblock view
- Search/filter by name or CR
- "Missing statblock" flag for incomplete entries
- Add new creature button
- **Each creature card also gets:**
  - Current HP tracker (same damage/heal input system as PC cards)
  - AC display
  - Key combat stats (relevant saves, DCs, recharge abilities)
  - Condition tracker (same pill tags as combat section)
  - Death/defeated toggle
- Statblock display matches Gnawmaw format:
  - Name, type/alignment
  - AC / HP / Speed
  - Ability score grid with modifiers
  - Saves, Skills, Immunities, Senses, Languages
  - Red divider bars
  - Traits, Actions, Bonus Actions, Reactions
  - Legendary Actions (dark block)
  - Lore box (in-world citation, italic)
- NO XP anywhere

**Pre-loaded creature (statblock complete):**
- Gnawmaw (CR 12) — built for Party A (level 8)
  - AC 15, HP 189, Huge Aberration Chaotic Evil
  - Passive pull mechanic, grasping hands in throat, Hungering Wail recharge 5-6
  - 3 Legendary Actions, Instinctive Swallow reaction

### Tab: Encounter 🎲 — MEDIUM PRIORITY
- Initiative tracker: add combatants, sort by init, track turns, damage/heal, conditions per combatant
- CR budget calculator: input party level + size + difficulty → CR suggestions
  - Party A defaults: Level 8, 5 players
  - Party B defaults: Level 4, 3 players
- Round counter synced with Combat tab

### Tab: Prep 📋 — MEDIUM PRIORITY
Pre-session checklist, same collapsible section structure as Combat tab.
Suggested sections: Before You Start / Story & Stakes / NPCs / Encounters / Contingencies / End of Session

### Tab: Settings ⚙️ — LOW PRIORITY
Party selector, theme options, app version info.

---

## File Inventory (GitHub Repo)

| File | Description | Status |
|---|---|---|
| `caelos_dm_dashboard.html` | Main DM dashboard — upload every new version | ✅ Active |
| `caelos_dm_dashboard_codex.md` | This file | ✅ Current |
| `combat_cheatsheet_phone.html` | Standalone combat sheet (superseded) | Keep as backup |
| `gnawmaw_statblock.html` | Gnawmaw interactive statblock | ✅ Complete |
| `gnawmaw_statblock.png` | Gnawmaw for Legendkeeper import | ✅ Complete |
| `gnawmaw_statblock.pdf` | Gnawmaw PDF | ✅ Complete |
| `caelos_dm_tools_codex.md` | Original tools codex (superseded) | Archive |
| `README.md` | GitHub default | Ignore |

---

## How to Start a New Session

1. Paste this codex into a new Claude chat
2. Also paste the full contents of `caelos_dm_dashboard.html`
3. Tell Claude what you want to build next
4. Claude drops new features into the existing shell

**Suggested next session:** Build the Bestiary tab — highest immediate value, Gnawmaw statblock already exists as a template, and creature HP/stat tracking was explicitly requested.

---

## Known Issues / To Fix
- All PC stats are placeholder estimates — need real character sheet values entered via Edit Stats mode or updated in the codex for next session
- Lyle's class is unknown — update when confirmed
- Lorelei's class is unknown — update when confirmed
- Party B classes need verification generally

---

*Last updated: Session with Rhapsody, March 2026. v2.*
