# 🛠 Installation & Setup Guide

This document provides a step-by-step guide to setting up
the development environment for the **RLSim Simulator**.
Since this project uses Rust and WebAssembly (WASM),
certain build steps and server requirements are mandatory.

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
   git clone git@github.com:RL-Sim/RLSim.git
   cd RLSim
   ```

2. **Compile to WASM:**

   Run this command in the root directory:

   ```bash
   wasm-pack build --target web
   ```

   This creates a `/pkg` directory containing:
   - The compiled `.wasm` binary
   - Auto-generated `.js` loader code

   Do not delete this folder.

3. **Development Workflow**

   To make changes to the simulation:

   1. Modify the Rust code in `src/lib.rs` or `src/traffic.rs`
   2. Re-run the build command: `wasm-pack build --target web`
   3. Refresh your browser
