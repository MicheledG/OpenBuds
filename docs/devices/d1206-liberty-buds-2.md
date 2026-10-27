# soundcore Liberty Buds 2 (D1206) — vendor app feature inventory

What the vendor app exposes for this device, read from screen recordings of
the soundcore Android app with firmware 03.61. This is the target feature set
for OpenBuds; the matching protocol commands are documented separately once
captured.

## Main screen

- **Battery:** left, right and case, as percentages.
- **Ambient Sound:** four states — ANC off, ANC on, Adaptive ANC,
  Transparency.
  - **Mode** (ANC on): ANC level 1–5; level 5 is labelled "Ultra ANC"
    ("blocks out full-spectrum ambient noise as much as possible").
  - **Wind Noise Reduction:** on/off.
- **HearID:** equalizer profile ("Anker Signature" by default).
- **Gaming Mode:** on/off — lower latency.
- **Hearing Health:**
  - Real-Time Volume Monitoring: on/off — the earbuds measure sound levels
    in the ear and around it.
  - Volume Limit: on/off, with a maximum output volume (90 dB shown).
- **Controls:** master on/off, then per earbud (left / right):
  - Audio gestures — single, double, triple and long press. Each maps to one
    of Volume −, Volume +, Next, Previous, Play/Pause, None; long press
    defaults to Ambient Sound (cycle listening modes).
  - Call gestures — fixed: double press answers or declines, long press
    rejects a call or switches ambient sound during a call.
  - Touch Sensitivity: High / Medium / Low.
  - Reset to defaults.

## More Settings

- **Wearing Detection:** on/off — pause when an earbud is removed, resume
  when it is worn again (optical sensor).
- **Dual device connection:** multipoint on/off, list of connected devices
  with a toggle each, connect a new device.
- **Prompt Tone** (not yet recorded).
- **Sound Mode** (not yet recorded; likely codec / connection priority).
- **Fit Test:** checks the ear-tip seal for each earbud.
- **Auto Power Off:** 10, 20, 30, 60 minutes or never, when disconnected and
  not worn.
- **Find Device:** play a sound on the left or right earbud.
- **Device Name**, **Update Firmware** (shows the firmware version),
  **Serial Number**.

## Not present

- No conversation detection ("Easy Chat" / "Smart Conversation" on other
  soundcore models): the app has no setting that switches to transparency
  and pauses media when the wearer speaks.

## Notes for the protocol work

- The Auto Power Off screen describes the device as "Liberty 3 Pro", which
  suggests the vendor app reuses that model's code path. The Liberty 3 Pro
  (A3952) protocol is already documented by OpenSCQ30, so it is the first
  candidate to compare captures against.
- Screens still to record: Transparency options (if any), Sound Mode,
  Prompt Tone, HearID, the long-press option list.
