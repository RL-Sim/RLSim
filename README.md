# 🚦 RLSim: Interactive Traffic Rules Learning & Simulator

Learn traffic rules. Test them. Master them.

## 🎓 Complete Learning Ecosystem

RLSim is more than a simulator - it's a **complete learning platform** for traffic rules:

---
- 📖 Interactive Manual (Web, PDF, eBook, Mobile)
- 🧪 Interactive Scenarios (Embedded Simulator)              
- 🌍 20+ Countries (Traffic Rules by Region)                 
- ✅ Better than Traditional Driving Books

  .

- 👉 [Start Learning Now](https://github.com/RL-Sim/academy) 
---

### Why RLSim Academy?

- **📚 Printable Manual** - Professional PDF you can print and study offline
- **📱 Any Device** - Web, mobile app (PWA), eBook (EPUB/Kindle)
- **🎮 Interactive** - Embedded RLSim simulator to test rules in real-time
- **🌐 Global** - Traffic rules for 20+ countries
- **🎯 Structured Learning** - Beginner → Intermediate → Advanced paths
- **✍️ Better than Books** - Visual, interactive, always up-to-date

[👉 Go to RLSim Academy](https://github.com/RL-Sim/academy)

---

## 🔧 For Developers: The Simulator

If you're interested in the **technical implementation** of traffic rules:

A high-performance 2D traffic intersection simulator built with **Rust**,
compiled to **WebAssembly (WASM)**,
and rendered using **Scalable Vector Graphics (SVG)**.

## 🛠 Tech Stack

- **Logic Engine:** [Rust]
  (High-performance, memory-safe traffic calculations)
- **Runtime:** [WebAssembly]
  (Near-native execution in the browser)
- **Renderer:** **SVG (Scalable Vector Graphics)**
  (Resolution-independent, DOM-accessible 2D graphics)
- **Animation:** [GSAP]
  (Optional motion path handling)
- **Bridge:** `wasm-bindgen` & `web-sys`
  (Direct Rust-to-DOM manipulation)

## 🚀 Key Features

- **Deterministic Traffic Logic:** Implements the "Right-before-Left" priority rule
  using a spatial coordinate system
- **Vector Precision:** Crisp rendering at any zoom level,
  perfect for observing tight intersection maneuvers
- **Zero-Dependency UI:** The game "world" is part of the DOM,
  allowing for easy debugging via Browser Inspector
- **High Performance:** Capable of simulating 100+ vehicles
  with complex yielding logic without frame drops

## ⚖️ The Simulation Rules

The "Right-before-Left" (Priority to the Right) rule is the core of this project.
The simulation follows these logical steps:

1. **Detection:** As a vehicle approaches an unregulated intersection,
   it enters a "Scan Zone"
2. **Validation:** The Rust engine checks the lane
   immediately to the vehicle's right
3. **Yielding:** If a vehicle is present in the right-hand lane
   within a specific distance threshold ($d < threshold$),
   the current vehicle's velocity is set to $0$
4. **Resolution:** Once the right-hand lane is clear,
   the vehicle resumes its target speed

## 📚 Documentation

Our documentation follows the [Diátaxis framework],
organized into four types for different learning needs:

- **[Getting Started]** - Learning-oriented guides to get you up and running
- **[How-To Guides]** - Task-oriented guides to solve specific problems
- **[API Reference]** - Information-oriented technical documentation
- **[Concepts]** - Understanding-oriented guides explaining design decisions

**Quick Links:**

- [Installation & Setup] - Prerequisites and build instructions
- [Project Structure] - Detailed directory layout and file descriptions
- [Architecture] - System design and data flow
- [Technical Choices] - Why SVG/Rust and design rationale
- [Roadmap] - Development phases and planned features

## Links

- [API Reference]
- [Architecture]
- [Concepts]
- [Diátaxis framework]
- [Getting Started]
- [How-To Guides]
- [Installation & Setup]
- [Project Structure]
- [Roadmap]
- [RLSim Academy]
- [Technical Choices]

<!-- Reference Links -->

[API Reference]: ./docs/reference/api/index.md
[Architecture]: ./docs/reference/architecture.md
[Concepts]: ./docs/explanation/index.md
[Diátaxis framework]: https://diataxis.fr/
[Getting Started]: ./docs/tutorials/getting-started.md
[How-To Guides]: ./docs/how-to/index.md
[Installation & Setup]: ./INSTALL.md
[Project Structure]: docs/reference/project-structure.md
[Roadmap]: ./ROADMAP.md
[RLSim Academy]: https://github.com/RL-Sim/academy
[Technical Choices]: ./TECH_CHOICE.md
