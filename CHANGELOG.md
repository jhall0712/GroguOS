# Changelog

## Display 0.2.9

- Added random sound category buttons for `Cute`, `Phrases`, `Sentences`,
  `Burps and Slurps`, and `Sound Combos`.
- Added setup-page support for choosing either a specific sound track or a
  random sound category.
- Hid category fields when `Play Track` is selected, and hid track fields when
  `Random Category` is selected.
- Added remove buttons for display menu commands across Built-Ins, Maestro,
  Sounds, and Combos.
- Rebranded the display setup page and default display setup AP to `GroguOS`.

## 0.3.1

- Added fixed random sound categories for `Cute`, `Phrases`, `Sentences`,
  `Burps and Slurps`, and `Sound Combos`.
- Added random-category sound buttons to the Control Setup and public Control
  screen.
- Added random-category support to RC Button Assignments sound actions.
- Kept specific-track sound actions available and disabled category selection
  when specific track mode is selected.
- Updated settings export/import and local/web previews for the new sound
  category fields.

## 0.3.0

- Added mobile landscape control mode with left and right on-screen joysticks.
- Added a compact landscape-mode toggle to the public control page header.
- Mapped the left joystick to paired head tilt and neck rotate controls.
- Mapped the right joystick to arms and elbows, with selectors for left, right,
  or both sides.
- Kept joystick positions where released instead of snapping back to center.
- Tightened the home screen layout for small ESP32 browser viewports and added
  the Grogu logo asset.
- Rebranded the home and settings pages to `GroguOS`.
- Added webflash-hosted previews for Home, Settings, Control, and Display pages.

## 0.2.9

- Added a setup AP landing screen with separate Control and Settings paths.
- Added configurable setup access point name and optional password support.
- Added password-protected Settings access.
- Reworked the settings page with `System` first, network settings first, and
  command/sound tests folded into Button Assignments.
- Added public Control screen configuration for movement, sound, and combo
  buttons.
- Added Live RC controls with servo toggles and a shared slider.
- Paired the two head tilt servos under one Live RC control while honoring each
  servo's reversed setting.
- Removed `Servo` from left/right elbow labels across the UI.

## 0.2.8

- Added independent combo `Wait After ms` timing so multiple combo actions can
  start together by setting the wait to `0`.
- Kept Maestro combo holdoff as a separate `Maestro Holdoff ms` field so Maestro
  scripts can still own the servos without forcing combo pacing.
- Added combo sequence documentation with examples for sounds, Maestro scripts,
  delays, volume changes, display triggers, and RC button assignments.
- Published Master and Display as `0.2.8`, removed the `0.2.5` rollback entry,
  and removed the beta tag from the Display firmware entry.

## 0.2.7

- Added display battery indicator support for the round touch display.
- Added configurable display sleep timing with tap-to-wake and deep sleep after extended idle sleep.
- Updated display-triggered Maestro sequences to use the Master firmware's saved holdoff timing.
- Added RC button actions for sound playback and combo sequence triggers.

## 0.2.6

- Published `0.2.6 Master` as the current master firmware and kept `0.2.5` as the rollback option.
- Published the round display firmware as `Display 0.2.6`.
- Added ESP-NOW display support, configurable display menus, and display connection setup at `http://192.168.5.1`.
- Added combo sequence editing improvements: add/delete combos, add/remove steps, sound steps, and cleaner step-specific fields.
- Updated webflash publishing so the current master, display, and rollback firmware entries stay together.

## 0.2.5

- Added more visible auto-mode wrist movements tied to the paired arm movements and raised-arm pauses.
- Restored reversed calibration endpoint behavior so logical Min/Max actions flip correctly for reversed servos.
- Kept web test moves synced with firmware targets so calibration moves do not snap back after the web hold expires.

## 0.2.4

- Added DFPlayer Mini sound support with a setup-page Sound Test tab.
- Added webflash entries and firmware output for the 0.2.4 release.
- Fixed setup tab routing so lowercase URLs like `/?tab=calibrate`, `/?tab=buttons`, and `/?tab=sound` load and save through the correct handlers.

## 0.2.3

- Added Left Wrist Servo and Right Wrist Servo setup entries for the new forearm/wrist joints.
- Kept the new wrist servos out of built-in animations for now.
- Reset CH3/CH4 arm minimum endpoint back to `3200` Maestro units, matching `800 us`.
- Expanded CH3/CH4 arm maximum endpoint to `12000` Maestro units, matching `3000 us`.
- Kept CH0/CH1 head tilt and CH2 neck rotate capped at `992` through `8800`.

## 0.2.2

