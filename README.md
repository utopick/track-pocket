![TRACK.Pocket](.github/banner.png)

# TRACK.Pocket

So, after collecting a lot of gadgets and buying lots of music apps, I found out that I was still missing some simplicity for quick music making. I love the Pico-8 tracker and my Pocket Operator KO, so I took some old code, spent some time furiously drawing on notebooks and fed it all to the token burner machine. 

Thus came TRACK.Pocket, a small, browser-based pocket music maker. It runs from one self-contained HTML file, with no install or network connection required. Its one third-party library (the MP3 encoder) is bundled inside that file, so there are no other requests or dependencies. 

It should run fully offline, although opening the HTML file directly can cause microphone permission issues, so use the hosted GitHub page instead or self-host the HTML.

I've been adding features and tweaking a lot, so they come out not fully tested, and there could be bugs. Leave me an issue and I'll fix it.

There is a help wiki inside (también en español), so don't worry, but it should be quite straightforward. It has small interactive demos you can touch and hear: a pattern, the live FX pads and the sampler's grid and quantize.

![TRACK.Pocket on five screens, each in a different theme](.github/readme/hero.png)

## Why did I release these apps? 

Because GitHub allows me to host them in a GitHub site if I make them public, and I don't want to host them myself. 

Also because my friends told me that they are useful and some of you may want a free, ad-less gadget to pass the time.


## Ads and cookies

Yeah, no. 

No ads, no cookies, no nothing. This is free and clean. No hidden stealers, no weird intentions. 

Why is it free? Because not everything should be a product. 

## AI use

I'm a programmer and I have been coding for a living for over two decades, but life has given me too little free time that I want to spend offline and outside, making music and art. So I used a lot of AI to build this in a quick and easy way.

I'm not going to take credit for the code, even though I started this app from my own older code. I built it with the help of an AI coding assistant (Claude, by Anthropic), which wrote and reworked much of the code following my literally scanned designs and notes. And some hyperfocused allnighters too. Also Codex helped, so yeah, he tried his best.

The app itself uses no AI: everything runs on your device, and nothing you play, sing or record is sent to any AI service or anywhere else for that matter. If that isn't the case just shame me publicly and I'll fix it.

I did waste a lot of tokens by making it code it in a non-minified way, so if the bubble bursts and we go back to the basics it could be maintainable. 

If you want to try something made by me by hand (95%, I did use an early model to find some bugs but solved them by hand) in which I made the code, assets, music and all, check out Nanaya, my Pico-8 tower defense game: https://www.lexaloffle.com/bbs/?tid=153336

![A strip of TRACK.Pocket screens](.github/readme/strip.png)

## How to run it

Open `index.html` in a modern web browser. Start playback with **▶**. Your browser may ask for permission to play audio, so interact with the page first if you hear nothing.

The microphone needs `https://` or `localhost` (or the Android app), which is why the hosted page is the easy way.


## The basics

If you've ever used a groovebox or tracker, you've got it. If not, it should be pretty intuitive, and if it's not, then it's my fault.

- **Patterns:** add notes to the step grid, choose instruments, and build a repeating phrase.
- **Playback:** adjust the tempo and press play to hear the current pattern. Use the song and live views to arrange or perform patterns.
- **Playing along:** the keys play while the pattern runs without writing anything. Turn **PLAYING · WRITE** on to write into the selected step as the pattern plays, or press **●** to record each note on the step you hear. On a computer, the keyboard plays the keys, the drum pads and the FX pads, and each pad shows its key.
- **Sampler:** record or open a sound into one of 16 slots, then crop, reverse, normalize or add effects. In **MORE**, an optional **GRID** lays the song's steps over the wave so a selection snaps to whole steps, and **QUANTIZE** moves the hits onto it to tighten a loose beat.
- **Live FX:** open the FX pads and hold one to apply it; release to turn it off. **Latch** keeps an effect on after you release it. **Off** clears active effects. The FX board includes loop/repeat, rolls, pitch, filter, and other performance effects.
- **Save:** use the file controls to save and load songs. TRACK.Pocket also keeps browser-local autosave data.

![The views: pattern, drums, live, live FX, song and mixer](.github/readme/views.png)

### Themes

Six looks, from **THEME** at the bottom of the files screen: Dark, Light, Terminal, Dungeon synth, DMG and Demoscene. All three Pocket apps share them.

![The drum pattern in the six themes](.github/readme/themes.png)

### Your songs

Songs and settings are stored in your browser (in IndexedDB, which gives you plenty of space, and you can export them); they are not uploaded anywhere. Clearing browser storage removes local saves.

## Android app (APK)

The same `index.html` is packaged as an Android app with [Capacitor](https://capacitorjs.com). GitHub Actions builds a debug APK on every push (or from **Actions → Android APK → Run workflow**). Download it from the run page under **Artifacts**, unzip it, and open the `.apk` on the phone (Android asks you to allow installing from that source once).

- The first time you record, Android asks for microphone permission. If you refused it, allow it in **Settings → Apps → TRACK.Pocket → Permissions**.
- In the app, exports (JSON, WAV, MP3, MIDI) open the Android share sheet, so you can save them to Files or Drive or send them to another app.
- Everything for the Android build lives in `android/`. The native project (`android/native/`) is generated by each build and is not kept in the repository; `android/scripts/android-permissions.js` adds the microphone permissions to it.
- Saved songs live in the app's own storage (IndexedDB in the WebView). They survive app updates but not uninstalling, and they are separate from songs saved in Chrome: move songs between them with a JSON export and import.

To build locally you need Node 20, JDK 17 and the Android SDK:

```bash
cd android
npm install
npm run android:init   # once: creates android/native/
npm run android:apk    # android/native/app/build/outputs/apk/debug/app-debug.apk
```

I did not build it natively, GitHub Actions are free (to a point) and save me dozens of gigabytes from the SDK (seriously?).

## Files

- `index.html` — the complete app (markup, styles, and audio code)
- `android/` and `.github/workflows/android.yml` — Android build
- `LICENSE` — license terms



## License

TRACK.Pocket is released under the MIT License (see `LICENSE`).

AI assisted coding uses the uncredited work of a lot of coders through the years (me included) so anything I make that's not for work its as free as possible.

Have fun.

### Third-party code

The banner, app icons and favicons are lettered in [Inter](https://rsms.me/inter/) (SIL Open Font License 1.1), converted to shapes, so no font file is included.

`index.html` bundles **lamejs 1.2.1** (the MP3 encoder, a JavaScript port of LAME) in its own `<script id="lamejs">` block. lamejs is **not** covered by the MIT License: it is licensed under the [GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.html). It is included unmodified; its source is at <https://github.com/zhuker/lamejs> (npm package `lamejs@1.2.1`). You may replace that block with any other compatible build of lamejs.
