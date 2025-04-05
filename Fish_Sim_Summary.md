# Fish Simulation (`Fish_sim_cla`) Code Structure Summary

This document outlines the structure and functionality of the `Fish_sim_cla` HTML file, which implements a 3D fish simulation using Three.js.

## 1. HTML Structure (`<head>` and `<body>`)

*   **`<head>`**:
    *   Sets character encoding, viewport settings, and the page title ("თევზის 3D სიმულაცია").
    *   Includes Tailwind CSS via CDN for styling.
    *   Contains a `<style>` block for custom CSS rules.
    *   Defines an `importmap` to manage the Three.js module import.
*   **`<body>`**:
    *   Uses Flexbox (`flex h-screen`) for the main layout.
    *   **Left Side (Simulation Area)** (`<div class="flex-grow relative h-full">`):
        *   `#simulationCanvas`: The canvas element where the Three.js simulation is rendered.
        *   View Labels (`#topViewLabel`, `#sideViewLabel`, `#frontViewLabel`): Absolute positioned labels to identify the different camera views.
        *   Metrics Display (`.metrics-display`): Fixed positioned container showing real-time friction and speed metrics.
        *   Loading Indicator (`#loadingContainer`, `.spinner`): Displays while the simulation is initializing.
        *   Error Message Container (`#errorContainer`, `#errorMessage`): Displays initialization or runtime errors.
    *   **Right Side (Control Panel)** (`<div class="w-72 h-full ... control-panel">`):
        *   Contains sliders and toggles to control the fish's shape and physics properties.
        *   **Shape Controls**: Sliders for Length, Width, Height, Tail Pointiness, and Body Roundness, each with a value display (`<span>`).
        *   **Physics Controls**: A toggle switch (`#mucusToggle`) for enabling/disabling mucus simulation.
        *   Informational Text: A brief note about the simplified simulation.

## 2. CSS Styling (`<style>`)

*   Provides custom styles beyond Tailwind CSS:
    *   Basic body and canvas setup.
    *   Custom styling for `input[type="range"]` sliders (appearance, thumb).
    *   Custom styling for the toggle switch.
    *   Styling for the view labels (`.view-label`).
    *   Styling for the control panel (`.control-panel`, `.control-header`, `.metric-card`, value indicators).
    *   Styling for the error message and loading indicator overlays.
    *   Styling for the fixed metrics display (`.metrics-display`, `.metric-badge`).
    *   Compact layout adjustments for the control panel.

## 3. JavaScript Logic (`<script type="module">`)

*   **Imports**: Imports the `THREE` module using the defined import map.
*   **Global Variables**:
    *   `scene`, `renderer`, `cameraTop`, `cameraSide`, `cameraFront`: Core Three.js objects.
    *   `frustumSize`: Controls the orthographic camera view size.
    *   `fishMesh`, `fishMaterial`: Objects representing the fish.
    *   `particles`, `particleGeometry`, `particleMaterial`, `particleVelocities`: Objects and data for the particle system simulating water flow.
    *   `controls`: Object to store current values from UI controls.
    *   `metrics`: Object to store calculated friction and speed.
    *   `isMucusActive`: Boolean flag for the mucus effect.
    *   Constants (`particleCount`, `particleSpawnBox`, `simulationBounds`, `viewPadding`).
    *   `fishBoundingBox`: A `THREE.Box3` for simplified collision detection.
    *   `animationFrameId`: Stores the ID for the animation loop to allow cancellation.
    *   `viewportInfo`: Stores calculated dimensions for the three viewports.
*   **Helper Functions**:
    *   `safeGetElement(id)`: Safely gets a DOM element by ID, logging a warning if not found.
    *   `showError(message)`: Displays an error message in the dedicated error overlay.
    *   `hideLoading()`: Hides the loading indicator.
*   **UI Element References**: Variables holding references to DOM elements (sliders, toggles, value spans, canvas, labels, etc.), obtained using `safeGetElement`.
*   **WebGL Support Check**:
    *   `checkWebGLSupport()`: Checks if the browser supports WebGL; shows an error if not.
*   **Viewport Management**:
    *   `updateViewports()`: Calculates the size and position of the top, side, and front viewports based on canvas container size and padding.
    *   `updateViewLabels()`: Positions the view labels (`#topViewLabel`, etc.) within their respective viewports.