- Expanded CH3/CH4 arm calibration to `2000` through `10000` Maestro units, matching `500-2500 us`.
- Kept CH0/CH1 head tilt and CH2 neck rotate capped at the previous safer `992` through `8800` range.
- Updated the setup page, calibration page, settings import, and live test command to use channel-specific endpoint limits.

## 0.2.1

- Expanded the Maestro target ceiling to `8800` quarter-microseconds.
- Updated left and right arm default endpoints to `3200` min and `8800` max, matching `800-2200 us`.
- Updated setup page target inputs and calibration ranges to allow the new upper endpoint.

## 0.2.0

- Added normalized paired head tilt mixing for Maestro CH0 and CH1.
- Stick up/down now produces one shared tilt percentage, then applies it around each tilt servo's own saved center and endpoint range.
- Different CH0 and CH1 center values are supported while keeping paired tilt movement synchronized.
- The `reversed` checkbox still controls each tilt servo's physical direction.
- Auto mode arms now move more often, including independent raises, both-arm raises, and occasional wave-style gestures.

## 0.1.9

- Added RC input calibration for SBUS CH1 through CH6.
- Added the `RC Input` setup tab with live channel values and Min/Center/Max capture buttons.
- Live RC mapping now uses saved RC center and deadband so transmitter neutral maps to each servo's saved center.
- Manual head mode now maps stick up/down to both head tilt servos and stick left/right to neck rotate.
- Swapped the observed DS-650 stick axes so SBUS CH2 drives head tilt and SBUS CH1 drives neck rotate.
- Added RC input calibration values to settings export/import.
- Added RC input calibration docs and updated the local setup-page preview.

## 0.1.8

- Web-triggered Auto Mode now stays latched until toggled off instead of timing out after the temporary command hold.
- Added a final Maestro output clamp so normal RC, auto, and built-in motion cannot exceed saved servo endpoints.
- Normalized saved servo settings in memory immediately when saving.
- Added Maestro `Stop Script` handling when Maestro sequence holdoff expires or failsafe takes over.
- Added RC button press/release hysteresis to prevent noisy button channels from repeatedly retriggering Maestro sequences.

## 0.1.7

- Started next development version.
- Built-in neck shake movement now honors the neck rotate servo's `reversed` setting.
- Calibration Min/Max actions now flip for reversed servos so logical endpoints match servo direction.
- Softened `Surprise No` with staggered partial arm raises and smaller neck turns to reduce servo current spikes.
- Added an extra final clamp so firmware-controlled motion cannot exceed saved per-servo endpoints.
- Tuned auto mode head tilt to stay near center most of the time, with only occasional larger looks.
- Added setup-page settings export/import for backing up and restoring servo/button configuration JSON.

## 0.1.6

- Added direct servo control as a Button Assignments action.
- RC3 through RC6 can now map continuously to Maestro CH2, CH3, or CH4.
- Kept Maestro CH0 and CH1 unavailable for direct button control to protect tank head tilt.

## 0.1.5

- Remapped normal/head mode for DS-650 tank control.
- SBUS CH1 now drives Maestro CH0 head tilt axis 1.
- SBUS CH2 now drives Maestro CH1 head tilt axis 2.
- Neck rotate no longer uses the live stick in normal/head mode.

## 0.1.4

- Added setup-page Command Test tab.
- Added browser-triggered built-in sequence testing.
- Added browser-triggered Maestro sequence testing with configurable holdoff.

## 0.1.3

- Added setup-page calibration tab with live Maestro channel movement.
- Added per-channel test slider/number controls and endpoint capture buttons.

## 0.1.2

- Started next development version.
- Added configurable RC button assignments for four HOTRC assignable buttons.
- Added Button Assignments setup tab with built-in Auto Mode toggle or Maestro sequence actions.
- Added Maestro `Restart Script at Subroutine` compact serial command support.
- Remapped DS-650 controls to six channels: stick on SBUS CH1/CH2 and buttons on CH3-CH6.
- Added built-in `Right Arm Wave` assignable button animation.
- Added built-in `Surprise No` assignable button animation.

## 0.1.1

- Renamed the setup Wi-Fi AP to `Grogu Setup`.
- Restyled the on-device servo setup page with one card per Maestro channel.
- Added Button B manual arm-mode toggle.
- Added multi-version Cloudflare Pages web flashing support.
- Added RC controls, auto mode, wiring, upload/test, and webflash docs.

## 0.1.0

- Initial XIAO ESP32-S3 puppet controller firmware.
- SBUS input decoding.
- Pololu Maestro compact serial output.
- Manual mode, auto lifelike mode, failsafe centering, NVS settings, and setup AP.
