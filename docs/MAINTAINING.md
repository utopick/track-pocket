# Maintaining TRACK.Pocket

A guide for whoever looks after this code next. It explains how the app is put together, in plain words, so you can
find your way before you change anything. It covers the `midi` branch (the newest code).

## What it is
TRACK.Pocket is a pocket music maker: a tracker (notes on a grid of steps), a sampler, a mixer, live loops, live effects and the Variator (songs generated from a seed). It runs in any modern browser, and as an Android app built from the same file.

## The one-file rule
The whole app is **one file: `index.html`** (about 13,700 lines). There is no build step, no framework and no
dependencies to install, so anyone can open it, read it and change it.
- Inside, in this order:
  - a comment at the top that lists the layout of the file and every storage key (read it first)
  - `<style>`
  - the markup: one `<section>` per view
  - one big `<script>`
- To find something, search for its name. Views are sections with an id; buttons have ids; the code wires them with
  `$("#id").onclick = ...`.
- `$` is `document.querySelector` and `$$` is `querySelectorAll`. `toast("...")` shows a short message.
- The help texts live in the `HELP` array, in English (`en`) and Spanish (`es`). When you change what a button does,
  change its help in both.

## The views
`trk` (tracker: the squares and the keys, plus the song and live screens inside it), `smp` (sampler), `mix` (mixer), `var` (Variator), `file` (files, themes, options, help).
`showView` / `showTab` switches between them: it shows one section and asks that view to draw itself.

## The state: where things are kept
- `song`: the song being edited:
  - `bpm`, `kit`
  - `chInst`: the instrument of each channel
  - `patterns`: each has `len` rows and `ch[channel][row]` cells
  - `order`: the list of patterns that makes the song
  - `mix`: one entry per channel
  - A cell is `null` or `{ n: note (-1 = OFF), v: volume 0-15, c: chord offsets, ... }`.
- `S`: what the screen is doing:
  - `S.tab`, the view
  - `S.pat`, the pattern on screen
  - `S.col`, the channel
  - `S.playing`, `S.mode`, and `S.live` / `S.queue` for live loops
- `CH`: how many channels the song has (4, 8 or 16; the Variator uses 8).
- `samples[]`: the 16 sample slots.

Rule of thumb: change the state, then call the view's `...Face()` or `render...()` function to redraw it. Don't
change the screen by hand and hope the state follows.

## Sound
All sound is the Web Audio API: an `AudioContext` (`ctx`), made on the first tap because browsers only allow sound
after a gesture.
- `buildGraph` makes one bus per channel (volume, pan, reverb send, effects) into the master and the live effects.
- `voice()` plays one note of any instrument (synth, FM, sample or drum).
- `tick()` runs every 25 ms and schedules the next rows a little ahead with `scheduleRow()`; this is why the
  timing stays tight even when the screen is busy.
- `liveStep()` does the same for LIVE, where each channel can loop a different pattern.

## Storage
- IndexedDB (`dbGet` / `dbSet`) keeps the saved songs (`song:<name>`), their table (`song-meta`) and the
  `autosave`, which is written every 15 seconds and when the page is hidden.
- localStorage keeps the small things: theme, language, options, the MIDI map (`track.midi.map`) and the Variator's
  settings (`track.var`).
- The song file is JSON (`serialize` / `loadData`). **Never renumber instruments**: the numbers are stored in
  songs.

## MIDI (experimental, `midi` branch)
The same block is pasted in all three apps, starting at the comment "MIDI IN (experimental". Change it in one
app, then copy it to the other two so they stay identical.
- `midiConnect()` asks the browser for MIDI. Chrome, Edge and Firefox have it; Safari and iPhone don't.
- `midiMsg()` receives every message:
  - A control the user ASSIGNed runs its action (`MI.map`, `MI.actions()`).
  - Other notes go to `MI.on` / `MI.off`, which each app defines.
  - The sustain pedal (CC 64) delays `MI.off` until it comes up.
- ASSIGN (`midiAssign`) walks through `MI.actions()`, one press per action.
- TEST (`midiTest`) lists what a device sends and copies a report. Ask users to paste it in issues: it says exactly
  what their controller does.

## The Variator in two minutes
- `V_STY` describes each style:
  - tempo range, drum kit, swing
  - major or minor
  - instruments
  - drum grooves
  - chord progressions in Roman numerals (`"I V vi IV"`)
- `varBuild()` makes a whole song from `VAR` (seed, style, knobs). Everything random comes from `vRng(...)`, a
  generator seeded by a text key, so **the same key always gives the same numbers**.
  - Each part (drums, bass, chords, lead, arp, pad, perc, counter) has its own key, which includes its VARIATION
    number. That is why turning one knob changes only that part.
- `vPattern()` writes one 4-bar pattern, part by part, into ordinary cells. That is why an exported song is a
  normal song.
- While the view is open, `varEnter()` puts the Variator's song in `song` and keeps yours in `varAway`;
  `varLeave()` puts it back. `saveNow()` does nothing in between, so autosave never overwrites your song.

## The Android app
`android/` wraps the same `index.html` with Capacitor.
- `npm run android:init` makes the native project.
- `scripts/` copies the page, sets the microphone permission and makes the icons.
- GitHub Actions (`.github/workflows/android.yml`) builds a signed APK on every push. A tag like `v1.2.0` also
  publishes a release with the APK.
- The signing key is in the repository secrets: `ANDROID_KEYSTORE_BASE64`, `ANDROID_KEYSTORE_PASSWORD`,
  `ANDROID_KEY_ALIAS`. Never commit a key or a password.
- The web version is GitHub Pages, served from `main`.

## Trying a change
1. Open `index.html` in Chrome. Most things work straight from the file; the microphone and MIDI need
   `localhost`.
2. For `localhost`, run the small server in the parent folder (`Pocket dev server.command`, or
   `node dev-server.js`). It shows the three apps and the branch each one is on.
3. Look at it on a phone-sized window (about 390 × 844): the apps are designed for phones first.
4. Open the browser console. A change is not done until it shows no red errors.
5. Automated checks so far are Playwright scripts that open each app, press its buttons and send fake MIDI.
   Keeping them in the repository is the next step for contributors.

## Rules of thumb
- One file, no dependencies, no network (SING's lyrics search is the one exception). Keep it that way.
- Phones first: big squares, nothing that needs a hover or a right click.
- Every new button gets a `title`, a line of help in English and Spanish, and a toast that says what it did.
- Saved data must keep loading. When you change a format, fill in what old files lack (as `loadData` does).
- Fonts and code must be free and open (the only third-party code is listed in the top comment).
- Small commits with clear messages; try each change on a phone before a release.
