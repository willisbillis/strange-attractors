# Strange Attractors — Interactive 3D Chaos Explorer

A beautiful, interactive web-based 3D visualization gallery for exploring strange attractors and chaotic dynamical systems. Built with WebGL (Three.js) for real-time 60fps rendering directly in the browser — no installation required.

🌀 **[Open the visualization →](https://willisbillis.github.io/strange-attractors/)**

---

## What Are Strange Attractors?

A **strange attractor** is a fractal structure in phase space that a chaotic dynamical system tends toward over time. Unlike a fixed point or a limit cycle, a strange attractor has infinite detail at every scale and is characterized by **sensitive dependence on initial conditions** — the famous "butterfly effect" where vanishingly small differences in starting conditions lead to wildly different trajectories.

---

## Features

### Attractors Included
| Attractor | Discovered | Key Properties |
|-----------|-----------|----------------|
| **Lorenz** | Edward Lorenz, 1963 | The iconic butterfly shape; models atmospheric convection |
| **Rössler** | Otto Rössler, 1976 | Single-scroll spiral; models chemical reactions |
| **Thomas** | René Thomas, 1999 | Cyclic symmetry with trigonometric coupling |
| **Aizawa** | Yositake Aizawa, 1984 | Toroidal structure; six-parameter system |
| **Chen** | Chen & Ueta, 1999 | Double-scroll; discovered in circuit analysis |
| **Halvorsen** | Johan Halvorsen, ~2000s | Cyclic symmetry with quadratic nonlinearities |
| **Dadras** | Dadras & Momeni, 2009 | Multi-wing topology from a simple polynomial |

### Visualization
- **WebGL rendering** via Three.js with additive blending for glowing, ethereal trails
- **Four color schemes**: time gradient (rainbow), velocity (speed→color), depth (z→hue), solid
- **Circular buffer**: up to 50,000 points rendered as a continuous trajectory
- **Adjustable trail length, opacity, and line width**
- Subtle starfield background on a deep dark canvas

### Camera Controls
| Action | Control |
|--------|---------|
| Rotate | Left-click drag |
| Zoom | Mouse wheel |
| Pan | Right-click drag |
| Reset | Click "Reset Camera" |

### Interactive Controls
- **Attractor selector** dropdown
- **Dynamic parameter sliders** — each attractor exposes its own parameters with real-time update
- **Animation controls**: Play/Pause, Reset, Speed multiplier (0.1×–10×)
- **Initial conditions (X₀, Y₀, Z₀)** sliders
- **Gallery mode**: auto-cycles through all attractors every 10 seconds
- **Butterfly Effect mode**: renders 5 trajectories with 1×10⁻⁷ perturbation in initial conditions to visualize chaos
- **Screenshot**: saves the current frame as a PNG file

### Educational Info Panel
Each attractor includes:
- Mathematical equations
- Brief history and discoverer
- Parameter descriptions

---

## Equations

### Lorenz Attractor
```
dx/dt = σ(y − x)
dy/dt = x(ρ − z) − y
dz/dt = xy − βz
Default: σ=10, ρ=28, β=8/3
```

### Rössler Attractor
```
dx/dt = −y − z
dy/dt = x + ay
dz/dt = b + z(x − c)
Default: a=0.2, b=0.2, c=5.7
```

### Thomas Attractor
```
dx/dt = sin(y) − bx
dy/dt = sin(z) − by
dz/dt = sin(x) − bz
Default: b=0.208186
```

### Aizawa Attractor
```
dx/dt = (z − b)x − dy
dy/dt = dx + (z − b)y
dz/dt = c + az − z³/3 − (x² + y²)(1 + ez) + fzx³
Default: a=0.95, b=0.7, c=0.6, d=3.5, e=0.25, f=0.1
```

### Chen Attractor
```
dx/dt = a(y − x)
dy/dt = (c − a)x − xz + cy
dz/dt = xy − bz
Default: a=35, b=3, c=28
```

### Halvorsen Attractor
```
dx/dt = −ax − 4y − 4z − y²
dy/dt = −ay − 4z − 4x − z²
dz/dt = −az − 4x − 4y − x²
Default: a=1.4
```

### Dadras Attractor
```
dx/dt = y − ax + byz
dy/dt = cy − xz + z
dz/dt = dxy − ez
Default: a=3, b=2.7, c=1.7, d=2, e=9
```

---

## Technical Implementation

### Numerical Integration
All attractors are integrated using **4th-order Runge-Kutta (RK4)**:

```
k1 = f(t, y)
k2 = f(t + dt/2, y + dt·k1/2)
k3 = f(t + dt/2, y + dt·k2/2)
k4 = f(t + dt,   y + dt·k3)
y(t+dt) = y(t) + (dt/6)(k1 + 2k2 + 2k3 + k4)
```

Per-attractor `dt` values are tuned to balance accuracy and performance. The default is 10 integration steps per animation frame, giving smooth 60fps rendering.

### Architecture
The application is a single self-contained `index.html` file organized into these sections:
1. **Attractor definitions** — equations, parameters, metadata
2. **Simulation state** — circular buffer, position, flags
3. **Three.js setup** — scene, camera, renderer, OrbitControls
4. **Trajectory management** — circular buffer write, draw range update
5. **RK4 integrator** — generic step function
6. **Color scheme functions** — HSL→RGB, per-scheme logic
7. **UI initialization and event handlers** — sliders, buttons, panel toggles
8. **Gallery mode** — timed attractor cycling
9. **Animation loop** — requestAnimationFrame

### Performance
- `THREE.BufferGeometry` with pre-allocated `Float32Array` buffers
- `geometry.setDrawRange()` to window the circular buffer without data copies
- `THREE.AdditiveBlending` for GPU-side glow without post-processing
- `preserveDrawingBuffer: true` for screenshot support with no extra pass

---

## Usage

Simply open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari). No build step or server required. For the best experience, use a browser with hardware-accelerated WebGL.

To host on GitHub Pages, enable Pages from the repository settings pointing to the `main` branch root.

---

## Interesting Parameter Combinations to Try

| Attractor | Parameters | Effect |
|-----------|-----------|--------|
| Lorenz | ρ=99.96 | Period-3 window inside chaos |
| Rössler | c=4.0 | Period-2 limit cycle before chaos |
| Thomas | b=0.18 | Denser, more tightly wound orbits |
| Lorenz | σ=10, ρ=14, β=8/3 | Stable fixed point (no chaos) |
| Halvorsen | a=1.0 | Larger attractor footprint |
| Chen | a=40, c=30 | Modified double-scroll shape |

---

## References

- Lorenz, E. N. (1963). *Deterministic Nonperiodic Flow*. Journal of Atmospheric Sciences, 20(2), 130–141.
- Rössler, O. E. (1976). *An Equation for Continuous Chaos*. Physics Letters A, 57(5), 397–398.
- Chen, G., & Ueta, T. (1999). *Yet Another Chaotic Attractor*. International Journal of Bifurcation and Chaos, 9(7), 1465–1466.
- Dadras, S., & Momeni, H. R. (2009). *A novel three-dimensional autonomous chaotic system*. Physics Letters A, 373(40), 3637–3642.
- Sprott, J. C. (2010). *Elegant Chaos: Algebraically Simple Chaotic Flows*. World Scientific.
- Strogatz, S. H. (2018). *Nonlinear Dynamics and Chaos* (2nd ed.). CRC Press.

---

## License

MIT License — see [LICENSE](LICENSE).
