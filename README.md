# Snap Vanish 🫰 — Thanos snap for video calls (v1.1.1)

Works on **Google Meet, Microsoft Teams (web), Zoom (web client) and OmeTV**.

1. Start your camera in a call. Hold your hand up in frame — glowing skeleton lines
   appear over your hand on your own feed, so you can see tracking is live.
2. Do a **real finger snap**. You dissolve into dust particles, the room stays
   behind you, and your mic mutes.
3. Snap again — the dust flies back, you reappear, and your mic unmutes.

## Install (2 minutes, Load unpacked)

1. Unzip `snap-vanish.zip`.
2. Chrome/Edge extensions page: Chrome → `chrome://extensions`, Edge → `edge://extensions`
3. Turn ON **Developer mode** (top right).
4. **Load unpacked** → select the `snap-vanish` folder (the folder that directly
   contains `manifest.json`).
5. **Reload your call tab** (Meet / Teams / Zoom / OmeTV) — the extension must load
   before the site asks for your camera. Join/start the call after reloading.

Updating: remove the old version first (or replace the folder's contents and press
the reload ↻ button on its card), then reload the call tab.

## Use — on-page dock (v1.1, no popup needed mid-call)

When you open Meet / Teams / Zoom web / OmeTV, a small **Snap Vanish dock**
appears in a bottom corner of your screen (drag its header to move it to any
corner, press — to collapse it to just the header). It is a DOM overlay only —
the other side never sees it, it is not part of your video.

- **🫰 Snap button:** click it with the mouse instead of snapping your fingers.
  Click again to come back. This is the easiest way on OmeTV.
- **📷 Empty room button:** click it BEFORE you start talking, step out of
  frame for 2 seconds, and the dock will say “Empty room captured ✓”. That
  clean room is what the other side sees while you are dust.
- **⏻ ON/OFF** and **⚙️ Settings** icons: settings opens sensitivity, dust
  duration, dust direction and the snap-sound toggle, right in the dock.

## Use

- **Best empty room:** before the call, click **📷 Empty room** in the dock
  (or Capture empty room in the popup), then step out of frame for 2 seconds.
  That clean room is what the other side sees while you are dust. Without a
  capture, the room is rebuilt from background the camera has seen while you
  move, with a soft blur fill for any part never seen — good, but a capture
  looks far more real.
- **Snap:** hand in frame, palm toward camera, snap thumb + middle finger.
  A 🫰 flashes on your screen (only you see it) when a snap is caught.
- **Backups:** press **D** (not while typing), **Alt+Shift+V**, or **Snap now**
  in the popup. Change Alt+Shift+V at `chrome://extensions/shortcuts` if it clashes.
- **Popup settings:** on/off switch, Snap now button, Capture empty room,
  Require snap sound (on by default), snap sensitivity, dust duration (1–5 s),
  dust direction (Right / Left / Up / Down / Outward).

## How the snap check works (no false triggers)

A snap fires **only when two things happen together**:

1. **Hand gesture:** MediaPipe Hands (bundled, on-device) sees the snap pattern —
   thumb cocked on the middle finger, then the fingertip whipping away, or a
   pinch after the fingers were apart. Typing hands and normal waves do not
   match this pattern.
2. **Mic click:** within about 0.7 seconds, the mic must also hear a real
   snap click — a short, high-frequency transient jumping above background
   noise. The click is analysed locally and is never recorded, stored or sent.

If the gesture happens with no click, nothing happens. If a click happens with
no gesture (typing, a pen click), nothing happens. If your mic is off or the
site never gave the extension an audio track, gesture snaps will not fire and
the status pill says so — use D / Alt+Shift+V / Snap now instead.
Turning “Require snap sound” off in the popup returns to gesture-only (less reliable).

## How the empty room works — honest note

There is no lightweight generative-AI model that can run fully inside a Chrome
extension and paint a photorealistic room from nothing. Snap Vanish instead does
the best local version of it:

- **MediaPipe Selfie Segmentation** (bundled, on-device) cuts only you out, so
  only you turn to dust.
- The **background plate** is built live from every pixel the camera has
  actually seen as background while you move.
- Any part never seen is filled with a soft, blurred version of the surrounding
  room, so you never get a black silhouette.
- **Capture empty room** replaces the plate with a real photo of your empty
  room — that is the way to get a truly convincing empty room.

## Privacy

Everything runs locally in your browser. The hand/segmentation models are bundled
in the extension (that is why the zip is large). No video, audio or image ever
leaves your computer to the extension — the only video leaving is the filtered
feed the call site itself sends, exactly as a normal call would.

## Troubleshooting

- **No skeleton lines** → tracking has not started. Keep the call tab in front,
  wait a few seconds, hand well-lit and fully in frame. Reload the tab once
  after installing.
- **Skeleton shows but snap does not fire** → do a louder, sharper real snap
  (not just a pinch), or raise sensitivity. Check the mic is unmuted in the
  browser — the click detector needs the mic track.
- **False triggers** → lower sensitivity, and keep “Require snap sound” on.
- **Other side still hears you while dust** → muting disables the mic track
  the site receives, so they hear silence; Meet/Teams/Zoom's own mute button
  may not change visually. If a site asked for mic in a completely separate
  request, Snap Vanish registers that track too — reload the tab if it misses.
- **Camera looks unfiltered** → the site asked for the camera before the
  extension loaded. Reload the tab and rejoin; keep the tab visible (browsers
  pause camera processing in hidden tabs).
- **Zoom desktop app is not supported** — this is a browser extension, so it
  works on Zoom in the browser (zoom.us web client), not the installed Zoom app.
  Same for Teams: web only, not the desktop app.

## Files

- `src/inject.js` (+ `src/inject.bundle.js`, built from it + the embedded hand models)
  — camera filter, hand tracking + skeleton, snap gesture + mic-click gate,
  dust engine, background plate + blur inpaint, mic-track mute
- `src/bridge.js` — settings relay between popup and page, status pill
- `src/background.js` — Alt+Shift+V shortcut relay
- `src/popup.js`, `popup.html` — toolbar popup
- `vendor/mediapipe/` — MediaPipe Selfie Segmentation, pinned v0.1
- `vendor/mediapipe-hands/` — MediaPipe Hands, pinned v0.4
