# Indian Truck Parking

A browser parking game built around a hand-drawn **Desi truck** — the kind of
colourfully decorated lorry you see on Indian highways. Ten levels, tight bays,
arrow-key and touch controls, and a horn you will probably overuse.

No build step, no dependencies, no network calls. Open `index.html` and play.

## Play

```bash
# just open it
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

Or serve the folder and play on your phone over the same network:

```bash
python3 -m http.server 8000
# then visit http://<your-ip>:8000 on your phone
```

## Controls

| Action | Keyboard | Touch |
| --- | --- | --- |
| Accelerate | `↑` / `W` | ▲ pedal |
| Reverse / brake | `↓` / `S` | ▼ pedal |
| Steer | `←` `→` / `A` `D` | ◀ ▶ pedals |
| Horn | `H` or `Space` | HORN OK PLEASE button |
| Retry level | `R` | — |
| Pause | `Esc` / `P` | ☰ menu |

## How it scores

Drive into the yellow **P** bay and hold still for half a second. Your stars
come from how well the truck sits in the bay, not just from getting there:

- **Alignment** — distance from the bay centre and how square the truck sits
  (weighted 44% / 40%).
- **Time left** — a light 16% modifier, so a slow park still counts.

Three stars needs a genuinely tidy park. Park at an angle and you get two.

Every level has its own bay size and angle tolerance, so the later bays are
narrower and far less forgiving than the first dhaba stop.

## The ten levels

| # | Level | The idea |
| --- | --- | --- |
| 1 | Dhaba Stop | Wide bay, learn the throttle |
| 2 | Highway Turn | Park facing sideways |
| 3 | Market Squeeze | Crates narrow the run-up |
| 4 | Gau Mata | Drive around a cow. She is not moving |
| 5 | Parallel Park | Slide between two parked trucks |
| 6 | Truck Bay | Straight in between two trailers |
| 7 | Narrow Alley | A long barrier maze, reverse in |
| 8 | Back It Up | The bay faces away. You have to reverse |
| 9 | Cargo Yard | A crate field with a tight far bay |
| 10 | Delhi Terminal | Everything at once, 13° tolerance |

## How it is built

One self-contained HTML file. No frameworks, no assets, no images.

- **Physics** — a car-like bicycle model: speed, steering with speed-dependent
  turn rate, drag, and a forward/reverse split. Four fixed substeps per frame
  keeps collision response stable.
- **Collision** — oriented bounding boxes resolved with the separating axis
  theorem, with per-axis sliding so you scrape along a crate instead of
  dead-stopping.
- **Art** — the truck, crates, barrels, cones, planters, cows and barriers are
  all drawn procedurally with Canvas 2D paths. Only the UI font (Baloo 2) is
  fetched from Google Fonts.
- **Sound** — the horn is a four-oscillator stack (two saws, a square and a
  triangle) through the Web Audio API. No audio files.
- **Camera** — follows the truck with easing and never letterboxes the lot.
- **Levels** — each level is a data object: start pose, bay, obstacles, palette
  and time limit.
- **Progress** — stars and unlocked levels are kept in `localStorage`.

## Layout

```
index.html   the whole game
```

## Notes

Every level's start position and bay placement were checked to be reachable
with the truck's real turning circle, and the star thresholds were tuned against
measured offsets so three stars actually means a clean park.
