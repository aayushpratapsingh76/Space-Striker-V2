# Space Striker 🚀

An intense, arcade-style pixel-art vertical space shooter built entirely as a single-file web application. 

🔗 **Play Live:** [https://space-striker-v2.vercel.app/](https://space-striker-v2.vercel.app/)

---

## 🎮 Game Overview

Space Striker is a lightweight, zero-dependency, pure vanilla HTML5 canvas game. It is designed to run completely offline, optimized specifically for mobile environments (Chrome on Android) with precise touch tracking, while scaling to desktop screens.

### Technical Architecture
* **Single File Blueprint:** Built with pure HTML5, vanilla JavaScript (Canvas 2D API), and the Web Audio API. No frameworks, no external asset files, and no CDNs.
* **Performance Optimization:** Utilizes Object Pooling for resource-heavy operations (like a fixed 80-particle pool) and pre-allocated arrays to eliminate memory churn and ensure a consistent 60 FPS.
* **Deterministic Game Loop:** Powered by a delta-time normalized `requestAnimationFrame` loop ensuring uniform movement speed across varying hardware refresh rates.

---

## ⚡ Core Features

### 🛠️ Sprite & Visual System
* **Procedural Grid Graphics:** All entities are drawn using an inline 2D color grid array system rendering raw pixel rectangles. 
    * *Standard Ships:* Formed with a 4px rendering unit.
    * *Boss Class Entities:* Styled with an imposing 5px rendering unit.
* **Dynamic Parallax Background:** Features a 3-layer parallax scrolling starfield rolling downward at independent speeds (0.5px, 1.0px, and 1.8px per frame) over a `#0a0012` backdrop with a static, glowing celestial moon.

### 🕹️ Gameplay Mechanics & Scaling
* **Relative Drag Controls:** Smooth 1-to-1 movement tracking utilizing linear interpolation (`lerp` coefficient of `0.15`) bounded by a clean 20px viewport safety margin. Includes dedicated bounding rect matrix conversions to map touch coordinates accurately on mobile viewports.
* **Dynamic Upgrade Tree:** Auto-firing mechanics with variable cadence thresholds based on upgrade ranks (400ms down to a blazing 110ms) alongside scalable projectile spreads (1 bullet straight up, 2 bullets at ±15°, or 3 bullets at -25°, 0°, +25°).
* **Strategic Power-up Drops:** Decimated enemies drop temporary boosts including a protective **Absorptive Shield Ring**, a 6-second continuous **Pink Laser Beam**, and **Instant Health Recovery Hearts**.

### 🎵 Audio Synthesis (Web Audio API)
Every sound effect is generated mathematically in real-time using native oscillators and gain nodes. Includes a built-in 60ms structural cooldown preventing audio clipping during chaotic combat moments.

---

## ⚔️ Enemy Spacecraft Roster

The game features 10 specialized standard enemy ship types. All standard unit munitions adhere to a strict tactical rule: **projectiles fire straight down only (dy > 0, dx = 0)**.

| Ship Type | Base HP | XP Value | Unique Movement & Combat Behavior |
| :--- | :---: | :---: | :--- |
| **1. Scout** | 1 | 30 | Rapid-deployment interceptor. Straight vertical descent. |
| **2. Zigzagger** | 2 | 50 | Evasive maneuvering profile utilizing a clean sine wave trajectory. |
| **3. Armored** | 3 | 70 | Heavy hull plating. Drops heavy vertical fire down-lane. |
| **4. Sniper Drone** | 2 | 70 | Enters the arena, hovers statically at a distance, and locks firing vectors. |
| **5. Berserker** | 1 | 50 | High-velocity kamikaze craft designed to directly ram the player. |
| **6. Shielder** | 2 | 70 | Employs an energy barrier that completely absorbs the first incoming projectile. |
| **7. Splitter** | 2 | 120 | Mitigates damage by dividing into 2 functional Scout ships upon destruction. |
| **8. Bomber** | 3 | 120 | Drops high-explosive ordnance straight down along its path. |
| **9. Phantom** | 1 | 150 | Ultra-fast phase shift movement diagonally across the screen. Does not fire. |
| **10. Swarm Caller** | 3 | 200 | Stationary support carrier. Periodically spawns fresh Scout units into play. |

---

## 👑 The Boss Gauntlet & Progression

Combat progression is governed by strict scoring thresholds. When a milestone is crossed, an immediate **XP Lockout** triggers: normal spawning stops, active enemies are purged, the upgrade selection card interface screen opens, and a Boss Warning sequence sequence triggers.

> ⚠️ **XP Lockout Rule:** During a boss encounter, *all* kills (including any spawned minions) yield exactly **0 XP/Points**. Normal point generation resumes only after the boss is completely vaporized.

### Boss Roster Matrix
1. **INVADER KING** (1,000 pts | 40 HP) — *Tier: Easy*
2. **VOID CRUISER** (2,200 pts | 70 HP) — *Tier: Easy-Medium*
3. **DEATH STAR** (3,600 pts | 100 HP) — *Tier: Medium*
4. **PLASMA TITAN** (5,200 pts | 140 HP) — *Tier: Medium-Hard*
5. **SHADOW OVERLORD** (7,000 pts | 180 HP) — *Tier: Hard (2 Combat Phases)*
6. **CRYSTAL FORTRESS** (9,000 pts | 220 HP) — *Tier: Hard*
7. **NEBULA WARLORD** (11,200 pts | 260 HP) — *Tier: Hard*
8. **IRON COLOSSUS** (13,500 pts | 310 HP) — *Tier: Very Hard*
9. **VOID EMPEROR** (16,000 pts | 370 HP) — *Tier: Very Hard*
10. **OMEGA DESTROYER** (19,000 pts | 450 HP) — *Tier: Elite Final Encounter*

Defeating a boss unlocks the next standard enemy ship type into the normal spawning pool sequence.

---

## ⚙️ Difficulty Adjustments

Selectable from the title interface, difficulty dynamically scales structural coefficients across the engine runtime:

* **Easy Mode:** Enemy Speed × 0.70 | Spawn Intervals: 1600ms | Boss HP × 0.80 | Drop Rate: 28%
* **Medium Mode:** Enemy Speed × 1.00 | Spawn Intervals: 1200ms | Boss HP × 1.00 | Drop Rate: 16%
* **Hard Mode:** Enemy Speed × 1.35 | Spawn Intervals: 850ms | Boss HP × 1.30 | Drop Rate: 8%

---

## 📐 Architectural Canvas Zones

To prevent visual collisions and UI rendering overlapping, the 360x640px canvas space is strictly partitioned by vertical coordinate boundaries:

* **Zone 1 (y = 0 to y = 40):** Scoreboard metrics (Score layout Left, Current Lives Right).
* **Zone 2 (y = 44 to y = 80):** Interactive system Pause Button `[⏸]` bounding zone.
* **Zone 3 (y = 80 to y = 94):** Upgrade Status display (Active Weapon Array and Speed indexes).
* **Zone 4 (y = 94 to y = 104):** Active Power-Up countdown timer depletion bar.
* **Zone 5 (y = 104 to y = 120):** Boss Vitality / Health point telemetry tracker.
* **Gameplay Core (y = 120 to y = 640):** Exclusive movement and rendering space for active entities.

---

## 🚀 How to Run Locally

Because Space Striker is contained entirely within a single file, it requires no compilation, local server environments, or build pipelines.

1. Clone or download the repository.
2. Open the `index.html` file in any modern desktop or mobile web browser.
