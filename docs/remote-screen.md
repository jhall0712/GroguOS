# ESP-NOW Display Remote

The main firmware includes an ESP-NOW command receiver in the puppet controller
and a separate display firmware project for the round remote screen.
The web flasher publishes this display image as `Display 0.2.9`.

## Hardware Target

The display project targets an ESP32-S3 board with:

- 1.28 inch round LCD
- GC9A01 display driver
- CST816S capacitive touch controller
- 2.4 GHz Wi-Fi support

The first-pass configuration uses TFT_eSPI's Waveshare GC9A01 setup and these
touch pins:

| Signal | GPIO |
| --- | --- |
| Touch SDA | `GPIO6` |
| Touch SCL | `GPIO7` |
| Touch RST | `GPIO13` |
| Touch IRQ | `GPIO5` |

## Flashing

Use the web flasher and select `Display 0.2.9` for the round display board.
Use the current recommended Master firmware for the puppet controller board.

After flashing the display, connect to Wi-Fi AP `GroguOS Display` and open
`http://192.168.5.1`.

## Connection

The puppet and display communicate with ESP-NOW on channel `1`. The display
sends broadcast command packets, and the puppet listens while still hosting the
`Grogu Setup` Wi-Fi AP.

Default display connection settings:

| Setting | Default |
| --- | --- |
| Display setup AP | `GroguOS Display` |
| Display setup page | `http://192.168.5.1` |
| Puppet AP SSID | `Grogu Setup` |
| Puppet config IP | `192.168.4.1` |

Use the display setup page's `Connection` tab to change the GroguOS Display SSID or
password, puppet AP SSID, puppet IP, and press hold timing. ESP-NOW uses the
shared channel; the puppet IP is used for HTTP fallback/config references.

## Commands

The first supported command families are:

| Family | Behavior |
| --- | --- |
| Built-in | Auto Mode, Arm Mode, Right Arm Wave, Surprise No |
| Maestro | Restart Maestro script subroutine with a holdoff |
| Sound | Play numbered MP3 tracks, random categories, stop sound, set volume |

## Display UI

| Gesture | Action |
| --- | --- |
| Tap | Send selected command |
| Swipe left/right | Change menu |
| Swipe up/down | Change selected item |

The display setup page has menu tabs for built-ins, Maestro sequences, sounds,
and combos. Each command can be shown or hidden, relabeled, and assigned command
values such as sequence number, track number, volume, or holdoff.
