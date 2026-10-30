# Expressive Cutout, hardened fork

A dynamic island for the camera cutout of an Android phone, built to be trusted on a
privacy-focused daily phone. This fork is maintained by Inshal Rauf and is tuned for a
Pixel 10 Pro running GrapheneOS. It is a fork of [Expressive Cutout](https://github.com/EvanKoe/expressive-cutout)
by Evan Koehler, licensed under the GPLv3, and it tracks that project's `dev` branch.

The island mirrors notifications, now-playing music, calls and timers into a pill drawn
over the camera hole, styled with Material Expressive shapes and Material You colours.
The pill is true OLED black, so its pixels are off and it reads as part of the hole.

## What this fork changes

Security and privacy:

- The accessibility service that hosts the overlay can no longer read window content.
  `canRetrieveWindowContent` is false and the service subscribes to window-state events
  only, so the most it ever learns is which package is in the foreground.
- The assistant tile, the one feature that needed to read screen text, is removed.
- `READ_PHONE_STATE` is removed from the manifest. The phone tile still closes when the
  dialer's notification goes away.
- `allowBackup` is false, so settings never leave the app sandbox through a backup.
- The app still declares no `INTERNET` permission and contains no analytics, crash or
  ads SDK. You can confirm this on the built APK with `aapt dump permissions`.

Fixes and defaults:

- The colour picker's "black" preset is now `#000000`. Upstream's was `#0A0A0A`, which
  stays lit on an OLED panel and never matches the camera hole.
- The Shows-when-empty page carries its own on/off switch for the resting pill.
- A fresh install defaults to a black, shadow-free resting pill that is always shown.

Removed from the fork: the upstream author's profile, donation and community links, the
funding file, and the author photo. Attribution to the original project stays in the app,
in this file and in the licence.

## Permissions the app asks for

| Access | Used for |
| --- | --- |
| Notification access | Mirroring notifications, media and calls onto the island. Content is kept in memory only. |
| Accessibility service | Hosting the overlay window without the draw-over-apps permission. Foreground package name only. |
| Vibrate, post notifications, network state, battery-optimisation request | Haptics, the in-app test notification, the Wi-Fi event, background reliability. All normal-level. |
| Shizuku (optional, separate app) | Hiding status bar icons and the privacy dots. Not recommended on a locked-down phone; everything else works without it. |

## Building

The fork is meant to be built from source and signed with your own key.

```bash
./gradlew assembleRelease
# then zipalign and apksigner with your keystore, and
adb install -r --user 0 app/build/outputs/apk/release/<signed>.apk
```

Requires JDK 17 or 21 and Android SDK platform 35. On GrapheneOS, enable notification
access and the accessibility service by hand after installing. If Android shows
"Restricted setting", allow it from the app's info page first.

## Pixel 10 Pro geometry

With the display at 1080x2410 and 356 dpi, the camera hole is centred horizontally with
its centre 86 px from the top and a radius of 41 px, that is 20 to 57 dp. A collapsed pill
of 44 dp with its top at 17 dp wraps it with about 3.5 dp of black on each side. Settings
can be imported from a JSON file through the Profile tab.

## Keeping up with upstream

A GitHub Action in `.github/workflows/upstream-sync.yml` runs daily. When the upstream
`dev` branch moves, it merges it into `hardened` on a throwaway branch, refuses the merge
if `INTERNET`, `READ_PHONE_STATE`, backup or window-content reading reappear, builds, and
opens a pull request for review. Nothing is signed or installed automatically.

## Licence

GNU General Public License v3.0, see [LICENSE](LICENSE). Original work copyright
Evan Koehler. Modifications copyright Inshal Rauf. Any distributed derivative must
remain under the same licence.
