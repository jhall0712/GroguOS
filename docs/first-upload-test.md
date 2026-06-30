# First Upload and Test Procedure

## Before Powering Servos

1. Remove servo horns or disconnect linkages for first motion tests.
2. Confirm the external source is regulated 5V and sized for servo current.
3. Confirm the Maestro VIN/servo-power jumper is installed if your Maestro
   model supports powering logic from the servo rail.
4. Confirm the external regulated 5V source feeds XIAO `5V` and receiver VCC directly.
5. Confirm XIAO, receiver, Maestro, and servo grounds will be common.
6. Confirm the receiver SBUS signal voltage is safe for ESP32-S3 `3.3V` GPIO.

## Build

```powershell
pio run
```

## Upload

Connect the XIAO ESP32-S3 by USB, then run:

```powershell
pio run -t upload
```

## Serial Monitor

```powershell
pio device monitor -b 115200
```

Expected boot message:

```text
Puppet controller v0.2.8 ready. Wi-Fi AP: Grogu Setup, config page: http://192.168.4.1
```

## Web Configuration

1. Connect a phone or laptop to Wi-Fi network `Grogu Setup`.
2. Open `http://192.168.4.1`.
3. Use the `Calibrate` tab to test each Maestro channel carefully, or set
   conservative first-test limits on the `Servos` tab, such as:

| Setting | Example |
| --- | --- |
| Min | `5600` |
| Center | `6000` |
| Max | `6400` |
| Speed | `25` |
| Acceleration | `8` |
| Smoothing | `0.18` |

4. Save settings.

## Maestro Setup

Set the Maestro serial configuration to:

| Setting | Value |
| --- | --- |
| Serial mode | UART serial |
| Baud rate | `9600` |
| Protocol | Compact serial-compatible |
| CRC | Off |

## First Motion Test

1. Connect XIAO `D6` / `GPIO43` to Maestro `RX`.
2. Connect XIAO GND to Maestro GND.
3. Power the Maestro logic.
4. Attach one servo to Maestro `CH0`.
5. Apply the single regulated high-current 5V source to the Maestro servo rail.
6. Move SBUS `CH1` slowly and confirm the servo responds within the configured
   limits.
7. Repeat one servo at a time for Maestro channels `CH1` through `CH6`.

## DFPlayer Sound Test

1. Put numbered MP3 files in the DFPlayer microSD card `/MP3` folder, starting with
   `0001.mp3`.
2. Wire the DFPlayer Mini as shown in the wiring reference.
3. Connect to `Grogu Setup` and open `http://192.168.4.1`.
4. Choose the `Sound Test` tab.
5. Enter track `1`, choose a modest volume such as `15`, and click
   `Play Track`.

## RC Mode Checks

| RC channel | Firmware behavior |
| --- | --- |
| Stick up/down on SBUS `CH2` | Head mode: Maestro `CH0` and `CH1`; arm mode: Maestro `CH3` |
| Stick left/right on SBUS `CH1` | Head mode: Maestro `CH2`; arm mode: Maestro `CH4` |
| SBUS `CH3`-`CH6` button press | Run saved button assignment |

In manual head mode, stick up/down moves both head tilt servos together using a
shared tilt percentage, and stick left/right rotates the neck. The two tilt
servos may have different saved centers. In manual arm mode, the same stick
controls left and right arms:

| Manual arm mode input | Firmware behavior |
| --- | --- |
| Stick up/down on SBUS `CH2` | Maestro `CH3`, left arm |
| Stick left/right on SBUS `CH1` | Maestro `CH4`, right arm |

## Arm Mode Button Test

1. Open `http://192.168.4.1`.
2. Choose the `Button Assignments` tab.
3. Set one button to `Built-in sequence` and `Arm Mode toggle`.
4. Save button assignments.
5. Move the stick up/down; the two head tilt servos should move together.
   Move the stick left/right; Maestro `CH2` neck rotate should move.
6. Press the assigned arm-mode button once.
7. Move the stick up/down. The left arm should move.
8. Move the stick left/right. The right arm should move.
9. Press the assigned arm-mode button again.
10. Move the stick and confirm up/down returns to head tilt and left/right returns to neck rotate.

The head should hold its current pose while arm mode is active, and the arms
should hold their current pose after returning to head mode.

## Assignable Button Test

1. Open `http://192.168.4.1`.
2. Choose the `Button Assignments` tab.
3. Set `RC3 Button 3` to `Built-in sequence` and `Auto Mode toggle`.
4. Save button assignments.
5. Press the transmitter button assigned to SBUS `CH3`.
6. Confirm auto lifelike mode toggles on.
7. Press it again and confirm auto mode toggles off.

For a Maestro script test, set a button to `Maestro sequence`, enter sequence
`00`, save, then press the matching transmitter button. The Maestro should
restart its script at subroutine 0.

For a built-in animation test, set a button to `Built-in sequence` and
`Right Arm Wave`, save, then press the matching transmitter button. The right
arm should raise, wave a few times within its configured endpoints, then lower.

For the surprise animation, set a button to `Built-in sequence` and
`Surprise No`, save, then press the matching transmitter button. Both arms
should raise partway in a staggered motion, the head should rotate left/right a
few times, then the head and arms should return to center.

For a direct-control test, set one RC button to `Direct servo control`, choose
`Maestro CH2 - Neck rotate`, save, then move that transmitter channel. Maestro
CH2 should follow that RC channel within the saved neck endpoints.

## Failsafe Check

1. With servos moving normally, turn off the transmitter or disconnect SBUS.
2. Confirm all servos smoothly move toward their saved center positions.
3. Restore signal and confirm manual control resumes smoothly.
