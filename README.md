# Motion Matching Playground

A browser-based experimental **motion matching** playground built with vanilla JavaScript and [Three.js](https://threejs.org/).

The playground lets you load your own animation clips and a compatible rigged 3D character directly in the browser, build a motion database from the loaded clips, and interactively select animation frames based on the character's desired movement trajectory and current pose.

> **Asset note:** This repository does not bundle a motion-capture dataset or character assets. Animation and model files are supplied by the user at runtime. Only use assets that you own or have permission to use and redistribute.

## Features

- 🎞️ Load multiple **BVH** or **FBX** animation clips.
- 🧍 Load a compatible **GLB** or **FBX** rigged/skinned character model.
- 🧠 Runtime motion database and nearest-frame matching.
- 📐 27-dimensional motion features combining:
  - foot positions,
  - foot/hip velocities,
  - future trajectory positions,
  - future trajectory facing.
- 🎯 Configurable feature weights for pose, velocity, trajectory position, and trajectory direction.
- 🔄 Configurable animation switch threshold.
- 🫧 Optional inertialization for smoother transitions.
- ⏱️ Adjustable motion-search interval.
- 🎮 Keyboard controls with **WASD** and **arrow keys**.
- 🏃 Hold **Shift** to run.
- 📱 Touch controls with a virtual joystick and **RUN** button.
- 🦴 Optional stick-figure skeleton visualization.
- 👤 Optional 3D model visualization.
- 📍 Visual comparison between the desired trajectory and the trajectory of the matched frame.
- 🌙 Light/dark appearance based on the system theme.
- 📱 Responsive layout for desktop and touch devices.
- 🖱️ Drag-and-drop support for BVH/FBX animation files.
- 🔒 Processing happens locally in the browser; selected assets are not uploaded by the application.

## How motion matching works

The playground converts the loaded animation clips into a searchable motion database.

For each frame, it extracts motion information such as:

1. Character/foot positions.
2. Foot and hip velocity information.
3. Future trajectory positions.
4. Future trajectory facing directions.

The runtime creates a desired trajectory from the player's current input and compares it against the stored features. A weighted nearest-frame search then selects a candidate frame whose motion best matches the requested movement.

The main matching controls are exposed in the UI:

| Parameter | Purpose |
| --- | --- |
| **Pose match** | Controls the importance of pose similarity. |
| **Velocity match** | Controls the importance of velocity similarity. |
| **Trajectory position** | Controls how strongly future movement position affects matching. |
| **Trajectory facing** | Controls how strongly future facing direction affects matching. |
| **Switch threshold** | Controls how readily the system changes to another animation frame. |
| **Inertialization half-life** | Controls transition smoothing. |
| **Search interval** | Controls how frequently the motion database is searched. |
| **Camera distance** | Controls the viewing distance. |

The playground also handles animation clip boundaries so playback can loop within a clip rather than continuing into unrelated frames.

## Controls

### Desktop

| Input | Action |
| --- | --- |
| \`W\` / \`↑\` | Move forward |
| \`S\` / \`↓\` | Move backward |
| \`A\` / \`←\` | Move left |
| \`D\` / \`→\` | Move right |
| \`Shift\` | Run |

### Touch devices

- Drag the virtual joystick to move.
- Hold **RUN** to run.
- The joystick and RUN control appear automatically on touch/coarse-pointer devices.

## Loading assets

### Animations

Use:

- \`.bvh\`
- \`.fbx\`

You can select multiple animation files at once or drag BVH/FBX files onto the viewport.

For reliable matching, animation clips loaded into the same session should use the **same skeleton or a compatible skeleton hierarchy**.

When loading many clips, the optional **walk / run / sprint preference** can be enabled to prioritize locomotion-oriented clips.

### Character model

Use:

- \`.glb\`
- \`.fbx\`

The model should be a rigged/skinned character with a recognizable root/hips joint.

The runtime attempts to map equivalent skeleton joints using normalized bone names and controlled aliases. This allows compatible skeleton naming variations to be handled without requiring one specific asset provider.

## Running locally

This is a static browser application and does not require Node.js, PHP, a database, or a build step.

### Option 1 — Open directly

Open \`index.html\` in a modern browser.

### Option 2 — Use a local web server

For the most reliable browser behavior, serve the directory with any static HTTP server.

For example, with PHP installed:

\`\`\`bash
php -S localhost:8000
\`\`\`

Then open:

\`\`\`
http://localhost:8000
\`\`\`

No server-side application code is required.

## Technology

- **HTML5**
- **CSS3**
- **Vanilla JavaScript**
- **Three.js r128**
- Three.js:
  - \`BVHLoader\`
  - \`FBXLoader\`
  - \`GLTFLoader\`
- \`fflate\` through the Three.js example-loader dependency

Three.js handles the 3D scene, rendering, skeleton/model representation, animation data and file-loader functionality.

## Project structure

Motion-Matching-Playground/
├── index.html
├── THIRD-PARTY-NOTICES.txt
└── README.md

The current playground is intentionally self-contained: the application logic, UI and runtime are contained in \`index.html\`.

## Browser requirements

A modern browser with WebGL support is recommended.

The application uses:

- WebGL rendering,
- File API / ArrayBuffer,
- Pointer Events for touch controls,
- modern JavaScript features.

Chrome, Edge, Firefox and other current browsers should provide the required platform features.

## Performance notes

Motion matching is performed in JavaScript at runtime. Search cost depends on the number of animation frames loaded into the motion database.

For smoother performance:

- Prefer reasonably sized animation sets.
- Avoid loading unnecessarily large numbers of clips at once.
- Increase **Search interval** if matching is too CPU-intensive.
- Adjust the feature weights according to the movement style of your animation set.
- Use compatible clips with consistent skeleton structure and scale.

## Asset and licensing policy

The source code in this repository is separate from the animation/model assets loaded into the application.

**No third-party motion-capture dataset or character asset is bundled with the project.**

If you publish a deployment containing animation or character files, verify that their licenses permit the intended use and redistribution. In particular, do not assume that an animation available for research, personal use, or download is automatically suitable for commercial portfolio deployment.

Third-party runtime libraries retain their respective licenses and notices. See [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

## Limitations

This is an experimental browser implementation rather than a production animation system.

Current limitations include:

- Matching quality depends heavily on the supplied animation set.
- Clips in one motion database should use compatible skeletons.
- Runtime brute-force searching can become expensive with very large databases.
- Automatic skeleton retargeting cannot guarantee correct results for every rig.
- The system does not provide a full offline asset-management pipeline.
- Advanced production techniques such as learned motion matching, large-scale indexing structures, contact solving, or full foot-locking are outside the current scope.

## Why this project?

This project is intended as an interactive exploration of the core ideas behind motion matching:

**player input → desired trajectory → feature comparison → nearest motion frame → smooth transition**

It is designed to make the process inspectable and adjustable directly in the browser rather than hiding the matching process behind a prebuilt animation controller.

## License

Unless a separate license is added to this repository, the project source code is provided as an experimental portfolio/demo project. Third-party dependencies remain under their own licenses.

If you plan to reuse or redistribute the project, review the dependency notices and the licenses of every animation/model asset included in your deployment.
