# bobton v1 — Hypixel section and Clutch

**Hypixel** now appears directly below **Misc**. Scaffold has moved into it, and the new **Clutch** module sits alongside it. Scaffold keeps the same saved module ID, options, enabled state and keybind; moving its card does not reset an existing configuration. The catalog now contains 47 modules.

Right Shift opens the menu. Left-click a card to enable or disable its module; right-click for its settings and activation key. Both modules start disabled in a fresh configuration. Ordinary activation keys toggle per press; Zoom continues to work while its key is held.

## Clutch behavior and settings

Clutch predicts a void or sufficiently deep drop and tries to build a reachable landing pad. It remembers the takeoff floor and can place short supporting connections from an existing ledge toward the predicted landing point. It needs usable solid blocks in the hotbar or offhand and a loaded, visible support face within normal placement reach. It cannot create floating support or guarantee recovery after the player has moved beyond reachable blocks.

| Setting | Default | Range or behavior |
| --- | --- | --- |
| Auto-select Blocks | On | Searches the hotbar when neither held hand supplies a suitable block |
| Look at Placement | On | Visibly turns the camera toward the placement face |
| Swing Hand | On | Swings after an accepted interaction |
| Minimum Drop | 6 blocks | 3–16 blocks |
| Prediction Ticks | 12 | 2–12 ticks |
| Placement Delay | 2 ticks | 1–6 ticks |
| Placement Reach | 4.25 blocks | 2–4.5 blocks, capped to the player's normal reach |

Placement checks support visibility, block/entity collision, loaded chunks, build height, border and permissions. Temporary hotbar selection restores the original item and respects a manual selection. Clutch takes placement ownership from Scaffold during a catch. Mining first releases Clutch's temporary selection so AutoTool can restore the correct original item. GUI/focus changes, item use, swimming, water contact and incompatible movement suspend the catch.

## Verification

The automated suite passed **178 tests**, with **0 failures, errors or skipped tests**.

The catch planner also passed an offline Minecraft 1.21.11 survival fixture: ordinary `mob_attack` damage knocked the player east from a one-block-thick platform, with **no forward input** and normal knockback resistance **0**. Maximum health was raised to 100 for fixture safety; movement and knockback were unchanged.

Saved server NBT confirmed the player grounded at **(402.488568, 81, 100.5)**, with the whole footprint beyond the original platform's east edge at X=401. The world contained exactly two new stone blocks at **(401, 80, 100)** and **(402, 80, 100)**. Inventory held **62 of the initial 64 stone**, and the original diamond sword was selected again. Playerdata and the level's saved player state agreed.

The final JAR also passed the offhand version of the same knockback fixture: two offhand stone were placed, 62 remained, the player landed at the same position and the sword stayed selected. The activation key toggled Clutch on each press and stayed active after release.

Negative fixtures confirmed that no-block inventory caused no placements, and an unsupported fall outside reachable faces consumed none of the 64 available blocks and created no floating support. Negative fixtures used increased maximum health for observation; rescue was not claimed for those falls.

With Scaffold and Clutch enabled together, the same knockback test again landed at the same grounded position using two blocks and restored the sword. Both moved/new right-click settings pages and press-to-toggle activation were checked in the production client.

These results establish the tested local behavior. Live Hypixel and its anti-cheat have not been tested; undetectability is not guaranteed.
