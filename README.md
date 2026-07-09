# Voxel Craft

A Minecraft-style survival game in **a single HTML file** — no build, no install, no server. Open `index.html` in Chrome and play.

## Play

- Double-click `index.html`, or
- If hosted with GitHub Pages: open the site URL and click to play.

## Features

- **Infinite streaming world** (100,000 × 100,000 × 400) with hills, mountains, caves, and trees, generated from seeded noise
- **Mining & building** — hold left-click to mine (per-block hardness, crack animation), right-click to place; sneak at an edge to bridge
- **Ores & smelting** — coal, copper, iron, gold, diamond; metal ores smelt into ingots in a real working furnace (input / fuel / output, live progress)
- **Full crafting** — 2×2 inventory grid, craftable crafting table with 3×3 grid, and the complete vanilla recipe book: tools in 6 material tiers, armor in 4 tiers, torches, ladders, chest, furnace, bed, boat, rails, shield, and many more
- **Armor system** — vanilla armor points and the 1.9 damage-reduction formula; diamond has armor toughness; 🛡 bar in the HUD
- **Survival** — health, hunger, exhaustion, regeneration, starvation, fall damage, eating; death drops your whole inventory where you died
- **Animals** — cows, sheep, chickens, pigs, horses with faithful models; they flee when hit and drop loot (never, EVER punch a cow)
- **Hostile mobs** — creepers (visible 4-second fuse, big crater), zombies and spiders (3-hit melee kills), skeletons (3-hit bow shots); they spawn at night or in caves and **burn with real flames** in sunlight
- **Day/night cycle** (10 minutes) — craft a bed and sleep through the night to skip it and set your spawn
- **Chests** with persistent contents, a starter loot chest near spawn, torches that glow, climbable ladders

## Controls

| Key | Action |
|---|---|
| WASD + mouse | Move / look |
| R + W | Sprint |
| Shift | Sneak (edge-safe, enables bridging) |
| Space | Jump |
| Hold left click | Mine / attack |
| Right click | Place block / eat / use table, furnace, chest, bed |
| E | Inventory + crafting |
| 1–9, 0 | Hotbar slots |
| F | Swap held item with the left hand |

## Tech

Three.js (CDN import map), merged chunk meshes on a single texture atlas, voxel DDA raycasting, procedural pixel-art textures painted on canvas — all in one file.
