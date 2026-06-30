# RC Remote Controls

This page describes the DS-650 six-channel layout used by the firmware. The
HOTRC SBUS-A receiver still sends a 16-channel SBUS frame, but the DS-650 remote
only drives six useful channels.

## Control Layout

| DS-650 control | SBUS channel | Firmware behavior |
| --- | --- | --- |
| Stick up/down | `CH2` | Head mode: both head tilt servos together. Arm mode: left arm. |
| Stick left/right | `CH1` | Head mode: neck rotate. Arm mode: right arm. |
| RC3 Button 3 | `CH3` | Configurable built-in, Maestro sequence, or direct servo control. |
| RC4 Button 4 | `CH4` | Configurable built-in, Maestro sequence, or direct servo control. |
| RC5 Button 5 | `CH5` | Configurable built-in, Maestro sequence, or direct servo control. |
| RC6 Button 6 | `CH6` | Configurable built-in, Maestro sequence, or direct servo control. |

In normal head mode, SBUS `CH2` drives both physical head tilt servos together.
Use each servo's `reversed` checkbox if one tilt servo needs to move the other
way. SBUS `CH1` drives Maestro `CH2` neck rotate for left/right head movement.

Head tilt uses paired mixing. The firmware converts stick up/down into one
shared tilt percentage, then applies that same percentage around each tilt
servo's own saved `center`, `min`, and `max`. This lets Maestro `CH0` and
`CH1` have different center values while still moving together.

## Manual Control Modes

Manual mode uses the same stick for head control or arm control.

| Manual sub-mode | Stick up/down | Stick left/right |
| --- | --- | --- |
| Head mode | Head tilt axes 1 and 2, Maestro `CH0` and `CH1` | Neck rotate, Maestro `CH2` |
| Arm mode | Left arm, Maestro `CH3` | Right arm, Maestro `CH4` |

To toggle between head mode and arm mode, assign one of the four buttons to the
built-in `Arm Mode toggle` action from the `Button Assignments` tab.

## Auto Mode

To toggle auto lifelike mode, assign one of the four buttons to the built-in
`Auto Mode toggle` action from the `Button Assignments` tab.

Auto mode ignores live stick input and generates smooth random puppet movement.
See [Auto Lifelike Mode](auto-mode.md).

## Assignable Buttons

The four DS-650 assignable buttons should output SBUS `CH3` through `CH6`.
Configure their behavior from the `Button Assignments` tab on the `Grogu Setup`
web page.

See [RC Button Assignments](button-assignments.md) for the full setup details.

## Firmware Mapping Constants

The private firmware source keeps the mapping near the top of `src/main.cpp`:

```cpp
constexpr uint8_t HEAD_TILT_SBUS_CHANNEL = 1;    // SBUS CH2: stick up/down drives both head tilt servos
constexpr uint8_t HEAD_ROTATE_SBUS_CHANNEL = 0;  // SBUS CH1: stick left/right drives neck rotate
constexpr uint8_t SEQUENCE_BUTTON_SBUS_CHANNELS[RC_SEQUENCE_BUTTON_COUNT] = {
    2,  // SBUS CH3
    3,  // SBUS CH4
    4,  // SBUS CH5
    5,  // SBUS CH6
};
```

These values are zero-based in code: `0` means SBUS `CH1`, `1` means SBUS
`CH2`, and so on.

## Setup Checklist

1. Assign stick vertical to SBUS `CH2`.
2. Assign stick horizontal to SBUS `CH1`.
3. Assign the four configurable buttons to SBUS `CH3` through `CH6`.
4. In `Grogu Setup`, assign one button to `Auto Mode toggle` if you want auto mode.
5. In `Grogu Setup`, assign one button to `Arm Mode toggle` if you want live arm control.
6. Use the web configuration page to reverse any servo whose motion is backward.
7. Keep initial servo limits narrow until the mechanical direction is confirmed.
