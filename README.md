# 🏚️ CRYPTDEPTH — Neon Dungeon Crawler

> *A roguelike dungeon crawler built entirely in vanilla HTML, CSS & JavaScript. No frameworks. No dependencies. Just one file.*

---

## 🎮 Play It

Open `dungeon-crawler.html` in any modern browser — that's it. No install, no build step, no internet required after download.

**Works on:**
- 🖥️ Desktop (Chrome, Firefox, Edge, Safari)
- 📱 Mobile & Tablet (touch D-pad included)
- 💻 Laptop (keyboard or trackpad click)

---

## 📸 Screenshots

| Title Screen | Gameplay | Game Over |
|---|---|---|
| Tabbed menu with Play, About & Controls | Neon pixel-art dungeon with fog of war | Stats summary with floor & kill count |

---

## ✨ Features

- ⚔️ **Turn-based roguelike combat** — bump into enemies to attack
- 🗺️ **Procedurally generated maps** every run using BSP room algorithm
- 🌫️ **Fog of war** with field-of-view lighting — explore to reveal the map
- 🧛 **4 unique enemies** — each hand-drawn with pixel-art sprites
- 🎒 **Loot system** — potions, weapons, and armor scattered across rooms
- ⬆️ **XP & leveling** — gain power as you descend deeper
- 🔊 **Web Audio sound effects** — footsteps, combat hits, level-up fanfares, death
- 🕹️ **On-screen D-pad** — works on mobile and desktop mouse
- 📖 **About tab** — in-game guide so new players learn before they play
- 💀 **Permadeath** — every run is a fresh start

---

## 🕹️ How To Play

### The Goal
Descend as deep as possible through the dungeon. Each floor gets harder — enemies hit heavier and have more HP — but you grow stronger too. There is no final boss. Survive as long as you can.

### Movement
Move your warrior one tile at a time. The dungeon is **turn-based** — nothing moves until you do. Think before you step!

| Platform | How to Move |
|---|---|
| ⌨️ Keyboard | `W A S D` or `↑ ← ↓ →` Arrow Keys |
| 🖱️ Mouse | Click the on-screen D-pad buttons |
| 📱 Touch | Tap or hold the on-screen D-pad |

### Combat
Walk **into** an enemy to attack them. No separate attack button. The game automatically calculates damage:

```
Damage dealt = Your ATK − Enemy DEF + random(−1 to +2)
```

After you attack, the enemy gets their turn. If an enemy is adjacent to you at the start of your turn, they attack first. Use hallways to fight enemies one at a time!

### Items
Walk **into** items to automatically pick them up.

| Item | Effect |
|---|---|
| ⚡ Charge Pack (Potion) | Restores 8–15 HP |
| ⚔️ Blade (Weapon) | Permanently increases ATK |
| 🛡️ Plate (Armor) | Permanently increases DEF — reduces damage taken |
| 🪜 Stairs | Walk into them to descend to the next floor |

### Stats Explained

| Stat | What It Does |
|---|---|
| **HP** | Health Points. Reach 0 and you die. |
| **ATK** | Attack power. Higher = more damage to enemies. |
| **DEF** | Defense. Reduces damage you take from enemies. |
| **XP** | Experience points. Fill the bar to level up. |
| **LVL** | Your current level. Increases ATK and max HP. |
| **KILLS** | Total enemies defeated this run. |
| **FLOOR** | How deep you've descended. |

### Leveling Up
Kill enemies to earn XP. When your XP bar fills:
- 🔺 Level increases by 1
- ❤️ Max HP +8 (and current HP healed by 8)
- ⚔️ ATK +2
- XP bar resets, next level requires ~60% more XP

---

## 👹 Enemy Guide

### 👻 Wraith
- **Appearance:** Floors 1+
- **Stats:** Low HP, Low DEF, Moderate ATK
- **Behavior:** Chases you as soon as you're in sight
- **Strategy:** Easy to kill early on. Don't ignore them in groups.

### 🦂 Crawler
- **Appearance:** Floors 1+
- **Stats:** High HP, Some DEF, Strong ATK
- **Behavior:** Aggressive — charges directly toward you
- **Strategy:** Take hits to deal with them early. Grab armor first if possible.

### 🪨 Golem
- **Appearance:** Floors 2+
- **Stats:** Massive HP, High DEF, Very High ATK
- **Behavior:** Slow but unstoppable once in range
- **Strategy:** Lure into a hallway. Hit and retreat if you're low HP.

