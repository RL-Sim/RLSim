# Traffic Rule Specifications

Detailed specifications for traffic rules implemented in RLSim.

## Overview

This document defines the traffic rules and priority logic used in RLSim simulations.
Rules are implemented as pluggable traits to support multiple traffic conventions
and regional variations.

## Core Concepts

### Priority Engine Trait

All traffic rules are implemented through the `PriorityEngine` trait:

```rust
pub trait PriorityEngine {
    /// Check if a vehicle has priority at an intersection
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        other_vehicles: &[Car]
    ) -> bool;
    
    /// Get the human-readable name of this rule
    fn get_rule_name(&self) -> &str;
    
    /// Get the rule description
    fn get_description(&self) -> &str;
}
```

### Intersection Model

An intersection is defined by:

```rust
pub struct Intersection {
    /// Unique identifier
    pub id: u32,
    
    /// Center coordinates
    pub center: (f64, f64),
    
    /// Intersection type (unregulated, traffic light, roundabout)
    pub intersection_type: IntersectionType,
    
    /// Priority rule applied
    pub priority_rule: Box<dyn PriorityEngine>,
    
    /// Scan zone radius for detecting vehicles
    pub scan_radius: f64,
}

pub enum IntersectionType {
    Unregulated,
    TrafficLight,
    Roundabout,
    YieldSign,
    StopSign,
}
```

### Vehicle State at Intersection

```rust
pub struct VehicleState {
    /// Current position
    pub position: (f64, f64),
    
    /// Current velocity (m/s)
    pub velocity: f64,
    
    /// Target velocity (m/s)
    pub target_velocity: f64,
    
    /// Direction of travel (0-360 degrees)
    pub direction: f64,
    
    /// Lane identifier
    pub lane: u32,
    
    /// Distance to intersection
    pub distance_to_intersection: f64,
}
```

## Right-Hand Traffic (RHT) Rules

### Right-before-Left (`Rechts vor Links`)

**Applies to:** 🇩🇪 Germany, 🇲🇩 Moldova, 🇷🇴 Romania, 🇷🇺 Russia, and other RHT countries

**Rule Definition:**
At an unregulated intersection, a vehicle must yield to vehicles approaching from the right.

**Implementation Logic:**

```rust
pub struct RightPriority;

impl PriorityEngine for RightPriority {
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        other_vehicles: &[Car]
    ) -> bool {
        // 1. Check if vehicle is in scan zone
        let distance = calculate_distance(vehicle.position, intersection.center);
        if distance > intersection.scan_radius {
            return true; // Not in intersection, has priority
        }
        
        // 2. Determine vehicle's direction (0-360 degrees)
        let vehicle_direction = vehicle.direction;
        
        // 3. Check for vehicles from the right
        for other in other_vehicles {
            if !is_in_scan_zone(other, intersection) {
                continue;
            }
            
            // Calculate relative direction
            let relative_direction = calculate_relative_direction(
                vehicle_direction,
                other.direction
            );
            
            // Vehicle from right is between 270-90 degrees (right side)
            if is_from_right(relative_direction) {
                // Check if other vehicle is closer to intersection
                let other_distance = calculate_distance(
                    other.position,
                    intersection.center
                );
                
                if other_distance < distance {
                    return false; // Must yield
                }
            }
        }
        
        true // Has priority
    }
    
    fn get_rule_name(&self) -> &str {
        "Right-before-Left"
    }
    
    fn get_description(&self) -> &str {
        "Vehicles from the right have priority at unregulated intersections"
    }
}
```

**Priority Determination:**

1. **Scan Zone Detection** - Vehicle must be within scan radius of intersection
2. **Direction Calculation** - Determine vehicle's approach direction
3. **Right-Side Check** - Identify vehicles approaching from the right (270-90°)
4. **Distance Comparison** - Compare distances to intersection center
5. **Yield Decision** - Yield if vehicle from right is closer

**Edge Cases:**

- **Parallel Vehicles:** Vehicles on same lane have no priority conflict
- **Opposite Vehicles:** Vehicles from opposite direction (180°) have no priority conflict
- **Diagonal Vehicles:** Vehicles from left (90-270°) have no priority conflict
- **Simultaneous Arrival:** Both vehicles equally close → mutual yielding required

### Scan Zone Parameters

```rust
pub struct ScanZoneConfig {
    /// Radius in meters for detecting vehicles
    pub radius: f64,  // Default: 50m
    
    /// Minimum distance to intersection for priority check
    pub min_distance: f64,  // Default: 5m
    
    /// Maximum distance for priority consideration
    pub max_distance: f64,  // Default: 50m
    
    /// Angular tolerance in degrees
    pub angular_tolerance: f64,  // Default: 15°
}
```

## Left-Hand Traffic (LHT) Rules

### Left-before-Left (Priority to the Left)

**Applies to:** 🇬🇧 UK, 🇯🇵 Japan, 🇮🇳 India, 🇦🇺 Australia, and other LHT countries

