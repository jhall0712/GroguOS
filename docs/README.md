# GroguOS Documentation

This section collects the bench notes for wiring, configuration, upload, and
first tests for the XIAO ESP32-S3 puppet controller.

Current master firmware version: `0.4.4`

Current display firmware version: `Display 0.2.9`

Current stable rollback version: `0.4.0`

## Pages

- [Wiring Reference](wiring.md)
- [RC Remote Controls](rc-controls.md)
- [RC Input Calibration](rc-input-calibration.md)
- [RC Button Assignments](button-assignments.md)
- [Command Test](command-test.md)
- [Settings Backup](settings-backup.md)
- [Combo Sequences](combo-sequences.md)
- [Auto Lifelike Mode](auto-mode.md)
- [Servo Calibration](calibration.md)
- [Servo Setup Live Preview](servo-setup-preview.md)
- [DFPlayer Mini Sound](sound.md)
- [ESP-NOW Display Remote](remote-screen.md)
- [First Upload and Test Procedure](first-upload-test.md)
- [Cloudflare Web Flash Setup](cloudflare-webflash.md)
- [Future Features](future-features.md)
- [Changelog](../CHANGELOG.md)

## Firmware Defaults

- SBUS receiver input: XIAO `D7` / `GPIO44`
- Maestro serial output: XIAO `D6` / `GPIO43`
- Maestro baud rate: `9600`
- Master setup AP: `Grogu Setup`
- Master setup AP password: optional
- Master setup page: `http://192.168.4.1`
- Display setup AP: `GroguOS Display`
- Display setup page: `http://192.168.5.1`

## Current GroguOS Features

- Home landing screen with public Control and protected Settings paths.
- Configurable setup AP name and optional setup AP password.
- Password-protected Settings access when a settings password is configured.
- Public Control screen with movement, sound, and combo buttons.
- Mobile landscape Control mode with left/right on-screen joysticks.
- Live RC controls with paired head tilt support and reversed-servo handling.
- Fixed random sound categories for `Cute`, `Phrases`, `Sentences`,
  `Burps and Slurps`, and `Sound Combos`.
- Settings export/import for backing up servo, button, control, combo, sound,
  network, and access configuration.
- OTA update checks, current-version reinstall, and rollback from the protected
  System page when Grogu is connected to Home Wi-Fi.
- Auto Mode random sound settings on the Button Assignments page.
- Collapsible Control Setup categories for public movement, sound, and combo
  buttons.

Pin defaults are summarized here and live in the private firmware source.
