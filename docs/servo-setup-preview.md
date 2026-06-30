# Servo Setup Live Preview

Use the Node preview server to iterate on the setup page styling without
flashing the ESP32.

```powershell
npm run preview:servo
```

Then open:

```text
http://localhost:5177
```

Edit [`servo-setup-preview.html`](servo-setup-preview.html). The browser reloads
when the preview file changes. The preview includes the `System`, `Servos`,
`Calibrate`, `RC Input`, `Button Assignments`, `Combo Sequences`, and
`Control Setup` tabs. The Button Assignments preview includes the Auto Sounds
card, and Control Setup previews the collapsible Movements, Sounds, and Combo
Sequences categories.

After the design looks right, copy the matching CSS/HTML structure back into
the private firmware repo's `src/WebConfig.cpp`, then run:

```powershell
pio run
```
