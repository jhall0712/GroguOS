# RC Button Assignments

Firmware `0.1.6` supports the DS-650's four assignable buttons on SBUS `CH3`
through `CH6`.

## Default Channel Mapping

| Setup page button | Expected SBUS channel | Firmware index |
| --- | --- | --- |
| RC3 Button 3 | `CH3` | `SEQUENCE_BUTTON_SBUS_CHANNELS[0]` |
| RC4 Button 4 | `CH4` | `SEQUENCE_BUTTON_SBUS_CHANNELS[1]` |
| RC5 Button 5 | `CH5` | `SEQUENCE_BUTTON_SBUS_CHANNELS[2]` |
| RC6 Button 6 | `CH6` | `SEQUENCE_BUTTON_SBUS_CHANNELS[3]` |

## Setup Page

Connect to Wi-Fi AP `Grogu Setup`, open `http://192.168.4.1`, then choose the
`Button Assignments` tab.

Each of the four button boxes has:

| Setting | Purpose |
| --- | --- |
| Action | Disabled, built-in sequence, Maestro sequence, or direct servo control. |
| Built-in sequence | `Auto Mode toggle`, `Arm Mode toggle`, `Right Arm Wave`, or `Surprise No`. |
| Maestro sequence | Maestro script subroutine number, such as `00`, `01`, `02`. |
| Holdoff ms | How long the ESP32 pauses normal servo target updates after starting a Maestro sequence. |
| Direct servo channel | Maestro channel controlled directly by this RC channel. |

Settings are saved in ESP32 Preferences/NVS.

RC button actions are edge-triggered. A button channel must rise above the press
threshold and then fall back below the release threshold before it can trigger
again. This prevents noisy SBUS button channels from repeatedly starting the
same Maestro sequence.

## Direct Servo Control

When a button is assigned to `Direct servo control`, the firmware continuously
maps that RC channel's SBUS value to the selected Maestro servo channel.

Allowed direct targets are:

| Setup option | Maestro channel |
| --- | --- |
| `Maestro CH2 - Neck rotate` | `CH2` |
| `Maestro CH3 - Left arm` | `CH3` |
| `Maestro CH4 - Right arm` | `CH4` |
| `Maestro CH5 - Left Wrist Servo` | `CH5` |
| `Maestro CH6 - Right Wrist Servo` | `CH6` |

Maestro `CH0` and `CH1` are not available for direct RC button control because
they are reserved for the paired head tilt mechanism.

Direct control uses the selected servo's saved `min`, `center`, `max`, and
`reversed` settings. It is active during normal manual control when SBUS signal
is healthy. If the assigned RC control is only a two-position button, the servo
will move between the low and high ends of its saved range. If the transmitter
can make that channel proportional or multi-position, the servo follows that
range more smoothly.

## Built-In Sequences

| Built-in sequence | Behavior |
| --- | --- |
| `Auto Mode toggle` | Toggles firmware auto lifelike mode on/off. |
| `Arm Mode toggle` | Toggles the stick between head/neck control and left/right arm control. |
| `Right Arm Wave` | Raises the right arm, waves it a few times, then lowers it. |
| `Surprise No` | Raises both arms partway in a staggered surprise pose, shakes the head left/right like "no", then lowers arms and centers the head. |

`Right Arm Wave` uses the saved right-arm servo settings from the `Servos` tab.
The motion is clamped to the configured `min` and `max`, returns to `center`,
and uses the right arm's `reversed` flag to choose which endpoint means raised.

`Surprise No` uses the saved left arm, right arm, and neck rotate settings. Arm
raise targets use each arm's `reversed` flag, and the neck shake uses the neck
rotate `reversed` flag so left/right shake direction stays correct. The arms
raise to a partial endpoint and are staggered to reduce current spikes. The neck
movement stays inside the saved neck `min` and `max`.

Built-in firmware motion is clamped to each servo's saved endpoints. After
changing endpoints on the setup page, click the relevant save button before
testing a built-in sequence again.

If Wi-Fi drops or the ESP32 restarts during `Surprise No`, that usually points
to the 5V rail sagging while multiple servos move. Use a high-current regulated
5V source, keep wiring short/heavy for the servo rail, and consider lowering
arm speed/acceleration on the `Servos` tab.

## Maestro Sequence

When a button is assigned to `Maestro sequence`, the ESP32 sends the Pololu
Maestro compact serial command:

```text
0xA7, subroutine_number
```

This is Maestro's `Restart Script at Subroutine` command. Subroutines are
numbered from `0`, so setup page value `00` starts subroutine 0, `01` starts
subroutine 1, and so on.

The holdoff value is important because the ESP32 normally sends live servo
targets to the Maestro every `20 ms`. During holdoff, the ESP32 pauses those
normal target updates so the Maestro script has time to run. When holdoff
expires, the ESP32 sends Maestro `Stop Script` and resumes normal servo
control.

## Maestro Script Tips

- Maestro subroutines triggered by serial should end with `QUIT`.
- Avoid ending a serial-triggered subroutine with `RETURN`, because there is no
  caller to return to.
- Keep holdoff long enough for the sequence you expect to see.
