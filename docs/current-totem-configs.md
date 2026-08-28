# Current direct-Bluetooth TOTEM configurations

Research date: 2026-08-27.

There is no newer official, canonical TOTEM ZMK configuration. The original
author's configuration still builds direct Bluetooth left and right halves,
but the hardware repository labels that source "outdated." [Official TOTEM
firmware links](https://github.com/GEIGEIGEIST/TOTEM#firmware)

## Candidates

| Repository | What it is | Recommendation |
| --- | --- | --- |
| [rleyvasal/totem-zmk-config](https://github.com/rleyvasal/totem-zmk-config) | Active personal direct-BLE Totem config. Its build has left, right, and settings-reset images, and it pins a patched ZMK revision. | Most current public reference, but not a clean starter. It includes personal Colemak-DH behavior, host-management code, and a ZMK fork. Copy selective ideas, not the whole repo. |
| [rcarmo/zmk-config-totem](https://github.com/rcarmo/zmk-config-totem) | Direct-BLE Totem config with left, right, and settings-reset images. Its manifest follows ZMK `main`. | A useful simple reference, but it was last pushed in April 2026. |
| [GEIGEIGEIST/zmk-config-totem](https://github.com/GEIGEIGEIST/zmk-config-totem) | Original two-half direct-BLE configuration. | Best hardware reference. The official hardware project calls this ZMK source outdated, so do not treat it as the maintenance base. |

## Recommendation

Start from the existing repository because it already uses current ZMK `main`
and has the exact Totem shield and matrix. Convert it to the two-half layout:
left is central, right is peripheral, remove the dongle/Prospector build, and
write a new keymap. This stays simpler than adopting a stranger's personal
firmware or an older upstream configuration.
