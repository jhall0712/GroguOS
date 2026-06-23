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
when the preview file changes. The preview includes the `Servos`, `Calibrate`,
`RC Input`, `Button Assignments`, `Command Test`, and `Settings Backup` tabs.

After the design looks right, copy the matching CSS/HTML structure back into
the firmware web configuration source, then run:

```powershell
pio run
```