**Rule Definition:**
At an unregulated intersection, a vehicle must yield to vehicles approaching from the left.

**Implementation Logic:**

```rust
pub struct LeftPriority;

impl PriorityEngine for LeftPriority {
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        other_vehicles: &[Car]
    ) -> bool {
        // Similar to RightPriority but checks left side (90-270°)
        // instead of right side (270-90°)
        
        let distance = calculate_distance(vehicle.position, intersection.center);
        if distance > intersection.scan_radius {
            return true;
        }
        
        let vehicle_direction = vehicle.direction;
        
        for other in other_vehicles {
            if !is_in_scan_zone(other, intersection) {
                continue;
            }
            
            let relative_direction = calculate_relative_direction(
                vehicle_direction,
                other.direction
            );
            
            // Vehicle from left is between 90-270 degrees
            if is_from_left(relative_direction) {
                let other_distance = calculate_distance(
                    other.position,
                    intersection.center
                );
                
                if other_distance < distance {
                    return false; // Must yield
                }
            }
        }
        
        true // Has priority
    }
    
    fn get_rule_name(&self) -> &str {
        "Left-before-Left"
    }
    
    fn get_description(&self) -> &str {
        "Vehicles from the left have priority at unregulated intersections"
    }
}
```

## Special Intersection Types

### Traffic Light Intersections

**Rule:** Traffic lights override all priority rules

```rust
pub struct TrafficLightRule;

impl PriorityEngine for TrafficLightRule {
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        _other_vehicles: &[Car]
    ) -> bool {
        // Check traffic light state
        match intersection.traffic_light_state {
            TrafficLightState::Green => true,
            TrafficLightState::Yellow => {
                // Vehicle can proceed if close enough
                let distance = calculate_distance(
                    vehicle.position,
                    intersection.center
                );
                distance < YELLOW_THRESHOLD
            }
            TrafficLightState::Red => false,
        }
    }
    
    fn get_rule_name(&self) -> &str {
        "Traffic Light"
    }
    
    fn get_description(&self) -> &str {
        "Traffic light signals override all other priority rules"
    }
}
```

### Roundabout Rules

**Rule:** Vehicles in roundabout have priority over entering vehicles

```rust
pub struct RoundaboutRule;

impl PriorityEngine for RoundaboutRule {
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        other_vehicles: &[Car]
    ) -> bool {
        // Check if vehicle is already in roundabout
        if vehicle.in_roundabout {
            return true; // Vehicle in roundabout has priority
        }
        
        // Check for vehicles in roundabout
        for other in other_vehicles {
            if other.in_roundabout {
                let distance = calculate_distance(
                    other.position,
                    intersection.center
                );
                
                // If vehicle in roundabout is close, must yield
                if distance < ROUNDABOUT_THRESHOLD {
                    return false;
                }
            }
        }
        
        true // Can enter roundabout
    }
    
    fn get_rule_name(&self) -> &str {
        "Roundabout Priority"
    }
    
    fn get_description(&self) -> &str {
        "Vehicles in roundabout have priority over entering vehicles"
    }
}
```

### Yield Sign Rule

**Rule:** Vehicle with yield sign must yield to all other vehicles

```rust
pub struct YieldSignRule;

impl PriorityEngine for YieldSignRule {
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        other_vehicles: &[Car]
    ) -> bool {
        // Check if vehicle has yield sign
        if vehicle.has_yield_sign {
            // Must yield to any vehicle in intersection
            for other in other_vehicles {
                if is_in_scan_zone(other, intersection) {
                    return false; // Must yield
                }
            }
        }
        
        true // No yield sign or no conflicting vehicles
    }
    
    fn get_rule_name(&self) -> &str {
        "Yield Sign"
    }
    
    fn get_description(&self) -> &str {
        "Vehicles with yield signs must yield to all other traffic"
    }
}
```

### Stop Sign Rule

**Rule:** Vehicle must come to complete stop before proceeding

```rust
pub struct StopSignRule;

impl PriorityEngine for StopSignRule {
    fn check_priority(
        &self,
        vehicle: &Car,
        intersection: &Intersection,
        other_vehicles: &[Car]
    ) -> bool {
        // Check if vehicle has stop sign
        if vehicle.has_stop_sign {
            // Must be at complete stop
            if vehicle.velocity > STOP_THRESHOLD {
                return false; // Still moving, must stop
            }
            
            // After stopping, apply right-of-way rules
            // (typically right-before-left or left-before-left)
            return check_right_of_way(vehicle, intersection, other_vehicles);
        }
        
        true // No stop sign
    }
    
    fn get_rule_name(&self) -> &str {
        "Stop Sign"
    }
    
    fn get_description(&self) -> &str {
        "Vehicles must come to complete stop before proceeding"
    }
}
```

## Deadlock Resolution

### Four-Way Stop Scenario

When all four vehicles arrive simultaneously at an unregulated intersection:

