# bobton v1 — interaction hardening

This update keeps the 47 modules and existing menu, including Hypixel below Misc with Scaffold and Clutch. It reduces avoidable request conflicts and corrects interaction timing; it does not establish compatibility with every anti-cheat.

## Changes

- Automated attacks, placements, item uses and offhand swaps share one interaction opportunity per client tick. Normal outgoing player actions make later automation yield. Cancelled protection callbacks do not consume that opportunity, preserving AutoTrap's manual-input redirect.
- Outgoing end-of-tick automation runs before Minecraft 1.21.11's actual tick-end packet. Paused/disconnected cleanup still runs without sending gameplay actions.
- Rotation updates use Minecraft's actual last-sent orientation bookkeeping. Equivalent full turns are coalesced, Clutch preserves continuous yaw, and the normal movement stream restores the camera orientation after temporary action aiming. Position, sprinting and positional heartbeat tracking remain owned by Minecraft.
- AutoPot uses the yaw/pitch already carried by a normal use-item packet. It respects an existing manual use delay and consumes only one queued potion press which its accepted throw has already fulfilled. Additional intentional clicks and unrelated held-item input remain available. Rejected attempts back off instead of swapping/rotating on every tick.
- Fast Place applies each accepted-use delay once. Values 5–10 now slow held use correctly without restarting the countdown. The existing `anticheat_safe` preference is preserved, with the truthful UI label **Vanilla Use Delay**; its enabled default preserves at least the ordinary four-tick held-use interval. Aggressive settings remain server-visible.
- Scaffold and Clutch share a bounded ten-tick per-position retry window. Different supported connector blocks retain their placement cadence; switching modules or rolling back a prediction cannot immediately repeat the same position. Player/world changes clear stale attempts.
- AutoTrap checks support loading, height, border, placement permissions and collision. Its manual obsidian redirect no longer cancels scoped Scaffold/Clutch placements.
- Automated entity attacks exclude the client-only Reach extension while honoring server-supplied native weapon attack-range components. Reach still controls client selection. Fastbreak remains client prediction subject to server validation.
- A local Fake Player practice attack no longer sends a duplicate hand swing or allows a second automated practice hit in the same tick.

## Verification

The build passes **208 Java tests**, with no failures, errors or skips. New regressions cover shared interaction ownership, retry windows, rotation arithmetic, held-use pacing, queued potion presses and range policy.

Native verification uses the unmodified release JAR and embedded Fabric API against a private localhost Minecraft 1.21.11 Vanilla server. Minecraft's own outgoing TCP packet logger is enabled only in the private test profile; no packet-logging instrumentation is added to the distributed client.

The localhost checks recorded actual outbound packet types and tick-end boundaries, together with authoritative server inventory, health and block state:

| Check | Result |
| --- | --- |
| AutoPot with a queued held-use press | Healing applied (8→12 health), camera restored, and subsequent requests were four tick markers apart; the earlier regression emitted two uses in one marker window |
| Fast Place / Vanilla Use Delay | 15 bottles in the three-second sample; every request interval was exactly four ticks |
| Fast Place / Delay10 | 7 bottles in the three-second sample; every request interval was exactly ten ticks |
| Ordinary weapon / local Reach6 | No automated entity-interaction packets against the out-of-normal-range target; target health stayed20 |
| Server-granted weapon range5 | Extended-range target received two attacks and health fell20→6.224; the native component remained usable |
| AutoTrap manual redirect | Three supported placements succeeded while holding use, without cancelled manual attempts starving automation |
| Scaffold + Clutch | Real knockback caught beyond the original ledge, two stone used, grounded at original feet height, sword restored; both new blocks confirmed server-side |
| Local Fake Player | No entity-interaction packets; observed swings never duplicated within one tick marker window |

Potion timing used a server-authorized stacked healing-potion fixture to expose duplicate consumption. The knockback fixture raised maximum health for test safety without changing movement or knockback. Packet logging and offline localhost server settings were private test configuration, not part of the download. These targeted checks do not cover every module setting combination or other server software.

## Limits

These are protocol-correctness and request-coordination improvements. Automation, modified timing/reach, off-crosshair actions and predictable behavior can still be detected or rejected by a server. No live Hypixel or third-party anti-cheat was tested, and undetectability is not guaranteed.
