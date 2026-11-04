# bobton — recording update

Adds 23 demonstrated features and their settings to the bobton menu, for 45 implemented modules. Includes bold text, menu lighting, grading and particles, the reference theme presets, and recorded menu sounds.

[Menu preview](bobton-menu.png) · [Cape settings](bobton-capes.png) · [Shader preview](bobton-effects.png)

## Install

Use Minecraft Java **1.21.11**, Java **21**, and Fabric Loader **0.18.4 or later**. Replace older bobton/donut-scout JARs with **only `bobton-v1-1.21.11.jar`** in the profile's `mods` folder. Fabric API is included inside this JAR. The ZIP contains the same JAR plus installation instructions.

**Right Shift** opens the menu. Left-click toggles a module; right-click opens settings and its keybind. Ordinary module keys toggle on each press. **Hold C** for Zoom; release to stop. Sword Inspect also has a separate action key in its settings.

## Included from the clips

- Waypoints, Block Highlight, ShaderFog, Hand Shader, Swing Animation, Sword Inspect, Freelook, Totem Counter, Particles, Jump Circles.
- Fast Place, Fake Player, Fastbreak, AutoTool, Auto Promo, Time Changer, Custom Crosshair, Capes Library, SkinProtect, Auto Pot.
- Fake Pay, Fake Stats, Name Protect. Fake Pay simulates `/pay player amount` locally while enabled. Fake Stats changes local sidebar rows and balance output. Name Protect is under Misc.

New DonutSMP work is limited to Fake Pay and Fake Stats. The existing Chunk Finder and Amethyst features remain. Other DonutSMP tools were deferred. The removed CHUNK FINDER statistics panel and RADAR stay removed.

## Reference limits

Original shader programs and full-resolution cape textures were not supplied. The effects and cape artwork are reconstructed from the recordings, so they are not pixel-identical originals. Some slider bounds/defaults were never shown. Fake Pay's expanded settings and exact message template were not demonstrated.

## Validation

129 automated checks pass. The production JAR was checked in Minecraft 1.21.11 with its embedded Fabric API. No external Fabric API or development classes are required. The supplied reference recordings, audio and transcripts are not included in this download.
