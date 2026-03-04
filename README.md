# D2R Stash Checker

A screen-reader based stash tracker for Diablo II Resurrected.  
No save file access. No ToS violations. Hover an item in-game → add it to your online stash.

---

## How It Works

1. User hovers over an item in D2R — the tooltip appears on screen
2. App captures the tooltip region (screenshot / screen reader)
3. OCR extracts the text
4. Text is matched against the data files in this repo
5. Item is stored in the user's online stash profile
6. Stash can be shared, browsed, and traded publicly

---

## Repository Structure

```
D2R-MuleChecker/
├── README.md
├── LICENSE
└── data/
    └── items/               ← All game data files (raw from D2R CASC)
        ├── [CORE]           ← Base item definitions
        ├── [AFFIXES]        ← Magic/Rare/Unique mod definitions  
        ├── [SUPPORT]        ← Types, stats, inventory layout
        └── [OPTIONAL]       ← Useful later, not critical now
```

---

## Data Files Reference

All files are tab-separated `.txt` extracted directly from D2R's CASC archives.  
They are Blizzard's native data format and are read at runtime by the app — no pre-processing needed.

---

### ⚔️ CORE — Base Item Definitions
> These are the essential files. Every item in the game is defined here.

| File | Purpose |
|------|---------|
| `weapons.txt` | All weapons — damage, speed, requirements, socket count, inventory grid size, sprite filenames. Covers all 3 tiers (Normal / Exceptional / Elite). |
| `armor.txt` | All armor pieces — defense ranges, requirements, block chance, grid size, sprite filenames. Includes helms, body armor, gloves, boots, shields, belts. |
| `misc.txt` | Miscellaneous items — gems, runes, potions, scrolls, keys, charms, jewels, quest items. |
| `gems.txt` | Gem-specific stats — what each gem does when socketed into a weapon / armor / shield at each quality level (Chipped → Perfect). |
| `runes.txt` | Runeword definitions — which runes form which runewords, in what order, and on what base item types. |
| `sets.txt` | Set definitions — the full set bonuses (partial and complete) for all item sets. |
| `setitems.txt` | Individual set item definitions — which item belongs to which set, and what fixed mods each piece has. |
| `uniqueitems.txt` | All unique item definitions — fixed mods, level requirements, rarity. Cross-references `weapons.txt` / `armor.txt` / `misc.txt` by item code. |

---

### ✨ AFFIXES — Magic, Rare & Crafted Mod Definitions
> Required to display what rolled on a magic or rare item.

| File | Purpose |
|------|---------|
| `magicprefix.txt` | All magic item prefixes — the mod granted, spawn level ranges, item type restrictions. e.g. "Ruby" adds fire resistance. |
| `magicsuffix.txt` | All magic item suffixes — same structure as prefixes. e.g. "of the Fox" adds dexterity. |
| `rareprefix.txt` | Name prefixes for rare items — purely cosmetic, used to generate the two-word rare item name. e.g. "Grim", "Vile". |
| `raresuffix.txt` | Name suffixes for rare items — second word of the rare name. e.g. "Gash", "Plague". |
| `uniqueprefix.txt` | Unique item name prefixes (used by the name generator). |
| `uniquesuffix.txt` | Unique item name suffixes (used by the name generator). |
| `uniqueappellation.txt` | Single-word unique name components. |
| `automagic.txt` | Auto-magic affixes — mods that spawn automatically on certain item types at certain levels (e.g. staffs always get +skills). |

---

### 📊 SUPPORT — Stats, Types & Display
> Required for correctly reading, calculating, and displaying item stats.

| File | Purpose |
|------|---------|
| `properties.txt` | Master property definitions — maps every property code (e.g. `res-fire`) to how it's calculated and displayed. Required to decode any mod on any item. |
| `propertygroups.txt` | Groups related properties together for display purposes. |
| `itemstatcost.txt` | Defines every stat in the game — how it's encoded in save files, min/max values, display format, icon. The backbone of the stat system. |
| `itemtypes.txt` | Item type hierarchy — defines parent/child relationships (e.g. `axe` is a child of `mele` which is a child of `weap`). Used to determine what can be socketed, what runewords apply, etc. |
| `itemuicategories.txt` | How items are grouped in the in-game UI filter tabs. |
| `armtype.txt` | Armour type classifications — Light / Medium / Heavy. Affects movement speed and class restrictions. |
| `bodylocs.txt` | Equipment slot definitions — Head, Torso, Gloves, Boots, Belt, Ring, Amulet, Weapon, Shield etc. |
| `inventory.txt` | Inventory and stash grid dimensions per character class. Defines how many rows/columns each storage area has. |
| `belts.txt` | Belt-specific definitions — how many rows of potion slots each belt type provides. |
| `colors.txt` | Item colour transform codes — used to tint item sprites for set/unique/crafted quality display. |
| `storepage.txt` | Stash page definitions — number of pages, page names. |
| `runeworduicategories.txt` | UI category labels for the runeword browser. |
| `lowqualityitems.txt` | Names for low quality item prefixes (Cracked, Damaged, etc.). |
| `qualityitems.txt` | Names for superior item prefixes (Superior). |

---

### 🧱 CUBE & CRAFTING
> Horadric Cube recipe definitions.

| File | Purpose |
|------|---------|
| `cubemain.txt` | All Horadric Cube recipes — inputs, outputs, conditions. Covers upgrade recipes, rerolling, rune upgrades, crafted items. |
| `cubemod.txt` | Modifier definitions for cube recipes — what mods get applied to crafted item outputs. |

---

### 👤 CHARACTER
> Class and stat data.

| File | Purpose |
|------|---------|
| `charstats.txt` | Base stats per class (Str/Dex/Vit/Ene), stat gain per level, starting equipment. Used to validate item requirements. |
| `playerclass.txt` | Class definitions — internal class codes and names. |
| `skills.txt` | All skill definitions for all classes. Large file — only needed if displaying skill bonuses on items. |
| `skilldesc.txt` | Skill tooltip descriptions and display formatting. Only needed for rendering +skills tooltip text. |

---

### 🗑️ CAN BE DELETED — Not needed for item/stash app

| File | Why |
|------|-----|
| `objects.txt` | World objects (barrels, chests, shrines) — not items |
| `automap.txt` | Minimap rendering data — not relevant |
| `levels.txt` | Zone/area definitions — not relevant |
| `levelgroups.txt` | Groups of levels — not relevant |
| `hireling.txt` | Mercenary stats and gear — not needed |
| `npc.txt` | NPC vendor definitions — not needed |
| `experience.txt` | XP tables per class — not needed |
| `gamble.txt` | Gambling cost tables — not needed |
| `treasureclassex.txt` | Drop tables — not needed for display |
| `books.txt` | Tome/scroll book definitions — misc only |
| `itemratio.txt` | Item drop ratio calculations — not needed |
| `errors.log` | Debug log — delete this |

---

## Sprite Files

Sprites are not included in this repo (Blizzard IP).  
Once extracted from D2R CASC (`data/hd/items/`), place them in:

```
sprites/
└── items/          ← invhax.png, invaxe.png etc.
                       Filenames match the `invfile` column in weapons/armor/misc.txt
```

The app will fall back to a placeholder grid block if a sprite is missing.

---

## Data Sources

All `.txt` files extracted from **Diablo II Resurrected** CASC archives using CascView.  
Original data © Blizzard Entertainment. This repository contains game data for the  
purpose of building a personal item tracking tool. No game assets are redistributed.
