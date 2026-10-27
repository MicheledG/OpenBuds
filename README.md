# OpenBuds

An open-source companion app for true-wireless earbuds, for Android and
macOS. The goal: switch noise cancelling and transparency, and see battery,
from wherever is fastest — without opening the vendor app.

> **Status: early design.** Nothing to install yet. The first supported
> device will be the soundcore Liberty Buds 2 (model D1206).

## Planned features

**Android**
- Quick Settings tile that cycles noise cancelling → transparency → off.
- Home-screen widgets with battery for each earbud and the case, and
  one-tap listening modes.
- Notification actions while the earbuds are connected.
- Noise-cancelling and transparency levels, wind reduction, touch controls,
  wear detection.

**macOS**
- Menu bar item with battery and listening modes, in the spirit of the
  AirPods controls.
- Widgets and Shortcuts actions, plus global keyboard shortcuts.
- Install with Homebrew.

## How it works

The earbuds are controlled over Bluetooth with the same commands the vendor
app sends. A shared core implements that protocol once; the Android and
macOS apps are native and call into it.

## Supported devices

| Device | Model | Status |
|---|---|---|
| soundcore Liberty Buds 2 | D1206 | Planned |

## Contributing

Read [`AGENTS.md`](AGENTS.md) first. This repository is public: never commit
Bluetooth captures, device addresses, serial numbers or anything else that
identifies a person or a device.

## Acknowledgements

[OpenSCQ30](https://github.com/Oppzippy/OpenSCQ30) documents the protocol of
many soundcore devices and is the main prior art for this project.

## Disclaimer

OpenBuds is an independent project. It is not affiliated with, endorsed by
or sponsored by Anker Innovations. soundcore, Anker and Liberty are
trademarks of their respective owners and are used here only to identify
compatible devices.

## License

[GPL-3.0-or-later](LICENSE).
