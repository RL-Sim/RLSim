# Internationalization Strategy

This document explains the design philosophy and rollout strategy for supporting
multiple languages and traffic rule variations in RLSim.

## Overview

RLSim is designed to support global deployment with localized interfaces
and region-specific traffic rules.
This guide explains how we approach internationalization (i18n)
and the phased rollout strategy.

## Core Principles

### Separation of Logic and Localization

Traffic rule logic is abstracted from UI strings,
enabling independent updates to rules and translations.

- **Traffic Rules:** Implemented as pluggable `PriorityEngine` traits
- **UI Strings:** Stored in JSON locale files (`src/locales/`)
- **Benefit:** Add new languages without modifying Rust code

### Right-Hand Traffic (RHT) vs Left-Hand Traffic (LHT)

The system supports both traffic conventions:

- **RHT Countries:** 🇩🇪 Germany, 🇷🇴 Romania, 🇷🇺 Russia, 🇺🇸 USA, 🇫🇷 France, etc.
- **LHT Countries:** 🇬🇧 UK, 🇯🇵 Japan, 🇮🇳 India, 🇦🇺 Australia, etc.

Each region can have its own priority rules and UI language.

## Phased Rollout Strategy

The localization rollout follows a strategic geographic and linguistic approach,
prioritizing regions with high development activity and clear language clusters.

### Phase 1: Core English Foundation

**Target:** All regions
**Content:** English version as the universal baseline

- English UI strings
- English documentation
- English traffic rule specifications

### Phase 2: German-Speaking Region (🇩🇪 Germany)

**Target:** 🇩🇪 Germany
**Rollout Order:**

1. **English Version** - English UI for German users
2. **English → German Translation** - German UI for German users
3. **English → Romanian Translation** - Romanian UI for Romanian workers in Germany
4. **English → Russian Translation** - Russian UI for Russian workers in Germany

**Rationale:** 🇩🇪 Germany has significant immigrant populations from 🇷🇴 Romania and 🇷🇺 Russia.
Providing multilingual support improves accessibility for all workers.

### Phase 3: 🇲🇩 Moldova

**Target:** 🇲🇩 Moldova
**Rollout Order:**

1. **English Version** - English UI for Moldovan users
2. **English → Romanian Translation** - Romanian UI (official language)
3. **English → Russian Translation** - Russian UI (widely spoken)

**Rationale:** 🇲🇩 Moldova has both Romanian and Russian speakers.
Romanian is the official language; Russian is widely understood.

### Phase 4: 🇷🇴 Romania

**Target:** 🇷🇴 Romania
**Rollout Order:**

1. **English Version** - English UI for Romanian users
2. **English → Romanian Translation** - Romanian UI (official language)
3. **English → Russian Translation** - Russian UI for Russian speakers

**Rationale:** 🇷🇴 Romania's official language is Romanian.
Russian support serves minority populations and cross-border workers.

### Phase 5: 🇷🇺 Russia

**Target:** 🇷🇺 Russia
**Rollout Order:**

1. **English Version** - English UI for Russian users
2. **English → Russian Translation** - Russian UI (official language)

**Rationale:** 🇷🇺 Russia's official language is Russian.
English serves international users and developers.

### Phase 6: Extended Global Support

**Target:** Other countries and languages
**Rollout Order (by region/language cluster):**

- **Balkans:** 🇧🇬 Bulgarian, 🇷🇸 Serbian, 🇭🇷 Croatian, 🇦🇱 Albanian
- **Eastern Europe:** 🇵🇱 Polish, 🇨🇿 Czech, 🇸🇰 Slovak, 🇭🇺 Hungarian
- **Nordic:** 🇸🇪 Swedish, 🇳🇴 Norwegian, 🇩🇰 Danish, 🇫🇮 Finnish
- **Western Europe:** 🇫🇷 French, 🇪🇸 Spanish, 🇮🇹 Italian, 🇳🇱 Dutch
- **Asia-Pacific:** 🇯🇵 Japanese, 🇨🇳 Chinese (Simplified/Traditional), 🇰🇷 Korean
- **Other:** 🇵🇹 Portuguese, 🇬🇷 Greek, 🇹🇷 Turkish, and more

**Approach:**
- Prioritize languages with existing community contributions
- Support regional traffic rule variations
- Maintain consistent UI/UX across all languages

## Implementation Details

### Locale File Structure

```bash
src/locales/
├── en.json              # English (base language)
├── de.json              # German
├── ro.json              # Romanian
├── ru.json              # Russian
├── fr.json              # French (future)
├── es.json              # Spanish (future)
├── ja.json              # Japanese (future)
└── ...
```

### Locale File Format

Each locale file contains UI strings and region-specific metadata:

