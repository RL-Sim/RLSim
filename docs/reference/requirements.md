# Project Requirements: Global Traffic Simulator

The "Global/Scalable" requirements.

## 1. Scalability (Internationalization)

- **Rule Abstraction:** No "Right-before-Left" logic should be hardcoded into the `Car` struct. Logic must be handled by a `PriorityEngine` trait.
- **Directional Support:** The system must support both RHT (Right-Hand Traffic) and LHT (Left-Hand Traffic).
- **Language:** All UI strings must be stored in a `locales/` directory (JSON format) to support future translations.

## 2. Localization Use Cases
- 
- **Standard (EU/USA):** RHT, Priority to the Right.
- **Japan/UK:** LHT, Priority to the Left (or specific turning rules).

## 3. Technical Constraints
- 
- **Performance:** Must maintain 60FPS on SVG with 50+ vehicles.
- **Precision:** Coordinate math must use `f64` in Rust for smooth vector movement.
