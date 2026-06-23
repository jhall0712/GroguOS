# Command Test Tab

Firmware `0.1.4` adds a `Command Test` tab to the `Grogu Setup` web page.

Connect to Wi-Fi AP `Grogu Setup`, open `http://192.168.4.1`, then choose
`Command Test`.

## Built-In Sequence Test

Use the built-in sequence dropdown to send one of the same commands available
on the `Button Assignments` tab:

| Built-in sequence | Behavior |
| --- | --- |
| `Auto Mode toggle` | Toggles firmware auto lifelike mode on/off. |
| `Arm Mode toggle` | Toggles the live stick between head/neck control and arm control. |
| `Right Arm Wave` | Raises the right arm, waves it a few times, then lowers it. |
| `Surprise No` | Raises both arms partway in a staggered surprise pose, shakes the head left/right like "no", then lowers arms and centers the head. |

Browser-triggered `Auto Mode toggle` stays latched until you send `Auto Mode
toggle` again. This lets you test lifelike motion from the setup page even if
the RC signal is missing.

The one-shot built-in commands use a temporary hold so you can test them from
the setup page. If the RC signal is missing, the firmware returns to normal
failsafe centering after that hold expires.

## Maestro Sequence Test

Use the Maestro sequence box to send the compact serial `Restart Script at
Subroutine` command:

```text
0xA7, subroutine_number
```

Enter sequence numbers as `00`, `01`, `02`, etc. These map to Maestro
subroutine numbers `0`, `1`, `2`, and so on.

`Holdoff ms` pauses normal ESP32 servo target updates while the Maestro script
runs. When holdoff expires, the ESP32 sends Maestro `Stop Script` and resumes
normal servo control. Set holdoff long enough to see the Maestro script finish.
