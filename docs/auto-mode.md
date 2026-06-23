# Auto Lifelike Mode

Auto lifelike mode makes the puppet move without live stick input. It is meant
to create small, smooth, idle-style motion rather than big scripted gestures.

## How To Enter Auto Mode

Assign one of the DS-650 buttons on SBUS `CH3` through `CH6` to the built-in
`Auto Mode toggle` action.

| Button action | Behavior |
| --- | --- |
| Press once | Auto lifelike mode toggles on. Live stick input is ignored. |
| Press again | Auto lifelike mode toggles off. Manual stick control resumes. |

Configure this from the `Button Assignments` tab on the `Grogu Setup` page.

## Expected Motion

In auto mode, the firmware generates smooth random target positions for the
five Maestro channels.

| Puppet part | Expected auto behavior |
| --- | --- |
| Head tilt axis 1 | Subtle random tilts around the saved center position. |
| Head tilt axis 2 | Subtle random tilts around the saved center position. |
| Neck rotate | Slow left/right turns with slightly wider range than head tilt. |
| Left arm | More active idle movement, independent raises, paired raises, and occasional wave-style gestures. |
| Right arm | More active idle movement, independent raises, paired raises, and occasional wave-style gestures. |

Auto movement is intentionally limited by each servo's saved `min`, `center`,
`max`, and `smoothing` settings. It should not snap suddenly or exceed the
configured limits.

## Timing

The motion timing is randomized:

- Head tilt targets change roughly every `1.0` to `2.8` seconds.
- Neck rotate targets change roughly every `1.0` to `3.2` seconds.
- Arm idle targets change roughly every `1.2` to `4.2` seconds.
- Additional arm gestures start every few seconds at random. These may raise one
  arm, raise both arms together, or make one arm wave with small repeated
  up/down movements.

Head tilt targets are weighted toward the saved center position. Most automatic
head tilts stay small, some are moderate, and only occasional targets use a
larger range so Grogu looks up or around every now and then instead of holding
that pose often.

Arm gestures use each arm servo's saved `center`, endpoints, and `reversed`
setting. They are still smoothed by firmware and clamped to the configured
servo limits.

## Returning To Manual Mode

When auto mode is toggled off, the firmware does not jump straight to the
current stick position. It blends from the current auto-generated position
toward the live RC stick target using the configured servo smoothing.

The manual sub-mode still applies after leaving auto mode:

| Manual sub-mode | Stick resumes control of |
| --- | --- |
| Head mode | Head tilt axes 1 and 2 from stick up/down, and neck rotate from stick left/right |
| Arm mode | Left arm and right arm |

An assignable button can be configured as `Arm Mode toggle` to switch between
head mode and arm mode after auto mode is off.


## Failsafe

If SBUS signal is lost during auto or manual mode:

1. Auto mode is disabled.
2. Each servo smoothly moves toward its saved center position.
3. Servo output remains clamped to the saved `min` and `max` settings.

## Tuning

Use the web configuration page at `http://192.168.4.1` while connected to
`Grogu Setup`.

| Setting | Effect in auto mode |
| --- | --- |
| `min` / `max` | Hard travel limits for random movement. |
| `center` | Neutral pose that auto movement is based around. |
| `reversed` | Affects manual stick direction, not auto randomness. |
| `speed` | Maestro servo speed limit. |
| `acceleration` | Maestro acceleration limit. |
| `smoothing` | Firmware blend amount; lower values make motion softer. |
