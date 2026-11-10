# bobton v1 — interaction hardening

Minecraft Java **1.21.11**, Java **21**, Fabric Loader **0.18.4 or later**. Fabric API is included in the JAR.

Replace the old bobton/donut-scout JAR in the chosen Fabric profile's `mods` folder with `bobton-v1-1.21.11.jar`. Keep one client JAR installed. The ZIP contains the same JAR and installation instructions.

**Right Shift** opens the menu. Left-click ordinary modules to toggle; right-click for settings and a custom key. Zoom activates while its key is held (default **C**). **Hypixel**, directly below Misc, still contains Scaffold and Clutch; saved settings and bindings are retained.

This update coordinates automated requests, corrects potion and held-use pacing, reduces redundant rotation traffic, checks placement geometry and caps automated attacks to native ranges. Fast Place's existing preference now reads **Vanilla Use Delay** and defaults to the normal four-tick held-use minimum. See [NOTES.md](NOTES.md) for the changes and targeted network verification. [Settings preview](bobton-interactions.png).

**208 automated tests passed**, plus a production-client test against a private localhost Minecraft1.21.11 Vanilla TCP server. No live Hypixel or third-party anti-cheat was tested. Automation and modified behavior can still be detected; undetectability is not guaranteed.