```json
{
  "metadata": {
    "language": "de",
    "region": "DE",
    "name": "Deutsch (Deutschland)",
    "traffic_rule": "rht",
    "priority_direction": "right"
  },
  "ui": {
    "title": "Verkehrssimulator",
    "start_simulation": "Simulation starten",
    "pause": "Pausieren",
    "reset": "Zurücksetzen"
  },
  "rules": {
    "priority_rule": "Rechts vor Links",
    "description": "Fahrzeuge von rechts haben Vorrang"
  }
}
```

### Traffic Rule Abstraction

Traffic rules are implemented as pluggable traits:

```rust
pub trait PriorityEngine {
    fn check_priority(&self, vehicle: &Car, intersection: &Intersection) -> bool;
    fn get_rule_name(&self) -> &str;
}

pub struct RightPriority;
pub struct LeftPriority;

impl PriorityEngine for RightPriority {
    fn check_priority(&self, vehicle: &Car, intersection: &Intersection) -> bool {
        // Right-Hand Traffic logic
    }
    
    fn get_rule_name(&self) -> &str {
        "Right before Left"
    }
}
```

## Translation Workflow

### Step 1: Extract English Strings

All UI strings are extracted from Rust code into `en.json`:

```bash
cargo run --bin extract-strings
```

### Step 2: Create New Locale File

Copy `en.json` to new locale file:

```bash
cp src/locales/en.json src/locales/de.json
```

### Step 3: Translate Strings

Update the new locale file with translations:

```json
{
  "metadata": {
    "language": "de",
    "region": "DE",
    "name": "Deutsch (Deutschland)"
  },
  "ui": {
    "title": "Verkehrssimulator",
    ...
  }
}
```

### Step 4: Test Localization

Run tests to verify translations:

```bash
cargo test --features localization
```

### Step 5: Deploy

Build and deploy with new locale:

```bash
wasm-pack build --target web
```

## Regional Traffic Rules

Different regions have different traffic rules.
The system supports multiple rule implementations:

### Right-Hand Traffic (RHT)

**Countries:** 🇩🇪 Germany, 🇷🇴 Romania, 🇷🇺 Russia, 🇺🇸 USA, 🇫🇷 France, 🇪🇸 Spain, 🇮🇹 Italy, etc.

**Rule:** Vehicles from the right have priority at unregulated intersections.

**Implementation:** `RightPriority` trait

### Left-Hand Traffic (LHT)

**Countries:** 🇬🇧 UK, 🇯🇵 Japan, 🇮🇳 India, 🇦🇺 Australia, etc.

**Rule:** Vehicles from the left have priority at unregulated intersections.

**Implementation:** `LeftPriority` trait

### Custom Rules

Additional rules can be implemented for specific regions:

- **Roundabout Priority:** Vehicles in roundabout have priority
- **Yield Signs:** Explicit yield rules
- **Traffic Lights:** Signal-based priority

## Accessibility Considerations

### Language Selection

Users can select their preferred language:

- **UI Language:** Dropdown menu in settings
- **Persistent:** Saved in browser localStorage
- **Default:** Browser language preference

### Right-to-Left (RTL) Support

Future support for RTL languages (Arabic, Hebrew, Persian):

- CSS flexbox for RTL layout
- Text direction: `direction: rtl`
- Mirror UI elements as needed

### Accessibility Standards

All translations must meet:

- **WCAG 2.1 Level AA** - Web Content Accessibility Guidelines
- **Screen Reader Support** - Proper ARIA labels
- **Keyboard Navigation** - Full keyboard accessibility

## Community Contributions

### Translation Guidelines

Contributors can help translate RLSim:

1. **Fork the repository**
2. **Create a new locale file** in `src/locales/`
3. **Translate all strings** from `en.json`
4. **Test locally** with `cargo test`
5. **Submit a pull request**

### Translation Quality

All translations are reviewed for:

- **Accuracy** - Correct meaning and context
- **Consistency** - Uniform terminology
- **Completeness** - All strings translated
- **Cultural Appropriateness** - Region-specific considerations

## Future Enhancements

### Pluralization Support

Handle plural forms in different languages:

```json
{
  "vehicles": {
    "one": "1 Fahrzeug",
    "other": "{count} Fahrzeuge"
  }
}
```

### Date and Time Formatting

Localize date/time display:

- **🇩🇪 Germany:** `13.03.2026 20:53`
- **🇺🇸 USA:** `03/13/2026 8:53 PM`
- **ISO:** `2026-03-13T20:53:00Z`

### Number Formatting

Localize number display:

- **🇩🇪 Germany:** `1.234,56` (comma as decimal separator)
- **🇺🇸 USA:** `1,234.56` (period as decimal separator)

### Currency Support

Future support for region-specific currencies and pricing.

## Related Documentation

- [Architecture] - System design and data flow
- [Requirements] - Project requirements and constraints
- [Project Structure] - Directory layout and file descriptions

<!-- Reference Links -->

[Architecture]: ../reference/architecture.md
[Project Structure]: ../reference/project-structure.md
[Requirements]: ../reference/requirements.md
