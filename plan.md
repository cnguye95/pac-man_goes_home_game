# Pac-Man Parachute — Game Plan

## Project Overview
A vertical-scrolling HTML5 browser game where Pac-Man uses a parachute to ascend through 5 lanes of falling acid rain to reunite with his family above the clouds.

## Theme & Story
Pac-Man has been separated from his family, who waits for him above the clouds. He's strapped on a parachute that pulls him steadily upward. He must dodge corrosive acid rain as he ascends.

## Tech Stack
- Vanilla HTML, CSS, JavaScript — no frameworks, no build step
- HTML5 Canvas for rendering
- No external assets, no audio files, no third-party libraries

## Repository Structure
Claude Code is responsible for designing the folder/file layout following standard web-project conventions (separation of HTML, CSS, and JS; clear file names; a `README.md` at the root explaining how to run the game). Suggested baseline:
```
/
├── README.md
├── plan.md
├── index.html
└── src/
    ├── styles.css
    └── game.js
```
Claude Code may refine this if there's a clearly better structure, but the project must remain runnable by simply opening `index.html`.

---

## Game Mechanics

### Player (Pac-Man)
- Always rises upward at a **constant vertical speed** (parachute lift). The player does not control vertical movement.
- Moves **left and right between 5 discrete lanes**.
- **Controls:** `←` and `→` arrow keys only.
- Lane switching uses a quick tween (~100ms) for visual smoothness, but input is treated as instant — no input lag.
- Pac-Man is rendered with a parachute graphic (canopy + lines) drawn above his body.

### Pac-Man Visual (Canvas Primitives)
Pac-Man is drawn directly with Canvas drawing commands — no image files, no downloads, no cost. This is the standard way to render the classic Pac-Man look:
- A filled yellow arc (`ctx.arc(...)`) that stops short of a full circle, leaving a wedge cut out as the mouth.
- The mouth angle oscillates frame-to-frame (e.g., between 0.05π and 0.25π) to create the open-close chomping animation.
- A small black dot for the eye.
- The parachute is drawn above using a semicircle (canopy) and four straight lines (cords).

This approach is completely free, requires no asset management, and produces the authentic classic Pac-Man silhouette.

### Lanes
- Exactly **5 vertical lanes** spanning the full playfield width.
- Both Pac-Man and raindrops are confined to lane centers — no in-between positions.
- Lane width is uniform.

### Acid Rain
- Falls from the top of the screen downward at a constant speed.
- Spawned in **waves**. Each wave occupies **1 to 4 lanes** — **never all 5**. This guarantees there is always at least one passable lane in every wave.
- Visual: bright green/yellow droplets, optionally with a subtle glow.

### Difficulty Modes
The start screen offers a choice between **Easy** and **Hard**. Both modes share the same win condition (5000 points / 5 minutes) and the "never all 5 lanes" rule.

**Easy Mode**

| Phase   | Time         | Wave Interval | Lanes Filled per Wave |
|---------|--------------|---------------|------------------------|
| Phase 1 | 0:00 – 1:30  | ~2.0s         | 1–2                    |
| Phase 2 | 1:30 – 3:00  | ~1.5s         | 1–2                    |
| Phase 3 | 3:00 – 4:30  | ~1.2s         | 2–3                    |
| Phase 4 | 4:30 – 5:00  | ~1.0s         | 2–3                    |

Easy mode also uses a slightly slower rain fall speed.

**Hard Mode**

| Phase   | Time         | Wave Interval | Lanes Filled per Wave |
|---------|--------------|---------------|------------------------|
| Phase 1 | 0:00 – 1:30  | ~1.5s         | 1–2                    |
| Phase 2 | 1:30 – 3:00  | ~1.0s         | 2–3                    |
| Phase 3 | 3:00 – 4:30  | ~0.7s         | 3–4                    |
| Phase 4 | 4:30 – 5:00  | ~0.5s         | 3–4                    |

Hard mode uses the standard rain fall speed.

### Scoring
- Score increases at exactly **1000 points per minute** (≈16.67 points/second).
- Score and elapsed time are visible in the HUD at all times.

### Win / Lose Conditions
- **LOSE:** Any raindrop's bounding box overlaps Pac-Man → game over screen with a clear text announcement.
- **WIN:** Player reaches **5000 points** (5 minutes survived) → win screen showing Pac-Man reunited with his family above the clouds, with a clear text announcement.

