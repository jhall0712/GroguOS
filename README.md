# GroguOS

PlatformIO Arduino firmware for a Seeed Studio XIAO ESP32-S3 puppet controller.
GroguOS drives the puppet servos through a Pololu Maestro, reads an SBUS RC
receiver, plays DFPlayer Mini sounds, and hosts a browser-based setup and
control interface from the ESP32 setup access point.

Current master firmware version: `0.3.1`

Current display firmware version: `Display 0.2.8`

Current stable rollback version: `0.3.0`

Flash the latest firmware via web browser here:
[Web Flash Tool](https://animated-grogu.pages.dev/)

## Documentation

- [Docs index](docs/README.md)
- [Wiring reference](docs/wiring.md)
- [RC remote controls](docs/rc-controls.md)
- [RC input calibration](docs/rc-input-calibration.md)
- [RC button assignments](docs/button-assignments.md)
- [Settings backup](docs/settings-backup.md)
- [Combo sequences](docs/combo-sequences.md)
- [Auto lifelike mode](docs/auto-mode.md)
- [Servo calibration](docs/calibration.md)
- [Servo setup live preview](docs/servo-setup-preview.md)
- [First upload and test procedure](docs/first-upload-test.md)
- [ESP-NOW display remote](docs/remote-screen.md)
- [Changelog](CHANGELOG.md)

## Hardware

- HOTRC SBUS-A receiver SBUS signal to XIAO `D7` / `GPIO44` by default.
- XIAO `D6` / `GPIO43` by default to Pololu Maestro `RX`.
- DFPlayer Mini serial/audio wiring for sound playback.
- Single regulated high-current 5V source feeds the Maestro servo rail.
- Maestro VIN/servo-power jumper powers Maestro logic from the servo rail.
- XIAO `5V` and receiver VCC are fed directly from the external regulated 5V source.
- Common ground between source, receiver, XIAO, Maestro, DFPlayer, and servos.

Pin defaults are summarized in the docs and live in the private firmware source.

## GroguOS Setup

The ESP32 creates a setup Wi-Fi access point. By default it is named
`Grogu Setup`, but the AP name is configurable from the System settings page.
The setup AP password is optional.

Connect to the setup AP and open `http://192.168.4.1`. The landing screen opens
with two paths:

- `Control` opens the public Grogu interaction screen.
- `Settings` opens the password-protected setup area when a settings password is configured.

Settings are saved in ESP32 Preferences/NVS and can be exported/imported from
the System page.

## Control

The public Control screen is intended for show interaction without exposing the
setup pages. It supports configured buttons for:

- Movements
- Sounds
- Combo sequences

The mobile landscape control mode adds on-screen joystick controls. The left
joystick controls paired head tilt and neck rotate. The right joystick controls
arms and elbows, with selectors for left, right, or both sides. Joystick
positions hold where released instead of snapping back to center.

## Settings

The Settings page includes:

- `System` for network, setup access, settings backup, and system tools.
- `Servos` for per-servo min, center, max, reverse, speed, acceleration, and smoothing.
- `Button Assignments` for RC button actions, command tests, and sound tests.
- `Control Preview` for configuring the public Control screen.
- `Combos` for multi-step sound, movement, display, and timing sequences.
- `RC Input` for transmitter calibration.

## Sounds

DFPlayer Mini tracks can be played by exact track number or by fixed random
category. Random categories are mapped by track number range:

- `Cute`: tracks `1` through `25`
- `Phrases`: tracks `26` through `50`
- `Sentences`: tracks `51` through `75`
- `Burps and Slurps`: tracks `76` through `100`
- `Sound Combos`: tracks `101` through `125`

Random category sound actions are available from RC Button Assignments and the
public Control screen.

## RC Controls

- Stick up/down, SBUS CH2 -> Maestro CH0 and CH1 head tilt together, or Maestro CH3 left arm in arm mode.
- Stick left/right, SBUS CH1 -> Maestro CH2 neck rotate, or Maestro CH4 right arm in arm mode.
- Assignable RC buttons, SBUS CH3-CH6 -> built-in toggles, sound actions, combo calls, or direct servo controls.
- Assign a button to `Auto Mode toggle` for auto lifelike mode.
- Assign a button to `Arm Mode toggle` for live arm control.
- Use the `RC Input` setup tab to calibrate remote centers if the head drifts when the transmitter turns on.
- Head tilt uses paired mixing, so CH0 and CH1 can have different centers but move together from the same stick command.

## Firmware

The firmware source repository is private. Public firmware releases are shared
through the web flasher when available.
