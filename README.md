# Pac-Man Parachute

A vertical-scrolling HTML5 browser game built for a Human-AI Fusion class assignment.

Pac-Man has been separated from his family, who waits for him above the clouds. Strapped to a parachute that pulls him steadily upward, he must dodge waves of corrosive acid rain to make it home.

## How to Run

1. Clone or download this repository.
2. Open `game.html` in any modern browser (Chrome, Firefox, Edge, Safari).
3. No build step, no server, no internet connection required.

## Controls

| Key | Action |
|-----|--------|
| `←` / `→` Arrow | Move Pac-Man one lane left or right |
| `P` | Pause / resume |
| `R` | Retry after losing / play again after winning |
| `M` | Return to main menu (after game over or win) |

## Gameplay

- Pac-Man rises automatically — you only control left/right movement across **5 lanes**.
- Acid rain falls in waves of **1–4 lanes** — there is always at least one safe lane per wave.
- A raindrop that touches **either Pac-Man's body or his parachute** ends the run.
- Each wave biases away from the previous wave's safe lanes, so standing still is not survivable — you must keep moving.
- **Cherries** drop occasionally (~once every 22–34 seconds). Catch one and Pac-Man sheds his parachute, faces upward, and can **eat** the rain for 7.5 seconds — drops give bonus points instead of killing you.
- Survive for **5 minutes** (reach **5000 points**) to win and reunite with your family above the clouds.

## Difficulty Modes

Both modes share the same win condition (5000 points / 5 minutes survived) and the same "never all 5 lanes" guarantee. Difficulty advances every **30 seconds** across **10 incremental steps** over the 5-minute run; both rain fall speed and average drops-per-wave climb gradually so the run never spikes — it just steadily intensifies.

| Mode | Starting wave interval | Ending wave interval | Starting rain speed | Ending rain speed | Lanes per wave (start → end) |
|------|------------------------|----------------------|---------------------|-------------------|-------------------------------|
| **Easy** | 2.0s | 1.0s | 210 px/s | 250 px/s | 1–2 → 2–3 |
| **Hard** | 1.5s | 0.45s | 280 px/s | 340 px/s | 1–2 → 3–4 |

## Project Structure

```
/
├── game.html    — entire game (HTML + CSS + JS in one file)
├── README.md    — this file
└── plan.md      — original design specification
```
