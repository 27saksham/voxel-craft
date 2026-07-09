# Voxel Craft — Implementation Spec

Derived from the Minecraft Wiki's *Tutorial: Beginner's Guide*
(https://minecraft.wiki/w/Tutorial:Beginner%27s_guide) and the official
minecraft.net beginner tips. This document summarizes every system the guide
covers, translated into concrete, implementable rules for our single-file
Three.js voxel game (`index.html`). Each item is tagged:

- **[REQUIRED]** — must exist in this build; the reviewer checks these.
- **[DEFERRED]** — captured for completeness, explicitly out of scope for this
  build (needs crafting UI / entity AI / world persistence that comes later).
  The reviewer must NOT penalize deferred items.

---

## 1. World & Terrain

The guide's world is made of blocks; biomes vary resources; wood comes from
trees; stone/ores are underground.

- **[REQUIRED] Layered flat terrain** (32×32): y=0 indestructible bedrock,
  y=1–2 stone, y=3 dirt, y=4 grass surface. Single source of truth is the
  voxel `Map`; meshes derive from it with exposed-face culling.
- **[REQUIRED] Trees**: several trees scattered on the surface (not blocking
  spawn), each a 4–5 block **log** trunk with a **leaves** canopy
  (3×3 blob + cap). Logs and leaves are their own block types with their own
  pixel-art textures (log = bark sides + ring top; leaves = dark green noise).
- **[REQUIRED] Coal ore**: a fraction (~4–8%) of underground stone cells
  generate as coal ore blocks (stone texture with black chunks), giving the
  guide's "mine coal veins" progression a first target.
- **[DEFERRED] Biomes** (plains/forest/desert/swamp variations), villages,
  caves, infinite/chunked terrain.

## 2. Blocks, Mining & Hardness

The guide: different blocks take different effort; tools have tiers; stone
needs a pickaxe.

- **[REQUIRED] Hold-to-mine with per-block hardness** (seconds of held
  left-click): grass/dirt/sand ≈ 0.4s, leaves ≈ 0.2s, wood/log ≈ 0.6s,
  stone ≈ 1.2s, coal ore ≈ 1.5s. Bedrock: hardness ∞, never breaks.
- **[REQUIRED] Crack overlay**: progressive crack visual on the block being
  mined; progress resets when the target changes or the button is released.
- **[REQUIRED] Block drops**: finished mining spawns a collectible dropped
  item (small spinning cube with the block's texture) that falls to rest and
  can be picked up within ~1 block.
- **[DEFERRED] Tool tiers** (wood/stone/iron pickaxes gating what's minable
  and mining speed) — requires crafting (below). For now the bare hand mines
  everything except bedrock, at the hardness table above.

## 3. Inventory & Items

- **[REQUIRED] Hotbar inventory**: 10 right-hand slots + 1 left-hand slot;
  stacks of `{type, count}` capped at 64; number keys 1–9,0 select right-hand
  slots; **F** swaps the selected right-hand stack with the left hand
  (vanilla offhand swap — the only way items reach the left hand); pickups
  auto-stack into the right hand only; placing/eating consume the selected
  right-hand stack, falling back to the left hand when it is empty; UI shows
  texture thumbnails + counts and glows whichever hand is actually in use.
- **[REQUIRED] Non-placeable food item**: the **apple** exists as an item
  (droppable, collectible, shown in hand) but cannot be placed as a block.
- **[REQUIRED] Death drops**: on death, the entire inventory (both hands) is
  dropped as item stacks at the death location (guide: "drops all inventory
  items at death location"). Dying in the void destroys the inventory instead.
- **[DEFERRED] Full inventory screen (E), crafting grids (2×2 / 3×3),
  furnace + smelting, chests.**

## 4. Health, Hunger & Survival

Guide rules implemented as stated:

- **[REQUIRED] Health = 20 HP**, displayed as 10 hearts (2 HP each, half
  hearts shown) above the hotbar.
- **[REQUIRED] Hunger = 20 points**, displayed as 10 drumsticks (2 pts each)
  above the hotbar.
- **[REQUIRED] Hunger drain by activity**: exhaustion accumulates faster when
  sprinting than walking than idling; each 4 exhaustion removes 1 hunger pt.
- **[REQUIRED] Hunger ≤ 6 disables sprinting** (guide: "below 6 prevents
  sprinting").
- **[REQUIRED] Hunger = 0 causes starvation damage** (1 HP every ~4s, but
  never below 1 HP — normal-difficulty rule).
- **[REQUIRED] Hunger ≥ 18 regenerates health** (1 HP every ~4s, costing a
  little extra exhaustion).
- **[REQUIRED] Fall damage**: falls beyond ~3 blocks deal damage proportional
  to the extra distance.
- **[REQUIRED] Death & respawn**: at 0 HP the player drops inventory (see §3),
  respawns at world spawn with full health/hunger.
- **[REQUIRED] Eating**: right-clicking while holding food (apple) consumes
  one and restores 4 hunger points (capped at 20); does nothing when full.
- **[REQUIRED] Food source**: mining leaves has a chance (~1 in 5) to drop an
  apple instead of the leaves block.
- **[DEFERRED] Animals as food** (cows/pigs/chickens), cooking raw meat in a
  furnace, other foods.

## 5. Movement

- **[REQUIRED] Walk** is deliberately moderate (~3.8 blocks/s).
- **[REQUIRED] Sprint**: hold **R** with **W** (~6.4 blocks/s), blocked when
  hunger ≤ 6 or while crouching.
- **[REQUIRED] Crouch/sneak**: hold **Shift** — slow (~1.7 blocks/s), lowers
  the camera slightly, and **cannot walk off block edges** (sneak edge guard).
- **[REQUIRED] Jump + gravity + AABB collision**, axis-separated with
  nearest-face resolution (no jitter/teleporting), void respawn.

## 6. Day/Night Cycle & Light

Guide: 20-minute cycle, night is dangerous, light matters.

- **[REQUIRED] Day/night cycle**, time-scaled for playability (~4 minutes per
  full cycle, documented in code): the sun light orbits, its intensity and
  the ambient light dim at night, and the sky + fog colors blend from day
  blue to a dark night blue. The cycle runs continuously while playing.
- **[DEFERRED] Beds** (skip night, set spawn), torches with per-block light
  levels, mob spawning in darkness.

## 7. Combat & Mobs

- **[DEFERRED] Hostile mobs at night, animals, left-click attack combat, mob
  drops, iron golems/villagers.** (Requires entity AI; the health system in
  §4 is the foundation for it.)

## 8. First-Person Presentation

- **[REQUIRED] Viewmodel**: held block rendered bottom-right on a cleared
  depth buffer (never occluded by the world); bare peach arm when the active
  slot is empty; walk bob; mining punch motion.
- **[REQUIRED] HUD**: crosshair, targeted-block wireframe highlight, controls
  legend (including sprint/crouch/eat), hearts + hunger rows, hotbar +
  left-hand slot.

---

## Reviewer instructions

Check every **[REQUIRED]** line against `index.html`. Deferred items must not
lower the rating. Rate 0–10 for spec adherence + code coherence, list concrete
defects/advice, and say the exact phrase **"no more edits needed"** if the
build fully satisfies the spec.