*   **Initialization (`init()`)**:
    *   Checks for WebGL support.
    *   Sets up the Three.js `scene`, `renderer`, `lighting`, and orthographic `cameras` (Top, Side, Front) with correct aspect ratios and orientations.
    *   Creates the initial `fishMaterial`.
    *   Calls `updateShape()` to create the initial fish geometry based on default slider values.
    *   Calls `createParticles()` to set up the particle system.
    *   Adds event listeners for UI controls (`input`, `change`) and window resize.
    *   Calls `updateControlValues()`, `updatePhysicsParameters()`, and `updateViewLabels()` for initial setup.
    *   Hides the loading indicator.
    *   Returns `true` on success, `false` on failure.
*   **Cleanup (`cleanup()`)**:
    *   Called before the page unloads (`beforeunload` event).
    *   Cancels the animation frame request.
    *   Removes event listeners.
    *   Disposes of Three.js geometries, materials, and the renderer to free up resources.
    *   Removes objects from the scene.
*   **Fish Geometry Creation (`createFishGeometry(...)`)**:
    *   Generates the vertices and faces for the fish mesh based on input parameters (length, width, height, pointiness, roundness).
    *   Uses trigonometric functions and parameter logic to define the body shape, tapering, fins, and tail.
    *   **Note**: The geometry logic aims for a specific shape with distinct width/height and tapering, not a simple lathe. The tail is specifically positioned and shaped.
    *   Returns a `THREE.BufferGeometry`.
*   **Particle System (`createParticles()`)**:
    *   Initializes particle positions randomly within a `particleSpawnBox`.
    *   Initializes particle velocities (mostly forward along Z).
    *   Creates `particleGeometry` and `particleMaterial`.
    *   Creates the `THREE.Points` object (`particles`) and adds it to the scene.
    *   Adds `AxesHelper` and `GridHelper` objects to the scene for visual orientation aid.
*   **Shape Update (`updateShape()`)**:
    *   Reads current values from the shape control sliders.
    *   Calls `createFishGeometry` with the new values.
    *   Disposes of the old fish geometry and assigns the new one to `fishMesh`.
    *   Updates the `fishBoundingBox` based on the new geometry.
    *   Calls `updateControlValues()` to update the displayed slider values.
*   **Physics Update (`updatePhysicsParameters()`)**:
    *   Updates the `isMucusActive` flag based on the mucus toggle switch state.
*   **UI Value Update (`updateControlValues()`)**:
    *   Updates the text content of the `<span>` elements next to each slider to show the current numerical value.
*   **Event Handling**:
    *   `handleSliderChange()`: Called when any shape slider value changes. Calls `updateShape()`.
    *   `onWindowResize()`: Called when the browser window is resized. Updates renderer size, viewport calculations, camera aspect ratios/matrices, and view label positions. Calls `render()`.
*   **Animation Loop (`animate()`)**:
    *   Uses `requestAnimationFrame` for continuous updates.
    *   Updates particle positions based on their velocities.
    *   Performs simplified collision detection between particles and the `fishBoundingBox`.
    *   Adjusts particle velocity upon collision (deflection, damping), influenced by `isMucusActive`.
    *   Resets particles that go outside `simulationBounds`.
    *   Calculates simplified `friction` and `speed` metrics based on shape, collisions, particle speed after interaction, and mucus state.
    *   Updates the metric display elements (`#frictionDisplayFixed`, `#speedDisplayFixed`).
    *   Marks particle positions as needing update (`needsUpdate = true`).
    *   Calls `render()`.
*   **Rendering (`render()`)**:
    *   Enables `scissorTest`.
    *   Sets the `viewport` and `scissor` rectangle for each view (Top, Side, Front).
    *   Renders the `scene` using the corresponding `camera` for each viewport.
    *   Disables `scissorTest`.
*   **Initial Execution**:
    *   Calls `init()` to set up the simulation.
    *   If `init()` succeeds, it calls `animate()` to start the simulation loop.

## 4. Responsiveness Changes

*   **Layout:** Modified the main layout to stack vertically (`flex-col`) on small screens and transition to horizontal (`md:flex-row`) on medium screens and larger. The simulation area takes up the top half (`h-1/2`) on small screens, while the control panel takes the bottom half (`h-auto`, `w-full`). On larger screens, the layout reverts to the side-by-side configuration (`md:h-full`, `md:w-72`).
*   **Metrics Display:** Updated the positioning of the fixed metrics display (`.metrics-display`) to use Tailwind utility classes (`absolute top-4 right-4`) instead of a fixed pixel offset, making it relative to the simulation container. 

## 5. Fixes

*   **Stray Code Removal:** Removed misplaced JavaScript code comments and function fragments from the very beginning of the HTML document (before `<!DOCTYPE html>`) that were incorrectly rendered as text content. 