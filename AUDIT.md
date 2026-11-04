# bobton v1 — module audit and Scaffold update

Minecraft Java 1.21.11 / Fabric. This update reviews all 46 registered modules, repairs the issues below, improves menu readability and scrolling, and adds Scaffold under Misc.

## Verification scope

| Check | Result |
| --- | --- |
| Source/API review | All 46 module runtimes, exposed settings, input and cleanup paths reviewed against the current 1.21.11 implementation |
| Automated regression suite | 170 tests passed; 0 failures, 0 errors, 0 skipped |
| Production client | Bundled release JAR launched in Minecraft 1.21.11 with embedded Fabric API |
| Right-click module settings | All 46 pages opened in the production client |
| Enable/disable controls | All 45 ordinary module controls toggled on and off, with saved state verified; Zoom uses its separate hold action |
| Scaffold | Straight bridge, diagonal bridge, empty-block Safe Walk and original-slot restoration passed in an offline survival fixture |
| Action-key integration | A quick E tap added exactly one waypoint without opening vanilla inventory; a quick Mouse2 tap with a held sword visibly triggered Sword Inspect |
| Auto Totem | With a carried totem and an empty offhand, normal gameplay automation filled the offhand |
| Auto Inventory Totem | With Auto Totem disabled, the offhand stayed empty while inventory was closed and filled after opening inventory; authoritative predicates confirmed both states |
| Auto XP | With a 10-tick delay, holding Use for 3.05 seconds consumed 7 of 64 bottles; the shared vanilla/module cooldown limited the repeated throws |
| Zoom | Holding C changed the target width from 440 to 1762 pixels (about 4×); releasing C restored 440 pixels in the native 4K captures |
| Scaffold settings | Dropdown changed Automatic → Hold Use; delay slider changed 4 → 8; Reset restored Automatic/4; keybind capture with Backspace separately cleared the binding to Unbound |
| Final menu | Native 3840×2160 inspection confirmed readable Inter Bold labels without overlap; wheel input scrolled the Settings page to its Sounds controls |
| Remote servers / anti-cheat | Not tested; no claim of undetectability |

A source review and a settings-page check do not exercise every setting combination in gameplay. Native functional checks were targeted at Scaffold and selected integration paths. Server rules, permissions, latency, graphics drivers and other mods can change results.

## Reviewed module inventory

| Section | Count | Modules |
| --- | ---: | --- |
| Combat | 14 | Auto Inventory Totem, Anchor Macro, Auto Crystal, Auto Totem, Crystal Optimizer, Aim Assist, Silent Aim, Reach, AutoTrap, AutoCart, AutoClicker, Trigger Bot, Safe Anchor, Auto XP |
| Visuals | 16 | Name Tags, Health Box, Tracers, Target, Storage ESP, Zoom, Waypoints, Block Highlight, ShaderFog, Hand Shader, Swing Animation, Sword Inspect, Freelook, Totem Counter, Particles, Jump Circles |
| Misc | 12 | Time Changer, Fast Place, Fake Player, Fastbreak, AutoTool, Auto Promo, Custom Crosshair, Capes Library, SkinProtect, Auto Pot, Scaffold, Name Protect |
| Donut SMP | 4 | Existing Chunk Finder and Amethyst Bypass; Fake Pay and Fake Stats |

No additional Donut SMP modules were added. The removed CHUNK FINDER statistics panel and RADAR remain removed.

## Repairs included

