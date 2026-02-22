# 🛠 Installation & Setup Guide

This document provides a step-by-step guide to setting up the development environment for the **Right-before-Left Simulator**. Because this project uses Rust and WebAssembly (WASM), certain build steps and server requirements are mandatory.

---

## 📋 1. Prerequisites

Before you can build the project, you need to install the following tools:

### A. Rust Toolchain
The core simulation logic is written in Rust.
- **Install:** [rustup.rs](https://rustup.rs/)
- **Command:** `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

### B. wasm-pack
This tool compiles Rust into WebAssembly and creates the necessary JavaScript "glue" code.
- **Install:** `curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh`
- **Verify:** Run `wasm-pack --version` in your terminal.

---

## 🏗 2. Build Instructions

Follow these steps to generate the executable WASM package.

1. **Clone the Repository:**
   
   ```bash
   git clone [https://github.com/yourusername/priority-sim.git](https://github.com/yourusername/priority-sim.git)
   cd priority-sim

2. **Compile to WASM:**
   
    Run this command in the root directory:
    Bash

    wasm-pack build --target web

        Note: This creates a /pkg directory. This directory contains the compiled .wasm binary and the auto-generated .js loader. Do not delete this folder.

3. **Local Web Server Setup**

Critical: You cannot run this project by double-clicking index.html. Browsers block WebAssembly files loaded via the file:// protocol due to security (CORS) restrictions. You must use a local HTTP server.
Recommended Methods:
Option A: Python (Quickest)

If you have Python installed, run:
Bash

python3 -m http.server 8000

Then visit http://localhost:8000.
Option B: VS Code (Best for Dev)

Install the "Live Server" extension. Right-click index.html and select "Open with Live Server".
Option C: Node.js

If you use npm, run:
Bash

npx serve .

4. **Development Workflow**

To make changes to the simulation:

    Modify the Rust code in src/lib.rs or src/traffic.rs.

    Re-run the build command: wasm-pack build --target web.

    Refresh your browser.
