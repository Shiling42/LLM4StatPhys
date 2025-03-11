# Evolutionary Vicsek Model with Symmetric Genetic Parameters

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A self-contained HTML/JavaScript implementation of an extended Vicsek model incorporating evolutionary dynamics with a continuous, symmetric genetic parameter space.

## 📖 Overview

This simulation extends the classical Vicsek model of collective motion by introducing:

1. A circular genetic parameter space (values on a circle from 0 to 2π)
2. Genetic-based alignment behavior (similar types align, different types anti-align)
3. Evolutionary dynamics (reproduction, mutation, selection)
4. Phase transitions between mixed populations and distinct genetic clusters

The model demonstrates how simple local alignment rules based on genetic similarity can drive the emergence of speciation and flocking behavior, without any intrinsic fitness advantage for specific genetic values.

## 🔬 Theoretical Background

### The Classical Vicsek Model

The Vicsek model (Vicsek et al., 1995) is a minimal model of collective motion where particles move at constant speed and adjust their direction based on the average direction of their neighbors plus some noise. It demonstrates how local alignment rules can lead to global order.

### Our Extensions

#### 1. Symmetric Genetic Parameters

Each agent possesses a genetic parameter $\phi \in [0, 2\pi)$ represented as a position on a circle. This representation ensures:

- No intrinsic preference for any genetic state (symmetry)
- Equal probability for all possible values
- Natural distance metric (minimum arc length on the circle)

#### 2. Alignment Based on Genetic Similarity

The core innovation in our model is that alignment strength depends on genetic similarity:

$$w_{ij} = \begin{cases}
s_{\text{align}} \cdot (1 - d_{ij}/d_{\text{threshold}}) & \text{if } d_{ij} < d_{\text{threshold}} \\
-s_{\text{align}} \cdot \frac{d_{ij} - d_{\text{threshold}}}{1 - d_{\text{threshold}}} & \text{if } d_{ij} \geq d_{\text{threshold}}
\end{cases}$$

Where:
- $s_{\text{align}}$ is the alignment strength parameter
- $d_{ij}$ is the normalized genetic distance between agents $i$ and $j$
- $d_{\text{threshold}}$ is the genetic threshold parameter

This creates two critical behaviors:
- **Positive alignment:** Genetically similar agents move in the same direction (flock together)
- **Negative alignment (anti-alignment):** Genetically different agents move in opposite directions (avoid each other)

#### 3. Evolutionary Dynamics

The model includes the following evolutionary processes:

- **Reproduction:** Agents can reproduce with a probability determined by the replication rate
- **Mutation:** Offspring may have mutations in their genetic parameter
- **Selection:** Survival probability depends on genetic compatibility with neighbors

#### 4. Order Parameter

To detect phase transitions, we calculate:

$$\psi = \frac{1}{N}\left|\sum_{i=1}^{N} e^{i\phi_i}\right|$$

This measures the degree of genetic clustering (0 = mixed, 1 = single cluster).

## 💻 Implementation Details

### Agent Properties

Each agent has:
- Position (x, y) in space
- Movement direction (θ)
- Genetic parameter (φ)
- Age (increments each time step)

### Update Rules

At each time step:

1. **Movement:** Agents move in their current direction with constant speed
2. **Direction Update:** Based on weighted average of neighbors' directions:
   ```
   θ_i(t+1) = Arg[ ∑_{j∈N_i} w_ij * e^(iθ_j(t)) + e^(iθ_i(t)) ] + η_i(t)
   ```
   Where N_i is the set of neighbors, w_ij is the alignment weight, and η_i(t) is random noise

3. **Reproduction:** Chance to create offspring with possible genetic mutations
4. **Selection:** Survival probability based on genetic compatibility with neighbors

### Genetic Distance

The genetic distance is calculated on the circle:

```javascript
function geneticDistance(param1, param2) {
    const rawDistance = Math.abs(param1 - param2);
    return Math.min(rawDistance, TWO_PI - rawDistance);
}
```

## 🎮 Parameters and Their Effects

### Movement Parameters

- **Velocity:** Controls agent movement speed
- **Interaction Radius:** Determines the neighborhood size for alignment and selection
- **Noise Level:** Random fluctuations in direction. Higher noise disrupts ordered structures.

### Alignment Parameters

- **Alignment Strength:** The magnitude of alignment/anti-alignment effects
- **Genetic Threshold:** Distance at which alignment switches from positive to negative
   - Lower values create more distinct species
   - Higher values promote more mixing
- **Pairwise Interaction:** Toggle between pairwise interactions (realistic) and average direction model (classic Vicsek)

### Evolutionary Parameters

- **Replication Rate:** Controls how quickly the population grows
- **Mutation Rate:** Probability of genetic mutation during reproduction
- **Mutation Strength:** Magnitude of potential genetic changes
- **Selection Strength:** How strongly genetic compatibility affects survival
- **Max Capacity:** Environmental carrying capacity limiting population size

## 🧬 Emergent Behaviors

### Phase Transitions

The system exhibits several distinct phases:

#### 1. Mixed (Disordered) Phase

When noise is high or selection strength is low:
- Genetic parameters remain evenly distributed around the circle
- No distinct clusters form
- Order parameter ≈ 0

