# PerfectDarkVR-SteamFrame
A conversion of [https://github.com/Alex-LeTux](https://github.com/Alex-LeTux/perfect_dark_VR)'s Perfect Dark for the Steam Frame

(Written by ChatGPT)

# Perfect Dark PCVR for Steam Frame

An experimental community mod that runs the Windows version of **Perfect Dark PCVR v1.9.5-beta locally on Steam Frame through Proton**.

This repository contains the Steam Frame source patch, Windows installer source, original project icon, build instructions, and third-party notices. The game ROM is supplied separately by the user. This project is not affiliated with Rare, Microsoft, Nintendo, or Valve.

## Status

**Experimental / pre-release.** Stereo rendering, menu output, audio, refresh-rate switching, installation, Steam shortcut creation, and preservation of saves during updates have been tested on one Steam Frame. Full campaign completion, multiplayer, all controller interactions, and long sessions have not been verified.

Resolution changes are undergoing additional testing following a Wine OpenXR assertion and freeze. Do not describe an unverified installer as a stable release. See [known issues](#known-issues) and the release's matching validation report.

## Features

- Windows PCVR gameplay running locally through Proton.
- A D3D11 OpenXR presentation bridge for the existing OpenGL renderer.
- VR menu and blurred-background compatibility fixes.
- Per-eye render resolution presets in **Virtual Reality → Display & HUD**.
- **Off / FXAA** anti-aliasing options. SMAA is not implemented.
- Refresh-rate choices reported by the headset, up to **144 Hz**. The tested headset reports 72, 80, 90, 96, 108, 120, and 144 Hz; 144 Hz is experimental.
- An actual headset refresh-rate label and a rolling five-second average game FPS display. Display Hz and rendered FPS measure different things.
- A Windows setup wizard that transfers the program over SSH and adds **Perfect Dark PCVR** to Steam.
- Updates preserve game saves, display settings, and the Proton prefix, with backups of replaced program files.

## What you need

- A Steam Frame with SteamOS, SteamVR, and Steam Home running.
- **Proton 11.0 (ARM64)** and **Steam Linux Runtime 4.0 (ARM64)** installed through Steam.
- A Windows 10/11 x64 computer on the same network as the headset.
- SSH enabled on the Frame, its IP address or hostname, and the `steamos` account's SSH password.
- Your own **Perfect Dark US NTSC v1.1 ROM**, in big-endian `.z64` format, exactly 32 MiB.
- At least 768 MiB of free headset storage for the installer's initial check; allow more space for the Proton prefix and backups.

The installer checks the ROM's size and header and verifies transferred file hashes. It does not download a ROM or convert ROM byte order.

## Install

1. Download the installer from a release marked as tested. Check its published SHA-256 checksum.
2. Keep Steam Home and SteamVR open on the Frame. Close Perfect Dark before installing or updating.
3. Run the Windows installer and select your `.z64` ROM.
4. Enter the Frame's address, SSH username `steamos`, and password. Verify the SSH fingerprint on the first connection.
5. Choose **Install on Frame**. Launch **Perfect Dark PCVR** from the headset's Steam library.

No separate Python installation or Windows administrator rights are required for a packaged installer. The game installs to `~/Games/PerfectDark-PCVR`. SSH host keys are saved on the PC under `%LOCALAPPDATA%\PerfectDark-Frame-Setup\known_hosts`; the password is not saved.

If shortcut registration fails, keep Steam Home open and choose **Add to Steam again**. The installer verifies the shortcut and reuses an existing matching entry.

## Display and performance

Open **Virtual Reality → Display & HUD** to change render resolution, anti-aliasing, refresh rate, and the FPS display.

New installs start at **916 × 916 per eye**, anti-aliasing **Off**, a five-second average FPS display, and a 72 Hz per-game refresh preference. Existing settings take precedence during updates. Refresh preferences apply to this Steam application, rather than every VR game.

Start with the defaults. Higher scene resolution and FXAA can increase frame time. A 144 Hz display setting does not guarantee 144 rendered FPS. The current bridge reads image data back through the CPU and uploads it to D3D11; this presentation path is a performance bottleneck. During earlier menu/render tests, approximately 48–54 FPS was observed at 1080 × 1080 with AA Off. This is not a campaign benchmark or a performance guarantee.

## Saves and updates

The installer preserves `pd.ini`, `pd-vr.ini`, `eeprom.bin`, and `prefix/` in the owned installation. Replaced program files are backed up under `.setup-backups/`.

Back up your saves before testing development builds. An existing destination without this installer's ownership marker is left untouched and setup stops. Earlier test installations are separate; their saves are not automatically imported.

## Known issues

- Resolution changes have triggered a Wine OpenXR assertion or freeze in development builds. A fix is being validated; restarting the game at a chosen saved resolution remains part of the recovery/testing workflow.
- The CPU image-transfer bridge limits performance, particularly at high resolution.
- Full missions, multiplayer, long-session stability, and complete controller behavior need further testing.
- The installer expects the tested Steam Frame account and ARM64 runtime layout. Other Linux headsets and ordinary x64 Linux PCs are not validated targets.
- The experimental installer is unsigned. Verify its source and checksum before running it.

## Build and contribute

See [BUILDING.md](docs/BUILDING.md) for source preparation, the Windows game build, and installer packaging. See [RELEASING.md](docs/RELEASING.md) for the GitHub upload and release checklist.

For bug reports, include the mod version, SteamOS/SteamVR/Proton versions, render resolution, AA setting, refresh setting, steps to reproduce, and relevant portions of `vr_debug.txt`. Remove usernames, addresses, credentials, and other private information before attaching logs. Never attach ROMs or save files.

Credits and license

Based on [Alex-LeTux/perfect_dark_VR v1.9.5-beta](https://github.com/Alex-LeTux/perfect_dark_VR/tree/v1.9.5-beta), upstream commit prefix `f080291`, and the Perfect Dark PC port/decompilation contributors. The upstream source includes Ryan Dwyer's MIT notice.

The Steam shortcut helper is adapted from the supplied Halo Steam Frame Installer 1.4.0 under its MIT license. Valve OpenVR and Khronos OpenXR provide the runtime interfaces. Original copyright and dependency notices are retained.

Original installer code, project icon, and source changes are distributed under the [MIT license](LICENSE). Third-party components retain their own licenses; see [THIRD_PARTY.md](THIRD_PARTY.md) and `licenses/`.

The software license does not grant rights to the original Perfect Dark ROM, game assets, or trademarks. They are not supplied by this repository or installer. The installer uses an original geometric PD project mark rather than original game artwork.
