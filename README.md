# Cardputer ADV WebRadio (100 stations)

Internet radio for the **M5Stack Cardputer ADV**, based on
[WebRadio_WuSiU_Cardputer_Adv](https://github.com/wusiu/WebRadio_WuSiU_Cardputer_Adv) v2.0.0 by WuSiU
(itself based on [M5Cardputer_WebRadio](https://github.com/cyberwisk/M5Cardputer_WebRadio) by cyberwisk).

## Changes from WuSiU v2.0.0

Only two functional changes:

- **Up to 100 stations** in `station_list.txt` (was 20: longer lists were silently cut).
- **Back key**: `` ` `` closes the station list (same as `L`).

Plus a PlatformIO build (see below) instead of the Arduino IDE.

## Station list

Put `station_list.txt` at the root of the SD card, one station per line, name and stream URL separated by a comma:

```
FIP, http://icecast.radiofrance.fr/fip-midfi.mp3
France Inter, http://icecast.radiofrance.fr/franceinter-midfi.mp3
```

Without an SD card (or file), the station built into the firmware is used.

## Controls

| Key | Action |
|---|---|
| `/` / `,` | Next / previous station |
| `;` / `.` | Volume up / down |
| `M` | Mute |
| `L` | Station list (`;` `.` to move, `Enter` to play, `L` or `` ` `` to close) |
| `R` | Reconnect to the stream |
| `F` | FFT visualizer on / off |
| `B` | Screen brightness |

Wi-Fi: up to 5 networks are remembered. Hold `G0` while it connects to forget them.

## Build

[PlatformIO](https://platformio.org/):

```
pio run
```

Notes for this board (no PSRAM), learned the hard way:

- **Board variant**: the project uses `board_build.variant = m5stack_cardputer`, like the "M5Cardputer" board of the
  Arduino IDE. With the generic ESP32-S3 devkit variant, `SD.begin()` uses the wrong default SPI pins and takes
  GPIO11, the keyboard interrupt of the Cardputer ADV: the keyboard stops working and the SD card is not read.
- **ESP32-audioI2S 3.2.1**: newer versions (3.3 / 3.4+) allocate a ~720 KB stream buffer in PSRAM and play nothing
  on the Cardputer ADV. 3.2.1 falls back to internal RAM.
- Platform: [pioarduino](https://github.com/pioarduino/platform-espressif32) `54.03.21-2` (Arduino core 3.2.1),
  the core used by the original build.

The resulting `.pio/build/cardputer-adv/firmware.bin` can be installed with
[M5Launcher](https://github.com/bmorcelli/Launcher) from the SD card.

## License

MIT, see [LICENSE](LICENSE) (original copyright WuSiU).
