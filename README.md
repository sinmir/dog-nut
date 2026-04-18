# Pet the Dog

A single-file browser game. Drag across the dog to pet it. Medium-speed strokes score the most. Pet too fast or stop petting → the dog bites and the run ends.

Works on laptop browsers (mouse) and phone browsers (touch) via the Pointer Events API.

## Run

Open `index.html` directly, or serve the directory:

```sh
python3 -m http.server 8000 --directory /workspaces/gc-workspace/personal/dog-nut
```

Then open `http://localhost:8000/` on your laptop, or `http://<laptop-ip>:8000/` on your phone browser.

(Serving is recommended so browser autoplay rules treat the audio consistently.)

## Assets (all optional — drop in to upgrade)

The game is fully playable with zero assets (emoji + silence). Each file below enhances it:

```
assets/
├── dog.jpg         happy dog image      (fallback: 🐕 emoji)
├── dog-bite.jpg    angry/biting image   (fallback: 😠 emoji)
├── dog-growl.jpg   warning/growling dog (fallback: 🐺 emoji)
├── pet-loop.mp3    soothing pet sound   (fallback: silent)
├── bite.mp3        bark on bite         (fallback: silent)
├── growl.mp3       looping growl        (fallback: silent)
└── tail.png        transparent tail PNG (fallback: inline SVG curve)
```

If a file is missing, the game silently falls back — no code changes needed.

### Where to get free, royalty-free assets

**Audio (CC0, no attribution required):**
- Pixabay — https://pixabay.com/sound-effects/
  - `pet-loop.mp3`: search *calm loop*, *ambient soft pad*, *peaceful loop*
  - `growl.mp3`: search *dog growl*, *low growl loop*, *angry dog growl*
  - `bite.mp3`: search *dog bark*, *dog snap*, *aggressive bark*
- Mixkit — https://mixkit.co/free-sound-effects/ (free license, personal/commercial OK)
- Freesound — https://freesound.org/ (filter by license = CC0)

**Images (free to use):**
- Unsplash — https://unsplash.com/ (search: *happy dog face* for `dog.jpg`, *angry dog* for `dog-bite.jpg`, *growling dog* / *dog bared teeth* for `dog-growl.jpg`)
- Pexels — https://www.pexels.com/
- For best results: square-ish crop, subject centered. Frame all three dog images identically so the swap feels seamless.

Download the files you like, rename them to the four names above, and drop them in `assets/`.

## How scoring works

| Drag speed (avg px/sec) | Points | Bite chance |
|------------------------:|-------:|------------:|
| < 100 (too slow)        | +1     | 0%          |
| 100–300 (gentle)        | +3     | 0%          |
| 300–800 (just right)    | +10 + mood bonus | 0% |
| 800–1500 (a bit fast)   | +6     | up to ~15%  |
| > 1500 (frantic)        | +2     | up to ~80%  |

- **Mood (♥♥♥♥♥)** rises with good pets, falls with bad pets. High mood adds a points bonus; low mood makes bites more likely.
- **Stop petting for 3 seconds** → dog image swaps to the growling variant and a growl loops (warning).
- **Stop petting for 6 seconds** → the dog bites. Keep your hand moving.
- **Resume petting during warning** → growl stops instantly and the dog image snaps back to happy.

A **wagging tail** appears while you're petting.

Best score is saved in localStorage.

## Calibrating tail position

Since every dog photo is framed differently, the game ships with a default tail position that probably won't match your image. To fix this:

1. Tap the **⚙** button in the top bar.
2. A draggable **🌀 tail** pill appears on the dog.
3. Drag it onto your dog's tail (the tail follows live).
4. Tap **Save**. Position persists in localStorage.
5. **Reset** returns to default; **Cancel** discards unsaved changes.

Re-calibrate anytime (e.g., after swapping `dog.jpg`).

## Tuning

All gameplay constants live at the top of the `<script>` in `index.html` — speed bands, mood bonuses, idle timers, audio fade lengths. Adjust to taste.