#### 2. Multiple Species Phase

At intermediate noise and selection values:
- Population separates into multiple distinct genetic clusters
- Each cluster behaves as a "species" with internal alignment
- Different species move in different directions
- Order parameter ≈ 0.3-0.5

#### 3. Monoculture (Ordered) Phase

When noise is low and selection is high:
- One genetic variant dominates the population
- Strong genetic homogeneity
- Order parameter → 1

### Speciation Dynamics

The model demonstrates:

1. **Spontaneous Speciation:** Initial random distribution evolves into distinct clusters
2. **Stable Coexistence:** Multiple species can maintain separation and coexist
3. **Competitive Exclusion:** Under certain parameters, one species outcompetes others
4. **Cyclical Dynamics:** Species can chase each other around the genetic circle

## 🔍 Example Experiments

### Example 1: Species Formation

1. Set initial diversity to 0.8 (high)
2. Set genetic threshold to 0.15 (low)
3. Set noise level to 0.2
4. Watch as distinct species form from initial random distribution

### Example 2: Phase Transition

1. Start with noise = 0.5 (high)
2. Observe mixed population (disordered phase)
3. Gradually decrease noise to 0.1
4. Watch transition to ordered phase with distinct clusters

### Example 3: Competitive Dynamics

1. Set selection strength to 0.1 (high)
2. Set mutation rate to 0.1
3. Observe competition between emerging species

## 🧪 Mathematical Formulation

### Movement Dynamics

Agents move with constant speed $v$:

$$\vec{r}_i(t+1) = \vec{r}_i(t) + v \begin{pmatrix} \cos \theta_i(t) \\ \sin \theta_i(t) \end{pmatrix}$$

With periodic boundary conditions.

### Direction Update

The heading direction is updated based on neighbor alignment:

$$\theta_i(t+1) = \text{Arg}\left( \sum_{j \in \mathcal{N}_i} w_{ij} e^{i\theta_j(t)} + e^{i\theta_i(t)} \right) + \eta_i(t)$$

Where:
- $\mathcal{N}_i$ is the set of neighbors within interaction radius
- $w_{ij}$ is the alignment weight based on genetic similarity
- $\eta_i(t)$ is random noise from $[-\eta_{\max}/2, \eta_{\max}/2]$

### Reproduction and Mutation

Offspring inherit the parent's genetic parameter with possible mutation:

$$\phi_{\text{offspring}} = (\phi_{\text{parent}} + \Delta\phi) \mod 2\pi$$

Where $\Delta\phi$ is drawn from a uniform distribution with width proportional to the mutation strength.

### Selection Pressure

Survival probability depends on genetic compatibility with neighbors:

$$p_{\text{survival}} = 1 + s_{\text{sel}} \cdot \frac{1}{|\mathcal{N}_i|} \sum_{j \in \mathcal{N}_i} f(d_{ij})$$

Where $f(d_{ij})$ gives positive values for similar agents and negative values for dissimilar ones.

## 🔄 Visualization Components

The simulation includes several visualization components:

1. **Main Simulation Canvas:** Shows the agents moving in space
   - Each agent is colored based on its genetic parameter
   - Direction indicators show movement direction

2. **Genetic Polar Plot:** Visualizes the distribution of genetic parameters
   - Circular plot showing the density of agents at each genetic position
   - Helps track the formation and evolution of genetic clusters

3. **Statistics and Metrics:**
   - Genetic Order Parameter: Measures clustering of genetic parameters
   - Phase Indicator: Identifies the current system state
   - Population Count: Tracks total population size
   - Cluster Count: Identifies distinct genetic clusters

## 🔬 Scientific Applications

This model can be used to study:

1. **Speciation Mechanisms:** How movement rules and genetic preference can lead to reproductive isolation
2. **Critical Phenomena:** Phase transitions in biological collective behavior
3. **Biodiversity Maintenance:** Mechanisms that allow multiple species to coexist
4. **Self-Organization:** Emergence of complex patterns from simple local rules
5. **Evolutionary Game Theory:** How spatial dynamics affect competition between strategies

## 📚 References

1. Vicsek, T., Czirók, A., Ben-Jacob, E., Cohen, I., & Shochet, O. (1995). Novel type of phase transition in a system of self-driven particles. *Physical Review Letters, 75*(6), 1226-1229.

2. Couzin, I. D., Krause, J., James, R., Ruxton, G. D., & Franks, N. R. (2002). Collective memory and spatial sorting in animal groups. *Journal of Theoretical Biology, 218*(1), 1-11.

3. Edelstein-Keshet, L. (2001). Mathematical models of swarming and social aggregation. *Proceedings of the 2001 International Symposium on Nonlinear Theory and Its Applications, Miyagi, Japan*.

4. Szolnoki, A., & Perc, M. (2016). Biodiversity in models of cyclic dominance is preserved by heterogeneity in site-specific invasion rates. *Scientific Reports, 6*, 38608.

## 📄 Usage

1. Download the HTML file
2. Open it in any modern web browser
3. Use the controls to adjust parameters
4. Observe the emerging patterns and phase transitions

No installation, dependencies, or internet connection required.

---

**Note:** This simulation is designed for educational and research purposes. It provides a simplified model of complex evolutionary and collective behavior processes.