# Evolutionary Vicsek Model with Velocity and Alignment Evolution

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A single-file HTML/JavaScript implementation of an extended Vicsek model that incorporates evolutionary dynamics with a focus on velocity and alignment strength evolution.

## Overview

This simulation extends the classical Vicsek model of collective motion by adding:

1. **Circular Genetic Parameter Space** - Each agent has a genetic identity represented as a position on a circle (0-2π)
2. **Evolvable Movement Speeds** - Agents can evolve different velocities through mutation and selection
3. **Evolvable Alignment Strengths** - Agents can evolve how strongly they align with or avoid others
4. **Genetic-Based Alignment Rules** - Similar genetic types align, different types anti-align
5. **Phase Transitions** - The system exhibits distinct phases including mixed populations, multiple species, and monoculture

## Key Features

### Self-Contained HTML File
- No dependencies or installation needed
- Just open the HTML file in any modern browser to run

### Interactive Controls
- Real-time parameter adjustment via sliders
- Toggle evolution of velocity and alignment strength
- Adjust selection strength, mutation rates, and population parameters

### Live Visualization
- Color-coded agents based on genetic parameter
- Size indicates velocity (faster = larger)
- Direction indicators show movement direction
- Colored rings show alignment strength (blue = strong, red = weak)
- Polar plot shows genetic parameter distribution

### Statistics and Analytics
- Population tracking
- Genetic cluster detection
- Phase state identification
- Real-time display of evolutionary statistics (averages and ranges)

## Scientific Background

### The Vicsek Model

The original Vicsek model (Vicsek et al., 1995) is a minimal model of collective motion where particles:
1. Move at a constant speed
2. Adjust their direction based on the average direction of neighboring particles
3. Add some random noise to their direction

It demonstrates how local alignment rules can lead to global order and flocking behavior.

### Our Extensions

This implementation adds evolutionary dynamics to the Vicsek model by allowing:

1. **Genetic-Based Alignment**:
   - Agents align with genetically similar neighbors
   - Agents anti-align (avoid) genetically different neighbors
   - The threshold between alignment and anti-alignment is configurable

2. **Evolvable Velocity**:
   - Agents can develop different movement speeds through mutations
   - Selection can favor faster or slower agents depending on context

3. **Evolvable Alignment Strength**:
   - Agents can evolve to be more or less influenced by neighbors
   - Some may develop strong alignment preferences, others weak

## Mathematical Model

### Agent Properties

Each agent has:
- Position (x, y) in 2D space
- Movement direction θ
- Genetic parameter φ ∈ [0, 2π) (position on a circle)
- Velocity v (evolvable)
- Alignment strength s (evolvable)

### Movement Update

At each time step, agents update their position:

```
x(t+1) = x(t) + v * cos(θ(t))
y(t+1) = y(t) + v * sin(θ(t))
```

With periodic boundary conditions.

### Direction Update

The heading direction is updated based on neighbor alignment:

```
θ(t+1) = Arg[ Σ w_ij * e^(iθ_j(t)) + e^(iθ_i(t)) ] + η(t)
```

Where:
- w_ij is the alignment weight between agents i and j
- η(t) is random noise

### Alignment Weight Calculation

The alignment weight depends on genetic distance:

```
w_ij = {
  s_i * (1 - d_ij/d_threshold)           if d_ij < d_threshold
  -s_i * (d_ij - d_threshold)/(1 - d_threshold)   if d_ij ≥ d_threshold
}
```

Where:
- s_i is agent i's alignment strength (evolvable)
- d_ij is the normalized genetic distance between agents i and j
- d_threshold is the global genetic threshold parameter

### Genetic Distance

The genetic distance is calculated on a circle to ensure symmetry:

```
d_ij = min(|φ_i - φ_j|, 2π - |φ_i - φ_j|) / π
```

### Reproduction and Mutation

Agents can reproduce with probability determined by replication rate. Offspring inherit attributes with possible mutations:

```
v_offspring = v_parent + random(-μ_v, μ_v)
s_offspring = s_parent + random(-μ_s, μ_s)
```

Where μ_v and μ_s are the mutation strength parameters for velocity and alignment.

## Phase Transitions

The system exhibits several distinct phases:

### 1. Mixed (Disordered) Phase

When noise is high or selection weak:
- Genetic parameters remain evenly distributed
- No distinct clusters form
- Agents move in random directions

### 2. Multiple Species Phase

At moderate noise and selection:
- Population separates into multiple genetic clusters
- Each cluster behaves as a "species" with internal alignment
- Different species move in different directions
- Some species may evolve faster or slower velocities

### 3. Monoculture (Ordered) Phase

When noise is low and selection strong:
- One genetic variant dominates the population
- Strong genetic homogeneity
- All agents move in the same direction

## Parameters and Controls

### Movement Parameters
- **Velocity**: Base movement speed (can evolve)
- **Interaction Radius**: How far agents can detect neighbors
- **Noise Level**: Random direction fluctuations

### Alignment Parameters
- **Alignment Strength**: Base strength of alignment/anti-alignment (can evolve)
- **Genetic Threshold**: Distance at which alignment switches to anti-alignment

### Evolutionary Parameters
- **Replication Rate**: Probability of reproduction per time step
- **Mutation Rate**: Probability of mutation during reproduction
- **Mutation Strength**: Magnitude of potential mutations
- **Selection Strength**: How strongly genetic compatibility affects survival
- **Max Capacity**: Environmental carrying capacity

## How to Use

1. Download the HTML file
2. Open it in any modern web browser
3. Use the sliders to adjust parameters
4. Toggle evolution checkboxes to enable/disable specific evolutionary features
5. Observe the emergence of movement patterns and genetic clusters

## Example Experiments

### Experiment 1: Velocity Specialization

1. Start with moderate noise (0.3)
2. Enable only velocity evolution
3. Observe how different genetic clusters evolve different speeds

### Experiment 2: Alignment Strength Evolution

1. Enable only alignment strength evolution
2. Set genetic threshold to 0.25
3. Observe how some genetic types evolve to be more "social" (strong alignment)
4. While others become more "independent" (weak alignment)

### Experiment 3: Co-Evolution Effects

1. Enable both velocity and alignment evolution
2. Observe how these traits evolve together:
   - Do faster agents develop different alignment preferences?
   - Do strongly aligning agents move at different speeds?

## Credits and References

1. Vicsek, T., Czirók, A., Ben-Jacob, E., Cohen, I., & Shochet, O. (1995). Novel type of phase transition in a system of self-driven particles. *Physical Review Letters, 75*(6), 1226-1229.

2. Couzin, I. D., Krause, J., James, R., Ruxton, G. D., & Franks, N. R. (2002). Collective memory and spatial sorting in animal groups. *Journal of Theoretical Biology, 218*(1), 1-11.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**Note:** This simulation is designed for educational and research purposes. It provides a simplified model of complex evolutionary and collective behavior processes.