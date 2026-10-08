# Motion Matching Playground

A browser-based experimental **motion matching playground** built with vanilla JavaScript and [Three.js](https://threejs.org/).

Load your own animation clips and a compatible 3D character model, then experiment with real-time motion matching, trajectory prediction, feature weighting, animation switching, and inertialization directly in the browser.

## Features

- Real-time motion matching
- Load multiple **BVH / FBX** animation clips
- Load **GLB / FBX** character models
- Runtime motion database generation
- Nearest-frame motion matching
- Foot position and velocity features
- Hip/root velocity features
- Future trajectory prediction
- Future trajectory facing direction
- Configurable feature weights
- Configurable motion-switch threshold
- Optional inertialization for smoother transitions
- Desired vs matched trajectory visualization
- Stick-figure skeleton visualization
- Responsive desktop and mobile interface
- Keyboard and touch controls
- Light/dark interface support
- Drag-and-drop animation loading
- Browser-local processing of user-supplied assets

## How It Works

The playground builds a motion database from the animation clips supplied by the user.

At runtime, the system continuously:

1. Reads player movement input.
2. Generates a desired future trajectory.
3. Extracts motion features.
4. Compares the desired motion against available database frames.
5. Finds the closest matching frame.
6. Switches to the selected animation frame when appropriate.
7. Smooths the transition using inertialization when enabled.

The current feature representation uses:

- Foot positions
- Foot velocities
- Hip/root velocity
- Future trajectory positions
- Future trajectory facing directions

The matching system allows different feature groups to be weighted independently.

## Motion Matching Configuration

| Parameter | Description |
|---|---|
| Pose Weight | Controls the influence of pose-related features |
| Velocity Weight | Controls the influence of velocity features |
| Trajectory Position Weight | Controls future position matching |
| Trajectory Direction Weight | Controls future facing matching |
| Switch Threshold | Determines when a new animation frame should be selected |
| Inertialization | Smooths transitions between matched poses |
| Search Interval | Controls how frequently the motion database is searched |

The current feature representation contains **27 dimensions** covering pose, velocity, and future trajectory information.

## Controls

### Keyboard

| Key | Action |
|---|---|
| `W` / `↑` | Move forward |
| `S` / `↓` | Move backward |
| `A` / `←` | Move left |
| `D` / `→` | Move right |
| `Shift` | Run |

### Touch

On touch-enabled devices:

- Use the virtual joystick to control movement.
- Use the **RUN** button to increase movement speed.

## Loading Animations

The animation loader supports:

- `.bvh`
- `.fbx`

Multiple animation clips can be loaded into the same motion database.

When loading multiple files, the clips should use the **same or compatible skeleton structure**.

For best results, use clips representing different locomotion states such as:

- Walk
- Run
- Sprint
- Directional movement
- Turning
- Starting
- Stopping

## Loading a Character Model

The playground supports:

- `.glb`
- `.fbx`

The character is automatically inspected for common skeleton and bone naming conventions.

Compatible rigs are mapped using normalized bone names and common aliases where possible.

Because character rigs differ between assets, **retargeting is not guaranteed for every model**.

## Trajectory Visualization

The playground provides visual feedback for the desired and matched movement trajectories.

The visualization includes:

- Desired trajectory
- Matched trajectory
- Current character/root position
- Future movement direction

This makes it easier to understand why the motion matcher selected a particular animation frame.

## Technology

The project is intentionally lightweight and runs directly in the browser.

### Core

- HTML5
- CSS3
- JavaScript
- Three.js r128

### Three.js Components

The project uses Three.js loaders for:

- BVH
- FBX
- GLTF / GLB

Additional browser-side compression support is provided by `fflate`.

## Running Locally

No build system is required.

You can serve the project using any local HTTP server.

For PHP users:

```bash
php -S localhost:8000
```

Then open:

```text
http://localhost:8000
```

Using a local HTTP server is recommended instead of opening the HTML file directly.

## Project Structure

```text
Motion-Matching-Playground/
├── index.html
├── THIRD-PARTY-NOTICES.txt
└── README.md
```

### `index.html`

Contains the main browser application, including:

- User interface
- Responsive styling
- Three.js scene
- Animation loading
- Motion database generation
- Feature extraction
- Motion matching
- Character/model handling
- Keyboard controls
- Touch controls
- Trajectory visualization

### `THIRD-PARTY-NOTICES.txt`

Contains notices relating to third-party software used by the project.

### `README.md`

Project documentation, setup instructions, and technical information.

## Motion Matching Pipeline

```text
Player Input
     │
     ▼
Desired Movement
     │
     ▼
Future Trajectory
     │
     ▼
Feature Generation
     │
     ▼
Motion Database Search
     │
     ▼
Feature Distance Comparison
     │
     ▼
Best Matching Frame
     │
     ▼
Animation Transition
     │
     ▼
Inertialization
     │
     ▼
Character Motion
```

## Feature Matching

The motion matcher compares the desired motion state with motion frames stored in the runtime database.

The matching cost can be influenced by four main feature groups:

```text
Matching Cost
│
├── Pose
├── Velocity
├── Future Position
└── Future Direction
```

This makes it possible to experiment with how different aspects of motion influence animation selection.

## Important Notes

This project is an **experimental browser-based implementation** intended for learning, experimentation, demonstration, and portfolio presentation.

It is not intended to be a production-ready motion matching system.

Current limitations include:

- Motion searches are performed at runtime.
- Large animation databases can increase CPU usage.
- Retargeting depends on skeleton compatibility.
- Different rigs may require additional bone mapping.
- There is no production-level acceleration structure for extremely large databases.
- Advanced contact solving is not implemented.
- Dedicated foot locking is not implemented.
- Motion quality depends heavily on the animation data supplied by the user.

## Assets and Licensing

This repository does **not bundle a third-party motion-capture dataset or character asset collection**.

Animation and character files are supplied by the user at runtime.

You are responsible for ensuring that you have the necessary rights and licenses for any animation, motion-capture, character, or other assets you load into the playground.

Third-party libraries remain subject to their respective licenses.

See:

```text
THIRD-PARTY-NOTICES.txt
```

for project notices.

## Browser Processing

Animation and model files supplied through the interface are processed by the browser for the motion-matching session.

The core motion-matching process does not require those user-supplied assets to be uploaded to a remote server.

## Why This Project?

Motion matching is a character-animation technique that selects animation poses based on the character's current state and desired future movement.

This playground provides a lightweight environment for experimenting with those concepts without requiring a full game engine.

It can be useful for exploring:

- Motion matching algorithms
- Animation search
- Trajectory prediction
- Feature engineering
- Animation blending
- Character locomotion
- Runtime animation systems

## Future Improvements

Potential future improvements include:

- Faster motion database indexing
- KD-tree or other spatial indexing
- Improved animation blending
- Foot locking
- Contact detection
- More advanced trajectory prediction
- Improved skeleton normalization
- Improved FBX retargeting
- Animation database visualization
- Performance profiling
- Larger motion databases
- More sophisticated locomotion states

## Third-Party Dependencies

The project currently uses browser-hosted third-party libraries including:

- Three.js
- Three.js BVHLoader
- Three.js GLTFLoader
- Three.js FBXLoader
- fflate

See `THIRD-PARTY-NOTICES.txt` for applicable third-party notices and licensing information.

## License

The project source code is provided for experimentation, educational use, and portfolio presentation.

Third-party libraries, animation files, character models, and other external assets remain subject to their own licenses.

Do not redistribute third-party animation datasets or character assets unless their licenses explicitly permit redistribution.

---

**Motion Matching Playground**

A browser-based experiment in real-time animation matching and trajectory-driven locomotion.
