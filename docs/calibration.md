# Servo Calibration

Firmware `0.1.3` adds a `Calibrate` tab to the `Grogu Setup` page.

## Open The Calibrate Tab

1. Connect to Wi-Fi AP `Grogu Setup`.
2. Open `http://192.168.4.1`.
3. Choose the `Calibrate` tab.

## What The Tab Does

Each Maestro channel has:

| Control | Purpose |
| --- | --- |
| Live Test slider | Moves the selected Maestro channel from the browser. |
| Test Target number | Exact target value to move to. |
| Go Min / Go Center / Go Max | Move to the currently shown endpoint values. |
| Use Current as Min / Center / Max | Copy the current test target into that endpoint field. |
| Save Calibration Endpoints | Save all shown min/center/max values to ESP32 flash. |

The live test range uses Maestro target units and is limited by channel. Head
tilt and neck channels stay inside `992` through `8800`. Arm and wrist servo
endpoints can use `3200` through `12000`, which corresponds to `800-3000 us`.
Move slowly, especially before linkages and servo horns are confirmed.

Live Test moves are allowed to use the firmware hard range so you can discover
new endpoints. Normal RC control, auto mode, and built-in sequences use the
saved per-servo endpoints after you click save.

## Reversed Servos

The firmware keeps the saved numeric `min` lower than the saved numeric `max`
internally for safety. If a servo has `reversed` enabled on the `Servos` tab,
the `Calibrate` tab flips the logical Min/Max actions:

| Calibration action | Reversed off | Reversed on |
| --- | --- | --- |
| `Go Min` | Moves to saved numeric `min`. | Moves to saved numeric `max`. |
| `Go Max` | Moves to saved numeric `max`. | Moves to saved numeric `min`. |
| `Use Current as Min` | Copies current target to saved numeric `min`. | Copies current target to saved numeric `max`. |
| `Use Current as Max` | Copies current target to saved numeric `max`. | Copies current target to saved numeric `min`. |

This makes calibration match the logical servo direction while still saving
safe ordered endpoints.

## Endpoint Workflow

1. Remove servo horns or disconnect linkages for the first pass.
2. Move a channel with the `Live Test` slider.
3. Stop before the mechanism binds.
4. Click `Use Current as Min`, `Use Current as Center`, or `Use Current as Max`.
5. Repeat for the other endpoints.
6. Click `Save Calibration Endpoints`.
7. Re-test using `Go Min`, `Go Center`, and `Go Max`.

The saved endpoints are used by live RC control, auto lifelike mode, and built-in
animations.

## Important

Browser test moves temporarily pause normal firmware servo target updates for a
few seconds so the Maestro can hold the test position while you inspect it.
After that, normal RC/failsafe behavior resumes.
