# 🛣 Choosing the Engine: SVG vs. Canvas

When building a 2D web-based simulation, you must choose between two fundamentally different rendering technologies. For a traffic priority simulator, the choice impacts how you handle physics, collisions, and user interaction.

---

## 1. The Fundamental Concepts

### SVG: The "Object-Oriented" Map
SVG is a **Vector-based, Retained-mode** renderer. Every car, road line, and traffic light is an independent object in the browser's Document Object Model (DOM).
* **Concept:** If you draw a car, the browser "remembers" it as a car object with specific coordinates.
* **Analogy:** Like a set of physical toy cars on a printed map. You can pick up a car, move it, or change its color without redrawing the whole map.

### Canvas: The "Etch-A-Sketch" Grid
Canvas is a **Pixel-based, Immediate-mode** renderer. It is a blank slate of pixels that you "paint" on every single frame.
* **Concept:** If you draw a car, the browser immediately forgets it’s a car and only sees a cluster of colored pixels.
* **Analogy:** Like a whiteboard. To move a car, you must erase the entire board and redraw every single element in its new position 60 times per second.



---

## 2. Why SVG is superior for Traffic Simulators

While Canvas is faster for games with thousands of moving particles (like a space shooter), SVG is mathematically better for a **Logic-Driven Simulator**.

### A. Perfect Geometry & Paths
In a simulator, cars follow lanes. SVG allows you to define these lanes as **Mathematical Paths**.
* **SVG Benefit:** You can tell a car to "Follow Path ID: #North_Turn." The browser handles the curve and rotation automatically.
* **Canvas Limitation:** You would have to manually calculate the trigonometry (Sines and Cosines) for every pixel of that curve.

### B. Built-in Interaction (DOM Events)
In your simulator, you might want to click a car to inspect its speed or "priority status."
* **SVG Benefit:** You can simply use `car.addEventListener('click', ...)`.
* **Canvas Limitation:** You must calculate if the user's mouse coordinates ($x, y$) overlap with the pixels where you *think* the car was drawn.

### C. Inspection & Debugging
Debugging traffic logic (like "Right-before-Left") is easier when you can see the data.
* **SVG Benefit:** You can open the Browser Inspector (F12) and see the exact ($x, y$) coordinates of every car in the code.
* **Canvas Limitation:** The inspector only shows a single `<canvas>` tag; the internal "world" is invisible to the browser tools.



---

## 3. Comparison Summary

| Feature | SVG (Chosen) | HTML5 Canvas |
| :--- | :--- | :--- |
| **Rendering** | Vector (Perfect at any zoom) | Raster (Pixels) |
| **Logic Mode** | Retained (Objects exist in memory) | Immediate (Pixels only) |
| **Performance** | Best for < 500 objects | Best for 10,000+ objects |
| **Accessibility** | Screen readers can "see" cars | Completely invisible to screen readers |
| **Physics** | Easier (Object-based collision) | Harder (Pixel-based collision) |

---

## 4. The Verdict for your Project

For the **Right-before-Left Simulator**, SVG is the winner because:
1. **Precision:** Traffic rules require exact spatial awareness, which vector math provides.
2. **Pathing:** Using SVG `<path>` elements makes lane-following trivial.
3. **Integration:** Rust (via WASM) can manipulate SVG attributes (like `x`, `y`, `rotate`) much more cleanly than it can manipulate a raw pixel buffer in Canvas.