```rust
pub fn resolve_deadlock(
    vehicles: &[Car],
    intersection: &Intersection
) -> Vec<bool> {
    // 1. Check if all vehicles have equal priority
    let priorities: Vec<bool> = vehicles.iter()
        .map(|v| intersection.priority_rule.check_priority(v, intersection, vehicles))
        .collect();
    
    // 2. If all have priority (deadlock), apply tiebreaker
    if priorities.iter().all(|&p| p) {
        return apply_tiebreaker(vehicles, intersection);
    }
    
    priorities
}

pub fn apply_tiebreaker(
    vehicles: &[Car],
    intersection: &Intersection
) -> Vec<bool> {
    // Tiebreaker rules (in order of preference):
    // 1. Vehicle going straight has priority over turning
    // 2. Vehicle turning left has priority over turning right
    // 3. Vehicle arriving first has priority
    // 4. Random selection (mutual yielding)
    
    let mut priorities = vec![false; vehicles.len()];
    
    // Check for straight-going vehicles
    let straight_vehicles: Vec<usize> = vehicles.iter()
        .enumerate()
        .filter(|(_, v)| v.is_going_straight())
        .map(|(i, _)| i)
        .collect();
    
    if !straight_vehicles.is_empty() {
        for i in straight_vehicles {
            priorities[i] = true;
        }
        return priorities;
    }
    
    // Check for left-turning vehicles
    let left_turn_vehicles: Vec<usize> = vehicles.iter()
        .enumerate()
        .filter(|(_, v)| v.is_turning_left())
        .map(|(i, _)| i)
        .collect();
    
    if !left_turn_vehicles.is_empty() {
        for i in left_turn_vehicles {
            priorities[i] = true;
        }
        return priorities;
    }
    
    // All turning right - apply arrival time tiebreaker
    let earliest_arrival = vehicles.iter()
        .enumerate()
        .min_by_key(|(_, v)| v.arrival_time)
        .map(|(i, _)| i);
    
    if let Some(i) = earliest_arrival {
        priorities[i] = true;
    }
    
    priorities
}
```

## Velocity Control

### Acceleration and Deceleration

```rust
pub struct VelocityControl {
    /// Maximum acceleration (m/s²)
    pub max_acceleration: f64,  // Default: 2.0
    
    /// Maximum deceleration (m/s²)
    pub max_deceleration: f64,  // Default: 4.0
    
    /// Comfortable deceleration (m/s²)
    pub comfortable_deceleration: f64,  // Default: 1.5
}

pub fn update_velocity(
    vehicle: &mut Car,
    has_priority: bool,
    control: &VelocityControl,
    delta_time: f64
) {
    if has_priority {
        // Accelerate towards target velocity
        let acceleration = control.max_acceleration;
        vehicle.velocity = (vehicle.velocity + acceleration * delta_time)
            .min(vehicle.target_velocity);
    } else {
        // Decelerate to stop
        let deceleration = control.comfortable_deceleration;
        vehicle.velocity = (vehicle.velocity - deceleration * delta_time)
            .max(0.0);
    }
}
```

## Configuration Parameters

### Default Parameters

```rust
pub struct RLSimConfig {
    // Scan zone parameters
    pub scan_radius: f64,  // 50m
    pub angular_tolerance: f64,  // 15°
    
    // Velocity parameters
    pub max_acceleration: f64,  // 2.0 m/s²
    pub max_deceleration: f64,  // 4.0 m/s²
    pub comfortable_deceleration: f64,  // 1.5 m/s²
    
    // Intersection parameters
    pub yellow_light_threshold: f64,  // 20m
    pub roundabout_threshold: f64,  // 30m
    pub stop_threshold: f64,  // 0.1 m/s
    
    // Simulation parameters
    pub simulation_step: f64,  // 0.016s (60 FPS)
    pub max_vehicles: usize,  // 100
}
```

## Testing and Validation

### Unit Tests

```rust
#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn test_right_priority_basic() {
        // Vehicle from right should have priority
        let vehicle = create_test_vehicle(direction: 0.0);
        let other = create_test_vehicle(direction: 90.0);
        
        assert!(right_priority.check_priority(&vehicle, &intersection, &[other]));
    }
    
    #[test]
    fn test_deadlock_resolution() {
        // Four vehicles arriving simultaneously
        let vehicles = vec![
            create_test_vehicle(direction: 0.0),
            create_test_vehicle(direction: 90.0),
            create_test_vehicle(direction: 180.0),
            create_test_vehicle(direction: 270.0),
        ];
        
        let priorities = resolve_deadlock(&vehicles, &intersection);
        assert!(priorities.iter().any(|&p| p)); // At least one has priority
    }
}
```

## Related Documentation

- [Traffic Rule Theory] - Links to official regulations
- [Internationalization Strategy] - Localization approach
- [Project Structure] - Directory layout and file descriptions
- [Architecture] - System design and data flow

<!-- Reference Links -->

[Architecture]: ./architecture.md
[Internationalization Strategy]: ../explanation/internationalization.md
[Project Structure]: ./project-structure.md
[Traffic Rule Theory]: ../explanation/traffic-theory.md