### 🧛 Vampire
- **Appearance:** Floors 3+
- **Stats:** Moderate HP, Low DEF, Extreme ATK
- **Behavior:** Fast predator — closes distance quickly
- **Strategy:** Most dangerous enemy. Prioritize killing them before they surround you.

---

## 🗺️ Map System

Maps are generated fresh every floor using a **Binary Space Partitioning (BSP)** approach:

1. A grid of 38×28 tiles is filled with walls
2. Up to 120 room placement attempts are made
3. Rooms that don't overlap are carved out
4. Each new room is connected to the previous one via a tunnel
5. Enemies, loot, and stairs are randomly placed in rooms

**Fog of War:** Only tiles within your line-of-sight are fully visible. Visited tiles appear dimmed. Unvisited tiles are completely black. Enemies only move when they're in your field of view.

---

## 🔊 Sound Effects

All sounds are generated in real-time using the **Web Audio API** — no audio files needed.

| Sound | Trigger |
|---|---|
| 👣 Footstep | Moving to a new tile |
| 🧱 Bump | Walking into a wall |
| ⚔️ Hit | Attacking an enemy |
| 💀 Kill | Enemy defeated |
| 😖 Hurt | Taking damage |
| ⚡ Loot | Picking up an item |
| ⬆️ Level Up | 4-note ascending fanfare |
| 🪜 Stairs | Descending to next floor |
| 💀 Death | Dramatic crash when you die |

---

## 🏗️ Technical Details

### Stack
- **Pure vanilla HTML/CSS/JavaScript** — zero dependencies
- **Canvas API** for all rendering (tiles, sprites, HP bars)
- **Web Audio API** for procedurally generated sound effects
- **Single file** — entire game is `dungeon-crawler.html`

### File Structure
```
dungeon-crawler.html
├── <style>        — All CSS (UI, D-pad, title tabs, game over screen)
├── <body>         — HTML structure (title, game, game-over screens)
└── <script>       — All game logic
    ├── Config     — Tile size, grid dimensions, color palette
    ├── Map Gen    — BSP procedural room + tunnel generation
    ├── FOV        — Ray-casting field of view
    ├── Entities   — Player, enemies, items, stairs
    ├── Sprites    — Pixel-art canvas drawing functions
    ├── Combat     — Attack resolution, enemy AI, turn system
    ├── UI         — Stats bar, message log, HP bar
    ├── Controls   — Keyboard + D-pad + touch input
    └── SFX        — Web Audio synthesis engine
```

### Sprite Rendering
All sprites are drawn programmatically on the `<canvas>` element using `fillRect`, `beginPath`, `arc`, and `bezierCurveTo` — no image assets. Each enemy has a unique hand-crafted drawing function:

- `drawPlayer()` — Armored warrior with glowing visor, cape, sword
- `drawWraith()` — Flowing ghostly body with ragged edges
- `drawCrawler()` — Segmented insect with mandibles and legs
- `drawGolem()` — Stone giant with glowing lava cracks
- `drawVampire()` — Cloaked figure with fangs and red eyes

---

## 🚀 Getting Started (Local)

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/cryptdepth.git

# Navigate into it
cd cryptdepth

# Just open the file — no server needed!
open dungeon-crawler.html

# Or serve it locally if you prefer
npx serve .
```

---

## 💡 Survival Tips

1. **Hallways are your friend** — enemies can only come at you one at a time in corridors
2. **Don't rush stairs** — explore every room for loot before descending
3. **Watch enemy HP bars** — green → orange → red tells you when they're nearly dead
4. **Potions are precious** — save them for emergencies, not minor scratches
5. **DEF matters more than ATK** — reducing incoming damage compounds over time
6. **Retreat is valid** — the turn-based system lets you think and reposition
7. **Level up before descending** — fight everything on the floor before taking stairs

---

## 🔮 Possible Future Features

- [ ] Boss enemies on every 5th floor
- [ ] More item types (scrolls, rings, boots)
- [ ] Minimap in corner
- [ ] High score leaderboard (localStorage)
- [ ] Multiple character classes (Mage, Rogue, Paladin)
- [ ] Traps and environmental hazards
- [ ] Ranged enemies
- [ ] Shop rooms between floors

---

## 📄 License

MIT License — free to use, modify, and distribute. Attribution appreciated but not required.

---

## 🙏 Credits

Built with ❤️ using:
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [Google Fonts](https://fonts.google.com) — Share Tech Mono, Bebas Neue, Orbitron

---

*Enter the crypt. Kill everything. Don't die.*
