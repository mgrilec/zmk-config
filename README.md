# Temper firmware

The **Build temper** GitHub Actions workflow builds the QWERTY Miryoku keymap for
`nice_nano_v2`, with Nice!View displays on both halves. Temper builds use ZMK
**v0.3.0** so the board names and dependencies remain compatible. Other keyboard
workflows are unchanged.

## ZMK Studio

The left (central) firmware enables ZMK Studio with the `studio-rpc-usb-uart`
snippet. The right peripheral does not enable Studio. Studio locking stays enabled.
The 36-key layout in `config/temper-layouts.dtsi` follows the Temper PCB switch
positions and the existing matrix/keymap order.

1. Run **Actions → Build temper → Run workflow** on the branch containing these
   changes and download the firmware artifacts from both successful jobs.
2. Flash the `temper_left` UF2 onto the left controller and the `temper_right` UF2
   onto the right controller. Do not interchange the two images.
3. Connect the **left half** to the computer using a USB data cable.
4. If Bluetooth output is selected, hold **Esc** (Media), hold a Shift modifier on
   that layer, and press the **N position** (Output toggle) to force USB output.
5. Open <https://zmk.studio/> in Chrome or Edge and connect to the keyboard's
   USB serial port.
6. Unlock editing: hold **Esc** to activate Media, then press the physical
   **G and B positions together** (the left inner-index keys on the home and
   bottom rows, positions 14 and 24). Press the pair within 50 ms. Keep this
   unlock chord available when editing the keymap.

Esc here means the outer left thumb in the stock Base/Extra layers. Letter
positions above refer to the stock QWERTY Base layout, not typed characters on
Media. The unlock combo only applies on Media; it does not change normal G/B
behavior. The Tap layer has no access to Media; restart the keyboard to return to
Base if necessary.

Studio can reassign existing behaviors and edit the ten compiled layers. It
cannot create new firmware behaviors or change their compiled properties.
Studio changes are stored on the keyboard: later `.keymap` changes may require
**Restore Stock Settings** in Studio before they take effect. Restoring stock
settings discards Studio keymap edits.

References: [ZMK Studio](https://zmk.dev/docs/features/studio) and
[physical layouts](https://zmk.dev/docs/hardware-integration/physical-layouts).
