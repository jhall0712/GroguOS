# Cloudflare Web Flash Setup

The private firmware repo's `webflash/` folder is a static ESP Web Tools
installer that can be hosted on Cloudflare Pages.

Current master firmware version: `0.4.4`

Current display firmware version: `Display 0.2.9`

Current stable rollback point: `0.4.0`

## How It Works

- `webflash/index.html` loads ESP Web Tools and
  shows one browser install button per published firmware version.
- `webflash/versions.json` lists the available
  master, display, and rollback firmware options shown on the page.
- `webflash/manifest-<version>.json` files declare each XIAO ESP32-S3 firmware
  image.
- `webflash/manifest.json` points at the latest firmware image.
- `pio run` copies PlatformIO's combined factory image and app-only OTA image
  to versioned files under `webflash/firmware/`.
- `webflash/_headers` adds Cloudflare Pages headers for the manifest and
  firmware binary.

## Local Build

```powershell
pio run
```

After a successful build, confirm this file exists:

```text
webflash/firmware/grogu-xiao-esp32s3-0.2.8.factory.bin
webflash/firmware/grogu-xiao-esp32s3-latest.factory.bin
webflash/firmware/grogu-xiao-esp32s3-0.2.8.ota.bin
webflash/firmware/grogu-xiao-esp32s3-latest.ota.bin
```

Factory binaries are for first-time USB installs through ESP Web Tools. OTA
binaries are app-only images used by the authenticated System page update card
after Grogu is connected to Home Wi-Fi.

The version number comes from `custom_webflash_version` in the private firmware
repo's `platformio.ini`.

## Localhost Preview

Serve the web flasher locally from the `webflash/` folder:

```powershell
npm run preview:webflash
```

Then open either build UI preview:

```text
http://localhost:5178/preview/master
http://localhost:5178/preview/display
```

Or open either flasher preset URL in Chrome or Edge:

```text
http://localhost:5178/?firmware=master
http://localhost:5178/?firmware=display
```

The localhost preview uses the same `versions.json`, manifest files, and
firmware binaries as the Cloudflare Pages site. Web Serial is allowed on
`localhost`, so the install buttons can be tested without publishing first.

## Cloudflare Pages

Use these Cloudflare Pages settings:

| Setting | Value |
| --- | --- |
| Framework preset | None |
| Build command | Leave blank |
| Build output directory | `webflash` |
| Root directory | repository root |

Cloudflare Pages serves the site over HTTPS, which is required for Web Serial in
Chrome and Edge.

Stable production flasher URL:

```text
https://animated-grogu.pages.dev/
```

Cloudflare may also show deploy-specific preview URLs such as
`https://70e5b15c.animated-grogu.pages.dev/`. Those are normal preview links for
individual deployments; use the stable URL above for the public web flasher.

## Publishing Options

For a Git-connected Pages site, commit the generated firmware binary in
`webflash/firmware/` whenever you want to publish a new flashable build.

For a direct upload Pages site, run `pio run`, then upload the `webflash/`
directory.

## Direct Sync With Wrangler

You can deploy the private repo's local `webflash/` folder to Cloudflare Pages
without connecting GitHub by using Wrangler direct upload.

First, create a Cloudflare Pages project in the Cloudflare dashboard. Then set
the project name in PowerShell:

```powershell
$env:CLOUDFLARE_PAGES_PROJECT='animated-grogu'
```

Log in once:

```powershell
npx wrangler login
```

Build firmware and deploy the webflash folder:

```powershell
npm run deploy:webflash
```

If you already ran `pio run` and only want to upload the existing `webflash/`
folder:

```powershell
npm run deploy:webflash:only
```

For unattended deploys, create a Cloudflare API token and set it as an
environment variable before running the same deploy command:

```powershell
$env:CLOUDFLARE_API_TOKEN='your-api-token'
$env:CLOUDFLARE_PAGES_PROJECT='animated-grogu'
npm run deploy:webflash
```

Keep `webflash/firmware/*.factory.bin`, `webflash/manifest-*.json`, and
`webflash/versions.json` together so rollback buttons continue to work.

## On-Device OTA Updates

From Grogu's protected Settings area, open `System`, connect Grogu to Home
Wi-Fi, then use the OTA card:

1. Click `Check For Update`.
2. If a newer Cloudflare release is listed, click `Install Update`.
3. Use `Reinstall Current Version` to reload the same firmware build.
4. Use `Rollback` to install an older OTA-capable release from the rollback
   selector.

The card checks `https://animated-grogu.pages.dev/versions.json`, hides the raw
firmware URL, and refreshes the settings page after a successful OTA response so
the browser reconnects after the ESP32 reboots.

## Publishing Multiple Versions

Each published version needs:

```text
webflash/firmware/grogu-xiao-esp32s3-<version>.factory.bin
webflash/firmware/grogu-xiao-esp32s3-<version>.ota.bin
webflash/manifest-<version>.json
```

The display firmware is published as a separate entry:

```text
webflash/firmware/grogu-esp32s3-display-0.2.9.factory.bin
webflash/manifest-display-0.2.9.json
```

The web page reads `webflash/versions.json` and shows every version listed there
as a separate install button. The first entry is marked recommended.

To publish a new version:

1. Change `custom_webflash_version` in the private firmware repo's
   `platformio.ini`,
   for example from `0.2.7` to `0.2.8`.
2. Run `pio run`.
3. Commit or upload the updated `webflash/` folder.
4. Keep the selected rollback `webflash/firmware/*.factory.bin` and
   `webflash/firmware/*.ota.bin` files plus `webflash/manifest-*.json` files in
   place so rollback and reinstall buttons continue to work.

Do not rename or delete an older firmware file unless you also remove its entry
from `versions.json`.

## Test The Published Flasher

1. Open `https://animated-grogu.pages.dev/` in Chrome or Edge.
2. Connect the XIAO ESP32-S3 by USB.
3. Choose a firmware version and click its install button.
4. Select the XIAO serial port.
5. If the port is not visible, hold `BOOT`, tap `RESET`, then release `BOOT`.
6. After flashing `0.2.8 Master`, connect to Wi-Fi AP `Grogu Setup`.
7. Open `http://192.168.4.1` and set safe servo limits before powering servos.
8. After flashing `Display 0.2.9`, connect to Wi-Fi AP `GroguOS Display` and
   open `http://192.168.5.1` to configure display menus and puppet connection
   settings.

## Notes

- The flasher intentionally uses combined factory binaries at offset `0x0`, so
  each manifest only needs one firmware part.
- The web page depends on ESP Web Tools from `unpkg.com`.
- Keep external servo power disconnected during firmware flashing.
