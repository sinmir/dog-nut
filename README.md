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
- **Stop petting for 1.5 seconds** → dog image swaps to the growling variant and a growl loops (warning).
- **Stop petting for 4 seconds** → the dog bites. Keep your hand moving — this puppy is impatient.
- **Resume petting during warning** → growl stops instantly and the dog image snaps back to happy.

A **wagging tail** appears while you're petting.

Best score is saved in localStorage.

## Tail position

The wagging tail is positioned for the current `dog.jpg` (puppy lying on its side, rump in the upper-right). If you swap in a different image, tweak the `--tail-x` / `--tail-y` custom properties at the top of the `#tail` CSS rule in `index.html` — they're normalized `0–1` coordinates of the dog container.

## Tuning

All gameplay constants live at the top of the `<script>` in `index.html` — speed bands, mood bonuses, idle timers, audio fade lengths. Adjust to taste.
