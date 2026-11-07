# TRACK.Pocket

TRACK.Pocket is a small, browser-based pocket music maker. It runs from one self-contained HTML file, with no install or network connection required. Its one third-party library (the MP3 encoder) is bundled inside that file; see [Third-party code](#third-party-code).

## Run it

Open `tracker.html` in a modern web browser. Start playback with **▶**. Your browser may ask for permission to play audio; interact with the page first if you hear nothing.

## The basics

- **Patterns:** add notes to the step grid, choose instruments, and build a repeating phrase.
- **Playback:** adjust the tempo and press play to hear the current pattern. Use the song and live views to arrange or perform patterns.
- **Live FX:** open the FX pads and hold one to apply it; release to turn it off. **Latch** keeps an effect on after you release it. **Off** clears active effects. The FX board includes loop/repeat, rolls, pitch, filter, and other performance effects.
- **Save:** use the file controls to save and load songs. TRACK.Pocket also keeps browser-local autosave data.

Songs and settings are stored in your browser; they are not uploaded anywhere. Clearing browser storage removes local saves.

## Files

- `tracker.html` — the complete app (markup, styles, and audio code)
- `LICENSE` — license terms

## License

TRACK.Pocket is released under the MIT License (see `LICENSE`).

### Third-party code

`tracker.html` bundles **lamejs 1.2.1** (the MP3 encoder, a JavaScript port of LAME) in its own `<script id="lamejs">` block. lamejs is **not** covered by the MIT License: it is licensed under the [GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.html). It is included unmodified; its source is at <https://github.com/zhuker/lamejs> (npm package `lamejs@1.2.1`). You may replace that block with any other compatible build of lamejs.
