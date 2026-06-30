# DFPlayer Mini Sound

The firmware can control a DFPlayer Mini over UART and play MP3 files from the
DFPlayer microSD card.

## File Naming

Place MP3 files in the microSD card `/MP3` folder using four-digit numbered names:

```text
0001.mp3
0002.mp3
0003.mp3
```

The `Sound Test` tab on the `Grogu Setup` page sends the track number to the
DFPlayer. Track `1` plays `/MP3/0001.mp3`, track `2` plays `/MP3/0002.mp3`,
and so on.
The firmware uses a command-only UART connection and does not read DFPlayer
status responses.

## Wiring

| XIAO ESP32-S3 | DFPlayer Mini | Notes |
| --- | --- | --- |
| `D0` / `GPIO1` | `RX` | Put about `1k` in series on this line. |
| `GND` | `GND` | Must share common ground with the ESP32 and main power system. |
| Regulated `5V` | `VCC` | Use a supply that can handle audio current spikes. |
| Speaker | `SPK1` / `SPK2` | Speaker connects to DFPlayer, not the ESP32. |

The default firmware pins are:

```cpp
constexpr int8_t DFPLAYER_TX_PIN = 1;  // XIAO D0 / GPIO1 sends to DFPlayer RX
```

## Setup Page Test

1. Connect to Wi-Fi AP `Grogu Setup`.
2. Open `http://192.168.4.1`.
3. Choose the `Sound Test` tab.
4. Enter a track number and volume from `0` through `30`.
5. Click `Play Track`.

If a track does not play, check DFPlayer power, common ground, the one-way
ESP32 `D0` to DFPlayer `RX` wiring, the series resistor into DFPlayer `RX`, and
that the microSD card is inserted with numbered MP3 files in the `/MP3` folder.
