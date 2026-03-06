# Caelos DM Tools — App Codex
*Reference document for Claude. Load this at the start of any session to continue development.*

---

## Overview

A suite of custom DM tools for the Caelos campaign, hosted on GitHub Pages as standalone HTML files. Built for a DM who runs two parallel parties (Party A in-person, Party B virtually over Discord + Roll20). All tools are single-file HTML with embedded CSS and JS — no frameworks, no dependencies, no server required.

**GitHub Repo:** `https://YOURUSERNAME.github.io/caelos-dm-tools/`
**Design Philosophy:** Dark fantasy aesthetic. Cinzel font for headers, Crimson Text for body. Color palette: near-black bg (#0a0a0f), blood red accent (#9b1a1a), gold highlights (#e8c87a). Sleek, readable, touch-optimized for Android (Pixel 10 Pro).

---

## Completed Tools

### 1. Combat Flow Cheat Sheet
**File:** `combat_cheatsheet_phone.html`
**URL:** `https://YOURUSERNAME.github.io/caelos-dm-tools/combat_cheatsheet_phone.html`
**Purpose:** Phone-sized interactive DM combat reference. Pinned to home screen on Pixel 10 Pro for in-person Party A sessions. Also used alongside Roll20 for Party B virtual sessions.

**Features:**
- Round counter with contextual notes (Opening / Intensifying / Escalate / Desperate / End this)
- Six collapsible sections with color-coded accent bars:
  - 🔴 Every Round — Never Skip (red) — open by default
  - 🟠 Start of Combat (orange)
  - 🟢 During Combat (teal)
  - 🔵 End of Combat (blue)
  - 🟣 Stakes — Keep It Real (purple)
  - 🟡 Player Goes Down (yellow)
  - ⬡ Active Conditions (gray)
- Tappable checkboxes on every item (red check when ticked, strikethrough text)
- Condition tag tracker — Enemy (red) and Party (blue) conditions toggleable independently
- Three reset buttons: Checks only / Round only / Full reset
- Blood red favicon with ⚔ sword emoji on home screen icon
- Touch targets minimum 44-52px throughout
- Fonts: Cinzel (headers) + Crimson Text (body) via Google Fonts

**Design notes:**
- item-text: 17px, font-weight 600
- item-note: 14px, italic, color var(--sub)
- Sections default closed except "Every Round"
- warn items: red text (#e06868)
- gold items: gold text (var(--gold))

---

## Planned Tools

### 2. Bestiary / Statblock Viewer
**Status:** Planned
**Purpose:** Store and display creature statblocks with full formatting. DM can reference them mid-session without digging through files.

**Desired features:**
- Pre-loaded with campaign creatures (Gnawmaw and others as built)
- Ability to add new statblocks manually
- Each statblock displays: creature image (if available), name, type/alignment, AC/HP/Speed, ability scores, saves/skills, traits, actions, bonus actions, reactions, legendary actions, lore box
- Search/filter by name or CR
- Cross-reference with combat sheet (e.g. tap a condition in combat sheet → see what it does)
- Statblock HTML format already established — see Gnawmaw statblock below as template

**Gnawmaw statblock summary (CR 12):**
- Huge Aberration, Chaotic Evil
- AC 15, HP 189 (18d12+72), Speed 30/climb 20/fly 20 (hover)
- STR 22 DEX 12 CON 18 INT 7 WIS 14 CHA 16
- Passive pull mechanic (Maw of the Hollow), Tendril Senses (6 destroyable tendrils), Devouring Presence (aura damage)
- Multiattack: Gnash + Tendril Lash; Gullet Grasp on grappled targets; Hungering Wail recharge 5-6
- Legendary Actions (3): Tendril Strike / Creeping Advance / Hollow Pull / Gut Scream
- Reaction: Instinctive Swallow (swallows creatures reduced to 0hp)
- Built for Party A (level 8). No XP listed (milestone leveling campaign).
- Statblock files: `gnawmaw_statblock.html`, `gnawmaw_statblock.png`, `gnawmaw_statblock.pdf`

### 3. Per-Creature Combat Choreography Sheets
**Status:** Planned
**Purpose:** One sheet per major encounter creature. Statblock + DM tactical guide for that specific fight. Prevents defaulting to attack/next turn/attack pattern.

**Desired content per sheet:**
- Turn priority guide (what the creature does in round 1 vs round 3 vs bloodied)
- When to use each ability
- Legendary action priority order
- Environmental stakes for that specific encounter
- What happens when a player goes down
- Escape/retreat conditions
- Emotional arc of the fight (how the creature's behavior should shift)

### 4. Session Recap Generator
**Status:** Planned
**Purpose:** Paste in raw session notes → outputs a formatted "Previously on Caelos" summary. Separate versions for Party A and Party B.

### 5. Word of the Whelk Formatter
**Status:** Planned
**Purpose:** Input raw story content → outputs formatted in-world newspaper layout matching established Whelk aesthetic (broadsheet style, column layout, headlines, bylines).

### 6. NPC Quick Reference
**Status:** Planned
**Purpose:** Searchable card-based NPC browser. Each card: name, faction, appearance, motivation, what they know, party relationship. Filterable by party (A/B/Both) and faction.

---

## Campaign Context (for tool-building reference)

**Setting:** Caelos — steampunk-adjacent fantasy world. Two main regions: Calmaar (democratic, no death penalty) and Lethbridge (authoritarian, racially segregated Dome district).

**Two parallel parties:**
- **Party A** — Level 8. In-person sessions. Anauk (dwarf), The (masked human), Evelyn Verbena (vampire-adjacent), Lyle Frothbottom (halfling postman), Rooker (kenku pirate). Companion: Gertie the giant snail.
- **Party B** — Level 4. Virtual via Discord + Roll20. Lorelei (fire genasi), Val (tiefling), Luella Goldeneye (changeling).

**Leveling:** Milestone only. No XP anywhere in any tool.

**Tone:** Mature, cinematic, story-driven. Combat should feel dangerous and earned. DM tendency to forget mechanical complexity mid-fight — tools should actively combat this.

---

## Development Notes

**How statblocks are built:**
Single-file HTML with embedded creature image as base64. Parchment texture background, red divider bars, dark legendary actions block, lore box at bottom with in-world citation. Converted to PNG/PDF via wkhtmltopdf + pdftoppm for Legendkeeper import.

**Fonts used across all tools:**
- Cinzel (Google Fonts) — section headers, titles, labels
- Crimson Text (Google Fonts) — body text, notes, descriptions
- Fallback: Georgia, serif

**Color variables (all tools should use these):**
```
--blood:   #9b1a1a
--ember:   #c8722a
--gold:    #e8c87a
--bg:      #0a0a0f
--surface: #12121a
--raised:  #1a1a26
--border:  #2a2a3a
--text:    #ddd8cc
--sub:     #6a6860
```

**Android home screen icon:**
SVG favicon embedded inline. Blood red (#9b1a1a) rounded square background, ⚔ emoji centered. Added via `<link rel="icon">` and `<link rel="apple-touch-icon">` in `<head>`.

**Hosting:**
GitHub Pages, public repo. Files uploaded directly via GitHub web interface. No build process, no CLI required — DM is not a developer.

---

## How to Use This Document

Paste this into a new Claude chat along with the current HTML file(s) when continuing development. Say something like:

*"Here's the Caelos DM Tools Codex. I want to [add a bestiary / build the Gnawmaw choreography sheet / improve the combat cheat sheet]. Here's the current HTML: [paste file contents]."*

Claude will have full context on the design system, what's been built, what's planned, and how everything fits together.

---

*Last updated: Session with Rhapsody, March 2026.*
