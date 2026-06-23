# Settings Backup

Firmware `0.1.7` adds a `Settings Backup` tab to the `Grogu Setup` web page.

Connect to Wi-Fi AP `Grogu Setup`, open `http://192.168.4.1`, then choose
`Settings Backup`.

## Export

Click `Download Settings JSON` before flashing, experimenting, or making large
configuration changes.

The export includes:

- Servo `min`, `center`, `max`
- Servo `reversed`
- Servo `speed`, `acceleration`, and `smoothing`
- RC button action assignments
- Built-in sequence choices
- Maestro sequence numbers and holdoff times
- Direct servo control channel choices

## Import

Paste a previously exported JSON backup into the import box and click
`Import Settings JSON`.

Import overwrites the current saved settings in ESP32 flash, reapplies Maestro
speed/acceleration settings, and returns to the `Settings Backup` tab.

The importer accepts the `grogu-settings-v1` export format and clamps imported
values to the firmware's safe ranges.
