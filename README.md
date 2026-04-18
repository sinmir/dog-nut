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
├── dog.jpg         happy dog image     (fallback: 🐕 emoji)
├── dog-bite.jpg    angry/biting image  (fallback: 😠 emoji)
├── pet-loop.mp3    soothing pet sound  (fallback: silent)
└── bite.mp3        bark / growl        (fallback: silent)
```

If a file is missing, the game silently falls back — no code changes needed.

### Where to get free, royalty-free assets

**Audio (CC0, no attribution required):**
- Pixabay — https://pixabay.com/sound-effects/
  - For `pet-loop.mp3`: search *calm loop*, *ambient soft pad*, *peaceful loop*
  - For `bite.mp3`: search *dog growl*, *dog bark*, *angry dog*
- Mixkit — https://mixkit.co/free-sound-effects/ (free license, personal/commercial OK)
- Freesound — https://freesound.org/ (filter by license = CC0)

**Images (free to use):**
- Unsplash — https://unsplash.com/ (search: *happy dog face*, *angry dog*)
- Pexels — https://www.pexels.com/
- For best results: square-ish crop, subject centered.

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
- **Stop petting for 3 seconds** → dog visually desaturates (warning).
- **Stop petting for 6 seconds** → the dog bites. Keep your hand moving.

Best score is saved in localStorage.

## Tuning

All gameplay constants live at the top of the `<script>` in `index.html` — speed bands, mood bonuses, idle timers, audio fade lengths. Adjust to taste.
