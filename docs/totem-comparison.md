# Official TOTEM and this build

## Bottom line

Keep the current TOTEM shield and matrix definitions. They match the official
TOTEM's 38-key hardware. Do not replace this repository with the official
ZMK configuration: the official hardware repository labels that source as
"outdated," and its build only targets the two keyboard halves.

For direct Bluetooth to the laptop, make a fresh current-ZMK configuration
from this repository's two half shields, remove the dongle build, and make
one half, conventionally the left, the central. Start the keymap over
separately. A new layout does not require a new PCB definition.

## What is the same

The official TOTEM is a 38-key column-staggered Choc split. Its wireless
version uses two Seeed Studio XIAO nRF52840 BLE controllers. Its wired version
uses XIAO RP2040 controllers. [Official hardware README](https://github.com/GEIGEIGEIST/TOTEM#firmware)

The local shield uses the same 10-column by 4-row transform and the same XIAO
matrix pins as the official ZMK source: D0 through D3 for rows, D4, D5, D10,
D9, and D8 for the left columns, with the right side mirrored. The local
version adds current-ZMK physical-layout metadata and `wakeup-source`; it does
not change the switch matrix or physical key count. The official build guide's
wireless parts list also specifies two XIAO nRF52840 BLE boards, two batteries,
and two power switches. [Official build guide](https://github.com/GEIGEIGEIST/TOTEM/blob/main/docs/buildguide.md)

That makes a keymap reset a firmware choice, not a hardware-layout migration.
I cannot prove the installed PCB revision from this repository alone. If the
board's silkscreen or a photo says something other than TOTEM 0.3, check it
before ordering a new case or replacement PCB.

## What differs

| Area | Current repository | Official ZMK source |
| --- | --- | --- |
| Build targets | Three XIAO BLE images: left, right, and `totem_dongle` | Two XIAO BLE images: left and right |
| Central | Dongle | Left half |
| Host connection | Dongle connects to laptop over USB; halves connect to dongle over BLE | Left half connects to laptop over BLE; right half connects to left over BLE |
| Extra firmware | Prospector adapter/display, ZMK Studio over USB-UART on the dongle, +8 dBm TX power, battery proxy/fetching | Basic older ZMK setup |
| Keymap | A heavily customized three-layer map with home-row mods | An unrelated older personal keymap |

The official project's `build.yaml` names only `totem_left` and `totem_right`,
and its shield configuration makes the left half central. [Official ZMK build file](https://github.com/GEIGEIGEIST/zmk-config-totem/blob/master/build.yaml) [Official ZMK shield config](https://github.com/GEIGEIGEIST/zmk-config-totem/blob/master/config/boards/shields/totem/Kconfig.defconfig)

## Bluetooth and battery trade-off

The old build is already a Bluetooth split internally. The change you want is
to remove the USB receiver and pair the keyboard's central half directly to
the laptop over BLE.

That saves a controller and receiver, but shifts the central role back to a
keyboard half. ZMK says the central consumes significantly more power than a
peripheral because it wakes its radio to receive transmissions. A dongle is
specifically useful because it moves that role off the battery-powered halves.
[ZMK split roles and battery impact](https://zmk.dev/docs/features/split-keyboards#central-and-peripheral-roles)

So expect the direct-BLE left half to use more battery than it does today. The
right half remains a peripheral. ZMK's idle and deep-sleep modes still matter;
deep sleep uses very little power but takes a few seconds to reconnect.
[ZMK low-power states](https://zmk.dev/docs/features/low-power-states)

## Recommended migration

1. Keep `totem_left`, `totem_right`, and the matrix overlays from this repo.
2. Remove the dongle target and its Prospector/Studio-specific settings from
   the new configuration.
3. Configure the left half as central and build only left and right firmware.
4. Write the new keymap from scratch against the same 38 positions.
5. Clear settings/bonds on both keyboard controllers before the first direct-
   BLE pairing. Split pairing data persists across flashes, and ZMK documents
   settings reset for resolving changed split pairings. [ZMK connection troubleshooting](https://zmk.dev/docs/troubleshooting/connection-issues#split-keyboard-parts-unable-to-pair)

The practical choice is: use the official repository as the hardware reference,
but use this repository's shield as the starting point for a modern direct-BLE
config. Copying the official firmware wholesale would move the project back to
an explicitly outdated ZMK base and discard the local compatibility updates.
