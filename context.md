# Context for `index_simple.html`

This document provides a breakdown of the structure and functionality within the `index_simple.html` file, intended to aid in understanding and future modifications.

## File Overview

This HTML file sets up a simple 3D fish simulation using the Three.js library. It displays a parametrically controlled fish model in multiple orthographic views (Top, Side, Front) and provides UI controls to adjust the fish's shape and a basic physics parameter (mucus). It also includes a particle system to simulate water flow and displays calculated metrics like drag and speed.

## Structure Breakdown

### 1. HTML Head (`<head>` - Lines 3-117)

*   **Meta Tags (Lines 4-5):** Basic viewport and character set configuration.
*   **Title (Line 6):** Sets the browser tab title to "Simpler Fish Sim".
*   **Tailwind CSS (Line 7):** Imports the Tailwind CSS framework for styling.
*   **Embedded CSS (`<style>` - Lines 8-116):**
    *   **Basic Styles (Lines 9-10):** Body margin, overflow, font, background. Canvas positioning.
    *   **View Container Styles (Lines 12-20):** Styles for the individual view divs (e.g., `view-top`, `view-side`). Includes relative positioning and pointer-events none.
    *   **View Label Styles (Lines 21-31):** Styles for the labels ("Top", "Side", "Front") within each view container.
    *   **Slider Styles (Lines 33-42):** Basic styling for range inputs and their value indicators.
    *   **Loading Indicator Styles (Lines 44-55):** Styles for the loading container and spinner animation.
    *   **Toggle Switch Styles (Lines 58-64):** Custom styles for the mucus toggle switch.
    *   **Metrics Display Styles (Lines 66-115):** Styles for the fixed metrics badges (Total Drag, Speed) including positioning, appearance, and optional gradient backgrounds.

### 2. HTML Body (`<body>` - Lines 118-227)

*   **Main Simulation Area (`#simulation-area` - Lines 121-153):**
    *   **Canvas (`#mainCanvas` - Line 123):** The WebGL canvas where Three.js renders the scene. Sits behind the view containers.
    *   **View Grid Layout (Lines 127-139):** A CSS Grid layout defining the 2x2 structure for the view containers (`#view-top`, `#view-side`, `#view-front`). `pointer-events-none` allows interaction with the underlying canvas.
    *   **Metrics Display (Lines 142-153):** Fixed container holding the metric badges (`#frictionBadge`, `#speedBadge`).
    *   **Loading Indicator (Lines 148-150):** Container for the loading spinner, shown during initialization.
*   **Control Panel (Lines 156-224):**
    *   **Header (Line 157):** "Controls" title.
    *   **Shape Controls (Lines 159-203):** Contains sliders and value displays for fish `Length`, `Width`, `Height`, `Pointiness`, `Roundness`, and `Tail Fin Size`.
    *   **Appearance Section (Lines ~206-235):** Contains toggle switches for `Mucus Secretion`, `Pectoral Fins`, and `Pelvic Fins`.
    *   **Footer Text (Lines ~236-238):** Informational text.

### 3. JavaScript Setup (`<script type="importmap">` - Lines 226-232)

*   **Import Map:** Specifies how to import the Three.js module from a CDN.

### 4. Main JavaScript Logic (`<script type="module">` - Lines 234-675)

*   **Imports (Line 235):** Imports the `THREE` object.
*   **Global Variables (Lines 237-267):**
    *   Three.js objects: `scene`, `renderer`, `cameras`.
    *   Fish objects: `fishMesh`, `fishMaterial`.
    *   UI/Control state: `controls`, `animationFrameId`, `frustumSize`.
    *   Physics state: `isMucusActive`, `metrics` object.
    *   Appearance state: `arePectoralFinsVisible`, `arePelvicFinsVisible`.
    *   Animation state: `animationTime`, `clock`, roll/pitch frequencies and amplitudes, tail animation parameters (`tailFrequency`, `tailAmplitudeFactor`), `originalTailVertices`.
    *   Particle system variables: `particles`, `particleGeometry`, `particleMaterial`, `particleCount`, `particleVelocities`, `particleSpawnBox`, `simulationBounds`.
*   **DOM Element References (Lines ~316-339):** Gets references to various HTML elements.
*   **`init()` Function (Lines ~342-406):**
    *   Initializes the `scene`, `renderer` (with scissor test enabled for viewports).
    *   Sets up `ambientLight` and `directionalLight`.
    *   Creates orthographic `cameras` for top, side, and front views.
    *   Creates the `fishMaterial`.
    *   Calls `createParticles()`.
    *   Adds event listeners for sliders, toggles (mucus, pectoral, pelvic), and window resize.
    *   Calls initial update functions (`updateControlValues`, `updatePhysicsParameters`, `updateShape`, `onWindowResize`).
    *   Hides the loading indicator.
    *   Includes basic error handling.
