# RC Input Calibration

Firmware `0.1.9` and later include an `RC Input` tab on the `Grogu Setup` web
page.

Use this when the puppet is centered with the remote off, but moves when the
remote turns on with the stick released.

## What It Does

The firmware saves calibration for SBUS `CH1` through `CH6`:

| Setting | Purpose |
| --- | --- |
| `Min` | SBUS value when the control is held fully low. |
| `Center` | SBUS value when the stick/control is released. |
| `Max` | SBUS value when the control is held fully high. |
| `Deadband` | Neutral zone around center that still outputs the servo center. |

Live RC control maps saved RC center to the servo's saved `center`. This means
the remote no longer has to center exactly at SBUS `992`.

For paired head tilt, the stick up/down channel is converted into one shared
tilt percentage. Maestro `CH0` and `CH1` then use that same percentage around
their own saved centers and endpoint ranges.

## Basic Head Stick Calibration

1. Turn the transmitter on and leave the stick released.
2. Connect to Wi-Fi AP `Grogu Setup`.
3. Open `http://192.168.4.1`.
4. Open the `RC Input` tab.
5. For `RC2 Stick up/down`, click `Use Live as Center`.
6. For `RC1 Stick left/right`, click `Use Live as Center`.
7. Save RC input calibration.

If you want full travel calibration, hold each stick axis at one end and capture
`Min`, then hold it at the other end and capture `Max`.

## Tuning

Start with a deadband around `40`. Increase it if the head still drifts when the
stick is released. Lower it if the stick feels too insensitive around center.
