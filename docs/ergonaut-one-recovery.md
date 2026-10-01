# Ergonaut One dongle recovery

Use either of these chords on a half that is still connected to the dongle:

| Connected half | Press together | Key positions |
| --- | --- | --- |
| Left | Q + W + E + R | 1, 2, 3, 4 |
| Right | U + I + O + P | 7, 8, 9, 10 |

Stop typing for at least one second, then press all four keys together within
100 ms. The chord works on every layer, using the physical positions of those
letters on the default QWERTY layer. It immediately reboots the dongle; no long
hold is required. Release the keys and allow USB and the split links to reconnect.

ZMK executes reset behaviors invoked by a combo on the split central, which is
the dongle in this configuration. The chord does not clear Bluetooth pairings.
It cannot reach the dongle when both halves are disconnected, and restarting the
dongle is not guaranteed to recover a peripheral whose own firmware is stuck.

## Build and flash

Use the **Build ZMK firmware** workflow, which builds the dongle and both halves
from `build.yaml`. The separate legacy **Build ergonaut_one** workflow does not
build this dongle setup. Pull requests automatically run the full firmware matrix;
the workflow also remains available through **Run workflow**.

Download the `firmware` artifact from a successful run. For Ergonaut One, flash
the matching UF2 to each of these three devices:

- `ergonaut_one_dongle dongle_screen` on `xiao_ble//zmk`
- `ergonaut_one_left` on `xiao_ble//zmk`
- `ergonaut_one_right` on `xiao_ble//zmk`

Update all three devices together for the ZMK/Zephyr upgrade. Enter each XIAO's
bootloader and copy its matching UF2 to the bootloader drive. Keep the previous
firmware as a rollback option. Do not flash `settings_reset` for a normal upgrade;
it erases stored settings and pairing information.

## Version and compatibility

ZMK and its reusable build workflow are pinned to
`5b51501fead672c41b5cfb396f3dafe0894bf4e9` (upstream main, 2026-09-26).
This includes the split discovery TX-buffer fix merged in
[ZMK PR #3216](https://github.com/zmkfirmware/zmk/pull/3216).
That fix addressed behavior on newer Zephyr; it does not establish the cause
of disconnects on the previous v0.3 firmware.

The upgrade requires the Zephyr 4.1 board IDs, MIPI DBI display bus, and LVGL 9
display APIs. The shared dongle screen and other targets in `build.yaml` are
migrated alongside Ergonaut so the common firmware build remains usable.
Peripheral battery fetching remains enabled.

After flashing, check both halves, the screen's orientation/colors/battery
indicators, and each recovery chord separately. Physical reconnect reliability
and screen behavior still require testing on the actual hardware.

References: [ZMK combos](https://zmk.dev/docs/keymaps/combos) and
[Zephyr 4.1 migration](https://zmk.dev/blog/2025/12/09/zephyr-4-1).