*   **`createParticles()` Function (Lines ~398-434):**
    *   Creates `particleCount` particles with random initial positions within `particleSpawnBox` and initial velocities (mostly forward).
    *   Sets up `particleGeometry` and `particleMaterial`.
    *   Creates the `THREE.Points` object (`particles`) and adds it to the scene.
*   **`createFishGeometry()` Function (Lines ~450-666):**
    *   Generates the vertices and indices for the fish body procedurally based on input parameters (`length`, `width`, `height`, `pointiness`, `roundness`).
    *   Uses segments and loops to create a tube-like shape, tapering it based on `pointiness` and `roundness`.
    *   Adds a tail fin with thickness (Z offset) and width (X offset at base, tapering to tip) based on `tailFinSizeParam`.
    *   Conditionally (based on `showPectoralFins`) adds pectoral fins with thickness, slant, and dynamic attachment to the body sides.
    *   Conditionally (based on `showPelvicFins`) adds smaller pelvic fins with thickness, attached ventrally.
    *   Returns a `THREE.BufferGeometry`.
*   **`updateShape()` Function (Lines ~677-713):**
    *   Reads current values from sliders into the `controls` object.
    *   Calls `createFishGeometry` to generate the new shape, passing the current `arePectoralFinsVisible` and `arePelvicFinsVisible` states.
    *   Updates the `fishMesh.geometry`. If `fishMesh` doesn't exist, it creates it and adds it to the scene.
    *   Stores the initial positions of the tail fin vertices in `originalTailVertices`.
    *   Calls `updateControlValues()`.
    *   Includes basic error handling.
*   **`updateControlValues()` Function (Lines ~716-723):**
    *   Updates the text content of the value indicator `<span>` elements next to each slider.
*   **`handleSliderChange()` Function (Lines ~726-729):**
    *   Event handler called when any slider value changes.
    *   Calls `updateShape()`.
*   **`updatePhysicsParameters()` Function (Lines ~732-737):**
    *   Event handler called when the mucus toggle changes state.
    *   Updates the `isMucusActive` boolean variable.
*   **`onWindowResize()` Function (Lines ~741-746):**
    *   Updates the renderer size based on the `simulationArea` dimensions. Camera aspect ratios are updated within the `render` loop.
*   **`animate()` Function (Lines ~750-797):**
    *   The main animation loop, requested via `requestAnimationFrame`.
    *   Updates `animationTime` using `clock.getDelta()`.
    *   Calls `updateParticlePositions()`.
    *   Applies roll/pitch stabilization based on fin visibility.
    *   Applies tail swimming animation: loops through tail vertices, calculates X offset based on animation time, Z position, and tail size slider, updates vertex positions, sets `needsUpdate` flag, and calls `computeVertexNormals()`.
    *   Calls `render()`.
*   **`updateParticlePositions()` Function (Lines ~800-833):**
    *   Iterates through each particle.
    *   Updates its position based on its velocity.
    *   Checks if the particle is outside `simulationBounds`. If so, resets its position to within `particleSpawnBox` and resets its velocity.
    *   Updates the `particleGeometry.attributes.position` buffer and sets `needsUpdate` to true.
*   **`render()` Function (Lines ~836-917):**
    *   **Metric Calculation (Lines ~839-872):**
        *   Calculates simplified `formDrag` based on cross-section and pointiness/roundness.
        *   Calculates simplified `frictionDrag` based on approximate surface area.
        *   Applies `mucusMultiplier` to friction drag if `isMucusActive`.
        *   Calculates `totalDrag` from form and friction drag, adding extra drag constants (`PECTORAL_FIN_DRAG_ADDITION`, `PELVIC_FIN_DRAG_ADDITION`) if the respective fins are visible.
        *   Calculates `thrustPotential` based on tail fin size.
        *   Calculates `speed` based on thrust vs. drag.
        *   Updates the fixed UI metric displays (`frictionDisplayFixed`, `speedDisplayFixed`).
    *   **Viewport Rendering (Lines ~875-916):**
        *   Clears the main canvas with transparency.
        *   Iterates through each view container (`view-top`, `view-side`, `view-front`).
        *   Gets the container's bounding rectangle relative to the canvas.
        *   Calculates the viewport parameters (left, bottom, width, height) for WebGL.
        *   Updates the corresponding camera's aspect ratio and projection matrix based on the container size and `frustumSize`.
        *   Sets the renderer's clear color (to an opaque background for the viewport), viewport, and scissor rectangle.
        *   Clears the depth buffer for the viewport.
        *   Renders the `scene` using the specific `camera` for that view.
    *   Disables the scissor test after rendering all views.
*   **Initialization Calls (Lines ~920-921):**
    *   Calls `init()` to set everything up.
    *   Calls `animate()` to start the render loop. 