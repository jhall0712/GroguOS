# Combo Sequences

Combo sequences are saved on the Master firmware. They can be started from the
Master setup page, from the Display combo menu, or from an RC button assignment.

## Timing Model

Each step has two separate timing ideas:

- `Wait After ms` controls when the next combo step starts.
- `Maestro Holdoff ms` controls how long a Maestro script owns the servos before
  normal Master control resumes.

Set `Wait After ms` to `0` when the next action should start immediately. This
lets a sound, built-in action, Maestro sequence, delay, or volume change launch
without waiting for the previous step to finish.

For old saved combos, `Wait After ms` defaults to the previous holdoff value so
existing sequences keep their old pacing until you edit them.

## Setup Steps

1. Connect to `Grogu Setup` and open `http://192.168.4.1`.
2. Open the `Combos` tab.
3. Click `Add Combo` if you need another combo card.
4. Name the combo and enable `Show On Display` if the Display should list it.
5. Click `Add Step` for each action in the sequence.
6. Choose each step type and fill only the fields shown for that type.
7. Use `Wait After ms` to decide when the next step starts.
8. Click `Save Combo Sequences`.
9. Use `Test Combo` to run the selected combo from the setup page.

## Examples

### Play Sound And Start A Maestro Sequence Together

| Step | Type | Value | Maestro Holdoff ms | Wait After ms |
| --- | --- | --- | --- | --- |
| 1 | Sound Track | Track `1`, Volume `25` | unused | `0` |
| 2 | Maestro Sequence | Sequence `3` | `6000` | `0` |

The sound command is sent first. Because step 1 has `Wait After ms` set to `0`,
the Maestro sequence starts on the next combo tick instead of waiting for the
sound to finish.

### Start Sound, Then Move After A Short Pause

| Step | Type | Value | Maestro Holdoff ms | Wait After ms |
| --- | --- | --- | --- | --- |
| 1 | Sound Track | Track `2`, Volume `25` | unused | `500` |
| 2 | Built-in Sequence | Right Arm Wave | unused | `0` |

The sound starts, then the arm wave starts about half a second later.

### Run Two Maestro Scripts With A Gap

| Step | Type | Value | Maestro Holdoff ms | Wait After ms |
| --- | --- | --- | --- | --- |
| 1 | Maestro Sequence | Sequence `4` | `3000` | `3500` |
| 2 | Maestro Sequence | Sequence `5` | `2500` | `0` |

The first Maestro script owns the servos for three seconds. The combo waits 3.5
seconds before starting the next Maestro script.

### Change Volume And Play Immediately

| Step | Type | Value | Maestro Holdoff ms | Wait After ms |
| --- | --- | --- | --- | --- |
| 1 | Sound Volume | Volume `18` | unused | `0` |
| 2 | Sound Track | Track `4`, Volume `18` | unused | `0` |

This is useful when one combo should play quieter than the normal sound menu.

### Add A Timed Pause

| Step | Type | Value | Maestro Holdoff ms | Wait After ms |
| --- | --- | --- | --- | --- |
| 1 | Built-in Sequence | Surprise No | unused | `1500` |
| 2 | Delay | unused | unused | `2000` |
| 3 | Sound Track | Track `3`, Volume `25` | unused | `0` |

The delay step is just a spacer. Use it when the previous action should not own
the full timing by itself.

## RC Button Assignment

Open the `Buttons` tab and set an RC button action to `Combo`. Pick the combo
slot you want that button to trigger, then save. The selected RC button starts
the same saved combo sequence used by the setup page and Display.

## Practical Notes

- Use `Wait After ms = 0` for actions that should start together.
- Use a nonzero `Wait After ms` when you need choreography or a deliberate gap.
- For Maestro steps, keep `Maestro Holdoff ms` long enough for the Maestro
  script to finish or for normal Master control to stay out of the way.
- Sound steps are command-only; the Master does not wait for the DFPlayer to
  report track completion.
