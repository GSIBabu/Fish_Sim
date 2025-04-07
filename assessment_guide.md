# Simulation Assessment Guide: Control Relationships

This document outlines the cause-and-effect relationships between the adjustable controls in the `index_simple.html` fish simulation and the resulting calculated metrics, primarily Total Drag and Speed. This can be used as a basis for creating assessment questions.

**Core Concepts:**

*   **Total Drag = Form Drag + Friction Drag**
*   **Speed = Constant * Thrust Potential / (1 + Total Drag)**
*   Higher Thrust Potential increases Speed.
*   Higher Total Drag decreases Speed.

**Control Parameter Effects:**

1.  **Length Slider:**
    *   **Increasing Length:**
        *   Increases approximate surface area.
        *   Increases **Friction Drag**.
        *   Increases **Total Drag**.
        *   **Decreases Speed** (assuming Thrust Potential remains constant).
    *   **Decreasing Length:**
        *   Decreases approximate surface area.
        *   Decreases **Friction Drag**.
        *   Decreases **Total Drag**.
        *   **Increases Speed** (assuming Thrust Potential remains constant).

2.  **Width Slider:**
    *   **Increasing Width:**
        *   Increases frontal cross-sectional area -> Increases **Form Drag**.
        *   Increases approximate surface area -> Increases **Friction Drag**.
        *   Significantly increases **Total Drag**.
        *   **Decreases Speed**.
    *   **Decreasing Width:**
        *   Decreases frontal cross-sectional area -> Decreases **Form Drag**.
        *   Decreases approximate surface area -> Decreases **Friction Drag**.
        *   Significantly decreases **Total Drag**.
        *   **Increases Speed**.

3.  **Height Slider:**
    *   **Increasing Height:**
        *   Increases frontal cross-sectional area -> Increases **Form Drag**.
        *   Increases approximate surface area -> Increases **Friction Drag**.
        *   Significantly increases **Total Drag**.
        *   **Decreases Speed**.
    *   **Decreasing Height:**
        *   Decreases frontal cross-sectional area -> Decreases **Form Drag**.
        *   Decreases approximate surface area -> Decreases **Friction Drag**.
        *   Significantly decreases **Total Drag**.
        *   **Increases Speed**.

4.  **Pointiness Slider:**
    *   **Increasing Pointiness (More Streamlined):**
        *   Decreases the penalty factor for **Form Drag**.
        *   Decreases **Total Drag**.
        *   **Increases Speed**.
    *   **Decreasing Pointiness (Blunter):**
        *   Increases the penalty factor for **Form Drag**.
        *   Increases **Total Drag**.
        *   **Decreases Speed**.

5.  **Roundness Slider:**
    *   **Moving Roundness towards the middle value (e.g., 0.5-0.6):**
        *   Decreases the penalty factor for **Form Drag** (approaches ideal fusiform shape).
        *   Decreases **Total Drag**.
        *   **Increases Speed**.
    *   **Moving Roundness towards extremes (0.1 or 1.0):**
        *   Increases the penalty factor for **Form Drag** (less fusiform).
        *   Increases **Total Drag**.
        *   **Decreases Speed**.

6.  **Tail Fin Size Slider:**
    *   **Increasing Tail Fin Size:**
        *   Significantly increases **Thrust Potential**.
        *   **Increases Speed**.
    *   **Decreasing Tail Fin Size:**
        *   Significantly decreases **Thrust Potential**.
        *   **Decreases Speed**.

7.  **Mucus Toggle:**
    *   **Turning Mucus ON:**
        *   Applies a multiplier (e.g., 0.3) to **Friction Drag**, significantly reducing it.
        *   Decreases **Total Drag**.
        *   **Increases Speed**.
    *   **Turning Mucus OFF:**
        *   Removes the friction-reducing multiplier (multiplier is 1.0).
        *   Increases **Friction Drag** (relative to mucus ON).
        *   Increases **Total Drag**.
        *   **Decreases Speed**. 