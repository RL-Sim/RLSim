# Technical Choices

This document explains the key architectural decisions made for RLSim.

## Overview

RLSim's technology stack was chosen to balance performance, maintainability,
and developer experience.
This guide explains the reasoning behind our major technical decisions.

## Rendering Engine: SVG vs Canvas

For detailed comparison and rationale, see [Rendering Engine Choice].

**Decision:** SVG (Vector-based, Retained-mode renderer)

**Key Reasons:**

- Perfect for logic-driven simulators with < 500 objects
- Built-in DOM interaction and debugging
- Mathematical path support for lane-following
- Better accessibility (screen readers can "see" objects)

## Logic Engine: Rust + WebAssembly

**Decision:** Rust compiled to WebAssembly (WASM)

**Key Reasons:**

1. **Memory Safety:** Rust's ownership system
   prevents common bugs
2. **Performance:** Near-native execution speed
   in the browser
3. **Type Safety:** Compile-time checks
   catch errors early
4. **Concurrency:** Safe multithreaded
   traffic logic
5. **Interoperability:** `wasm-bindgen` provides
   seamless Rust-JavaScript bridge

### Why Not JavaScript?

JavaScript alone would require:

- Manual memory management
  for complex simulations
- Runtime type checking
  (slower)
- Harder debugging
  of traffic logic errors
- Difficult optimization
  of performance-critical code

### Why Not C/C++?

While C/C++ offer similar performance:

- Rust provides memory safety
  without garbage collection
- Easier to maintain
  and refactor
- Better error messages
  and tooling
- Growing WASM ecosystem
  and community

## Frontend: SVG + HTML/CSS

**Decision:** SVG for graphics, HTML/CSS for UI

**Key Reasons:**

1. **Vector Graphics:** Perfect for traffic
   simulation visualization
2. **DOM Integration:** Easy to style
   and interact with
3. **Responsive:** Scales perfectly
   on any device
4. **Accessibility:** Native support
   for screen readers
5. **Debugging:** Browser Inspector
   shows all objects

## Documentation: Diátaxis Framework

**Decision:** Organize documentation by learning type

**Key Reasons:**

1. **User-Centric:** Different users
   have different needs
2. **Industry Standard:** Used by Django,
   FastAPI, Kubernetes
3. **Scalability:** Easy to add
   new documentation
4. **Clarity:** Clear purpose
   for each document type

See [Project Structure] for details.

## Internationalization: Pluggable Rules + JSON Locales

**Decision:** Separate traffic logic from UI strings

**Key Reasons:**

1. **Flexibility:** Add new languages
   without code changes
2. **Scalability:** Support RHT and LHT
   traffic rules
3. **Maintainability:** Translations
   managed separately
4. **Community:** Easy for contributors
   to add languages

See [Internationalization Strategy] for details.

## Related Documentation

- [Rendering Engine Choice] - SVG vs Canvas detailed comparison
- [Internationalization Strategy] - Localization approach
- [Project Structure] - Directory layout and organization
- [Architecture] - System design and data flow

<!-- Reference Links -->

[Architecture]: ./docs/reference/architecture.md
[Internationalization Strategy]: ./docs/explanation/internationalization.md
[Project Structure]: ./docs/reference/project-structure.md
[Rendering Engine Choice]: ./docs/explanation/rendering-choice.md