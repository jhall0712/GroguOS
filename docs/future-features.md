# Future Features

## Republic Link ESP-NOW Control

Add an optional Republic Link layer so Republic Command or another paired droid
can trigger Grogu over secure ESP-NOW when nearby.

Suggested scope:

- Use ESP-NOW for cue-style commands only, not live servo control.
- Allow sounds, built-in sequences, Maestro sequences, and possibly combo
  sequences.
- Do not allow raw servo target moves, calibration changes, setup changes, or
  unrestricted command execution.
- Add a setup page section for enabling Republic Link, pairing controllers, and
  choosing allowed command categories per paired device.
- Store paired device MAC addresses, friendly names, secure ESP-NOW keys, and a
  permission mask in saved settings.
- Keep packets compact and typed rather than JSON, for example command type,
  action/index, value, holdoff, and nonce.
- Account for ESP-NOW and Wi-Fi channel sharing. Republic Command and Grogu need
  to agree on a radio channel, especially when Grogu is also connected to home
  Wi-Fi.

Potential command permissions:

- Sounds: play track, random category, stop sound, set volume.
- Built-in sequences: auto mode, arm mode, right arm wave, surprise no.
- Maestro sequences: run a saved Maestro sequence with holdoff.
- Combo sequences: trigger saved Grogu combo slots.

This should be treated as a local, low-latency show-control link. For remote
control across different networks, use a future cloud relay or MQTT-style
command bus instead of exposing Grogu directly to the internet.