---

## HUD & Screens

There is **no audio**. All feedback (win, lose, transitions) is communicated visually with on-screen text announcements.

### Start Screen
- Title: "Pac-Man Parachute"
- Brief story blurb
- Control hints (← and → to dodge)
- **Difficulty selector:** two buttons — `EASY` and `HARD`
- "Click a difficulty to begin"

### In-Game HUD
- Top-left: current score (`Score: 1234`)
- Top-right: elapsed time (`Time: 02:13`)
- Top-center: current difficulty label (`EASY` / `HARD`)

### Game Over Screen
- Large announcement text: **"YOU WERE DISSOLVED BY THE ACID RAIN"**
- Final score and time survived
- "Press R to retry" / Retry button

### Win Screen
- Large announcement text: **"YOU MADE IT HOME!"**
- Visual of Pac-Man reunited with his family above the clouds (drawn with canvas primitives — Ms. Pac-Man + smaller Pac-Men)
- "Press R to play again" / Play Again button

---

## Visuals
- **Background:** vertical gradient that shifts as the player ascends — stormy/dark at the bottom, transitioning to lighter blue with clouds near the top. Even though the world doesn't truly scroll (Pac-Man stays vertically centered), a subtle parallax cloud layer or color shift sells the upward motion.
- **Pac-Man:** canvas-drawn yellow circle with animated mouth + parachute (see Pac-Man Visual section).
- **Rain:** bright green vertical droplets.
- **Family (win screen only):** static canvas drawing using the same primitives.

---

## Implementation Phases

Build incrementally. After each phase, the game should be runnable and testable.

1. **Skeleton** — Render canvas with 5 visible lane guides; static Pac-Man (with parachute and animated mouth) in the center lane.
2. **Controls** — Arrow keys move Pac-Man between lanes 1–5 with a quick tween.
3. **Rain System** — Wave spawner that selects 1–4 lanes per wave (never 5); raindrops fall and despawn off-screen.
4. **Collisions & Game Over** — Bounding-box collision; lose screen with announcement.
5. **Scoring & Win State** — Score counter at 1000/min; timer; win at 5000 points triggers win screen with family reunion.
6. **Difficulty Modes** — Start screen with Easy / Hard selector; difficulty parameters wired to the rain spawner.
7. **Difficulty Scaling** — Ramp wave interval and lanes-per-wave over the 5-minute run for the chosen mode. Playtest for fairness.
8. **Polish** — Background gradient + clouds, start screen styling, optional particle effect on death.

---

## Acceptance Criteria
- [ ] Game runs by opening `index.html` in a modern browser, no build step
- [ ] Start screen presents Easy and Hard mode selection
- [ ] 5 distinct lanes; Pac-Man and rain confined to them
- [ ] Arrow keys (← and →) move Pac-Man between lanes
- [ ] Pac-Man is drawn with canvas primitives in the classic style with animated chomping mouth
- [ ] Pac-Man has a visible parachute attached above him
- [ ] Rain spawns in waves of 1–4 lanes, **never all 5**
- [ ] Rain frequency increases over the 5-minute run, with Easy and Hard ramps differing
- [ ] Score increments at 1000 points/minute
- [ ] Collision with any raindrop ends the game and shows a clear "you lose" announcement
- [ ] Reaching 5000 points triggers the win screen with a clear "you win" announcement and family reunion visual
- [ ] No audio files or sounds anywhere in the project
- [ ] HUD shows score, elapsed time, and current difficulty during play
- [ ] Repository follows clean naming conventions and includes a `README.md`

---

## Notes for Claude Code
- Build incrementally by phase. After each phase, pause and confirm the build runs before continuing.
- Keep the game loop clean: separate update logic from rendering.
- Use `requestAnimationFrame` and delta-time for frame-independent motion.
- Use clear, descriptive file and variable names. Add brief comments for non-obvious logic (especially the rain wave generator and difficulty ramp).
- Include a `README.md` at the project root with: project description, how to run (open `index.html`), controls, and difficulty mode descriptions.
- The "never all 5 lanes" rule is the single most important gameplay constraint — verify it in code with an explicit check, not just by relying on random chance.
