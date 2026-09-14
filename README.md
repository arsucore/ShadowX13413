# ShadowX13413/aruscore • Neural Particle Sandbox V2

A high-performance, browser-native 2D canvas simulation engine featuring spatial grid partitioning, adaptive frame budgeting, and real-time kinetic topology.

---

## Technical Highlights

* **$O(N)$ Spatial Hash Grid:** Eliminates classical $O(N^2)$ particle-pair checks by hashing coordinates into uniform cells (`GRID_SIZE = 100px`), enabling 1,800+ concurrent entities at a stable 60 FPS.
* **Dual-Layer Rendering Pipeline:** Decouples background starfield and nebula rasterization onto an offscreen canvas (`destination-over`), allowing dynamic trail decays (`destination-out`) without clearing cosmic assets.
* **Adaptive Workload Throttling:** Automatically monitors rolling framerate averages. If the engine detects sub-40 FPS performance, it shifts to `Lite` mode by halving neural connection lookups and interleaving calculation frames.
* **Idle Galaxy Formation:** When user input ceases for >4.5 seconds, particles smoothly transition from random vectors into an autonomous, 5-armed breathing logarithmic spiral.

---

## Controls & Interaction Matrix

| Input | Mechanism | Physical Effect |
| :--- | :--- | :--- |
| **Left Click (Hold)** | Inward radial vector | Gravity Well (Centripetal pull) |
| **Right Click (Hold)** | Inverted vector ($\times 4$ force) | High-velocity Repulsion Field |
| **Left + Right (Hold)** | Random angular dispersion | Dual-Vector Chaos Burst |
| **Double Click** | Single-frame scalar impulse | Global Shockwave + Color Excitation |
| **Idle State** | Parametric slot mapping | 5-Arm Spiral Galaxy Formation |

---

## Core Features

### Physics & Simulation Engine
* **Spatial Hash Binning ($O(N)$ Optimization):** Replaces classical $O(N^2)$ brute-force distance calculations with a fixed-size 100px uniform grid. Particles query only adjacent 3×3 cells, allowing 1,800+ concurrent particles to run at 60 FPS without garbage collection bottlenecks.
* **Vector Mechanics & Kinetic Drag:** Native implementation of Euler integration with velocity dampening (`friction = 0.96`), boundary toroidal wrapping (screen edge looping), and directional impulse injection.
* **Kinetic Modes:**
  * **Neural:** Density web connections capped at 100px with structured drag (`0.90`) and reduced velocity (`0.5x`).
  * **Swarm:** High-fluidity free-flight mode (`1.2x` speed) with zero line-rendering overhead.
  * **Chaos:** High-speed slippery kinematics (`0.98` friction) layered with continuous frame-level Brownian jitter.
* **Autonomous Spiral Galaxy Formation:** When user interaction halts for >4.5 seconds, particles read their persistent `idleSlot` and smoothly interpolate toward a rotating, pulsating 5-armed logarithmic spiral.
* **Harmonic Density Wave:** During idle mode, neural line thresholds dynamically oscillate across a sinusoidal vertical wave (`densityWave`), simulating biological neural firing.

---

### Graphics & Rendering Architecture
* **Dual-Layer Composite Canvas:**
  * Background starfield and multi-blob nebulae are rasterized **once** to an offscreen buffer upon initialization or resize.
  * Live frames apply `destination-out` (`rgba(0, 0, 0, 0.12)`) to smoothly decay particle motion trails, then composite the offscreen nebula using `destination-over` so backdrops never wash out or tear.
* **Twinkling Star Field:** Statically mapped pseudo-random stars with per-entity phase offsets and individual sine-based luminosity oscillators.
* **Additive Blending Pipeline:** Neural link rendering utilizes `lighter` composite operations to naturally blow out color intensity at dense web intersections.
* **Dynamic Chromatic Feedback:** Particles shift color temperature on kinetic events (e.g., excitation spikes on mouse interaction, strobe shifting during chaos bursts, yellow bloom on shockwaves) before decaying back to base HSL targets.
* **5 Switchable Palette Schemes:** Instant global recalculation across `Cyan/Blue`, `Fire`, `Magenta`, `Rainbow`, and `Monochrome` presets.

---

### Interactive HUD & Diagnostics
* **Movable Glassmorphism HUD:** Draggable, bounding-box-clamped control deck with backdrop blur (`blur(10px)`) and minimize/expand state toggling.
* **Real-time Diagnostics:** Rolling frame-counter readout and particle instance tracking.
* **Adaptive Hardware Throttling (Lite Engine):**
  * **Auto:** Monitors rolling framerate averages. Automatically throttles connection range by 35% and drops neighbor evaluations every other frame if FPS falls below 40.
  * **Lite / Full:** Manual overrides for low-power mobile GPUs or dedicated desktop hardware.
* **Real-time Sliders:** Granular control over connection range (`0–150px`) and physics speed multipliers (`0.1x–3.0x`).