- **Combat and inventory:** gameplay focus/overlay guards, stale-world target rejection, temporary reach cleanup and creative-range shortening. Crystal Optimizer now uses its CPS setting and limits attacks across target switches. Vanilla held-use and module repeats share a cooldown, avoiding duplicate initial requests and honoring configured slow delays. Crystal placement follows the actual 1.21.11 air/entity-clearance rule. AutoTrap uses its displayed range. Unrelated settings are hidden; Silent Aim exposes the timing preferences it actually uses.
- **Misc automation:** Fast Place tracks the hand/item whose interaction was accepted. AutoTool considers harvest tier and Efficiency and synchronizes/restores the selected slot. Auto Pot aims before vanilla consumption and restores camera/slot. Fake Player interactions stay local. Auto Promo no longer treats a healthy unloaded entity as a kill, and its text is protocol-safe. Cape visibility and malformed skin-shuffle indexes are corrected.
- **World overlays and scanners:** Chunk Finder draws separate purple geode bounds when requested, invalidates replaced chunks and starts fresh after disable/resume. Waypoint actions capture brief keyboard/mouse presses and consume conflicting vanilla shortcuts during valid gameplay. Name/waypoint plates fit the real UI font; target names reserve distance space. Storage ESP retains signed-height support and scans only loaded block entities.
- **Rendering:** zero-opacity ShaderFog restores vanilla fog color and distance, and active fog survives HUD hiding. Hand Shader uses a real spatial trail blur, releases unused snapshots and rejects stale captures. Swing Animation off restores vanilla swing duration and respects hand side. Sword Inspect captures quick taps, resets input on context changes and normalizes overlapping pose phases. Particle duplicate throttling no longer starves sustained hit bursts. Freelook and rendering helpers have consistent gameplay cleanup gates.
- **Identity and controls:** Name Protect handles aliases that prefix the original name, legacy-gradient names and incomplete formatting without losing subsequent formatting. Fake Stats line indexes match the 15 visible sidebar rows. Ordinary module keys toggle per physical press; Zoom remains hold-to-use. Menu priority and focus/world-change cancellation are preserved.

Regression tests cover these pure calculations and state machines, including Scaffold geometry, shared combat cooldowns, tool choice, text boundaries, waypoint/inspect actions, scanner bounds, particles and menu motion.

## Menu clarity

Menu typography uses the bundled Inter Bold font with Unicode fallback and fourfold glyph sampling. Normal menu label origins align to physical framebuffer pixels, text shadows are removed and secondary labels are brighter. Typed input uses the same font as other menu text. The final native menu was inspected at 3840×2160 with clean, readable labels and no overlap.

Wheel scrolling now eases by elapsed time. Rendered widgets and hitboxes use the same position; clicking or dragging stops inertia. Viewport edges have short fades. Existing 16 themes, B/BOBTON artwork, sidebar icons, sound packs and preferences are retained. Output resolution and frame rate depend on the display and hardware.

## Scaffold behavior and native evidence

Scaffold starts disabled. Its right-click settings provide Automatic/Hold Use placement, Auto-select Blocks, Safe Walk, Keep Level, Swing Hand, delay (2–10 ticks), reach (2–4.5 blocks, capped to vanilla reach) and lookahead.

Placement uses ordinary block interaction with a loaded, visible support face. It checks build height, world border, replacement state, placement permissions, entity collision and reach. Auto-selection excludes falling blocks, TNT, containers, workstations and unsuitable shapes. It restores the original hotbar slot when its temporary selection ends and respects a manual slot change. GUI/focus loss, incompatible movement, mining and existing item use stop placement. Safe Walk uses vanilla local ledge clipping.

Offline survival checks at negative height produced these results:

- **Straight:** crossed an air trench and placed 17 stone blocks. Authoritative world checks confirmed stone at three separated bridge positions. The player stayed at the original feet height and the original sword selection returned.
- **Diagonal:** crossed the trench diagonally using 15 stone blocks, stayed at the original feet height and restored the sword selection.
- **No blocks:** Safe Walk stopped the player at the supported edge without falling.

Scaffold does not make unsupported floating placements and stops when valid support, usable blocks or permissions are unavailable. Remote server placement/protection/anti-cheat decisions remain authoritative. No anti-cheat plugin was tested and undetectability cannot be guaranteed.

## Practical limits

World overlays and scanners can only inspect information already sent to the client. Chunk Finder and Amethyst Bypass identify heuristic evidence, not confirmed bases; natural formations can match. Dense scenes may reach bounded scan/marker budgets. Client-side Reach/Fastbreak predictions remain subject to server validation. Safe Anchor uses a heuristic safety filter rather than full explosion damage prediction. Fake Pay/Fake Stats are local display simulations. Reconstructed visual assets and effects do not establish pixel-identical original shaders or artwork.
