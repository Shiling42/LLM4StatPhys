# Evolutionary Vicsek Model with Symmetric Genetic Parameters

## Model Introduction

The Evolutionary Vicsek Model with Symmetric Genetic Parameters extends the classical Vicsek model of collective motion to incorporate evolutionary dynamics in a continuous genetic parameter space. This model explores how genetic traits influence alignment behaviors, leading to phenomena like speciation, phase transitions, and collective adaptation.

### Theoretical Background

The classical Vicsek model, introduced by Tamás Vicsek et al. in 1995, describes the emergence of order in systems of self-propelled particles. In the standard model, particles move at a constant speed and adjust their direction based on the average direction of neighboring particles, plus some noise.

Our extended model introduces several key innovations:

1. **Symmetric Genetic Parameters**: Each agent possesses a genetic parameter $\phi \in [0, 2\pi)$ represented as a position on a circle, ensuring no intrinsic preference for any genetic state.
2. **Genetic-Based Alignment**: Agents align or anti-align their movement directions based on genetic similarity.
3. **Evolutionary Dynamics**: Agents can reproduce with mutations and experience selection pressures based on their genetic compatibility with neighbors.
4. **Phase Transitions**: The system exhibits distinct phases (mixed, multiple species, monoculture) depending on control parameters.

This model serves as a minimal framework for studying how local interactions and evolutionary pressures can lead to the emergence of distinct species and complex collective behaviors.

## Mathematical Formulation

### Agent Properties

Each agent $i$ is characterized by:

- Position $\vec{r}_i(t) = (x_i(t), y_i(t))$
- Heading direction $\theta_i(t)$
- Genetic parameter $\phi_i(t) \in [0, 2\pi)$
- Age $a_i(t)$

### Movement Dynamics

Agents move with constant speed $v$ according to:

$$
\vec{r}_i(t+1) = \vec{r}_i(t) + v \begin{pmatrix} \cos \theta_i(t) \ \sin \theta_i(t) \end{pmatrix}
$$

With periodic boundary conditions applied to keep agents within the simulation area.

### Direction Update

The heading direction is updated based on alignment with neighboring agents:

$$\theta_i(t+1) = \text{Arg}\left( \sum_{j \in \mathcal{N}*i} w*{ij} e^{i\theta_j(t)} + e^{i\theta_i(t)} \right) + \eta_i(t)$$

Where:

- $\mathcal{N}_i$ is the set of neighbors within interaction radius $R$ of agent $i$
- $w_{ij}$ is the alignment weight between agents $i$ and $j$
- $\eta_i(t)$ is random noise drawn from a uniform distribution $[-\eta_{\max}/2, \eta_{\max}/2]$
- $\text{Arg}(z)$ gives the angle of complex number $z$

### Alignment Weight

The alignment weight $w_{ij}$ depends on genetic distance:

$$w_{ij} = \begin{cases} s_{\text{align}} \cdot (1 - d_{ij}/d_{\text{threshold}}) & \text{if } d_{ij} < d_{\text{threshold}} \ -s_{\text{align}} \cdot \min(1, \frac{d_{ij} - d_{\text{threshold}}}{d_{\text{threshold}}}) & \text{if } d_{ij} \geq d_{\text{threshold}} \end{cases}$$

Where:

- $s_{\text{align}}$ is the alignment strength parameter
- $d_{ij}$ is the genetic distance between agents $i$ and $j$
- $d_{\text{threshold}}$ is the genetic threshold parameter

### Genetic Distance

The genetic distance is calculated on the circle to ensure symmetry:

$$d_{ij} = \min(|\phi_i - \phi_j|, 2\pi - |\phi_i - \phi_j|)$$

### Evolutionary Dynamics

The model includes:

1. **Reproduction**: Each agent has a probability $p_{\text{rep}}$ of reproducing per time step.

2. **Mutation**: Offspring may have mutations in their genetic parameter: $$\phi_{\text{offspring}} = (\phi_{\text{parent}} + \Delta\phi) \mod 2\pi$$ Where $\Delta\phi$ is drawn from a uniform distribution $[-s_{\text{mut}} \cdot \pi, s_{\text{mut}} \cdot \pi]$, and $s_{\text{mut}}$ is the mutation strength.

3. **Selection**: Each agent's survival probability depends on genetic compatibility with neighbors: $$p_{\text{survival}} = 1 + s_{\text{sel}} \cdot \frac{1}{|\mathcal{N}*i|} \sum*{j \in \mathcal{N}*i} f(d*{ij})$$

   Where $f(d_{ij})$ is positive for similar agents and negative for dissimilar agents: $$f(d_{ij}) = \begin{cases} 1 - d_{ij}/d_{\text{threshold}} & \text{if } d_{ij} < d_{\text{threshold}} \ -(d_{ij} - d_{\text{threshold}})/(1 - d_{\text{threshold}}) & \text{if } d_{ij} \geq d_{\text{threshold}} \end{cases}$$

### Order Parameter

To detect phase transitions, we calculate an order parameter $\psi$ that measures the degree of genetic clustering:

$$\psi = \frac{1}{N}\left|\sum_{i=1}^{N} e^{i\phi_i}\right|$$

Where $N$ is the total number of agents. When $\psi$ is close to 1, the population has a single dominant genetic cluster. When $\psi$ is close to 0, the genetic parameters are uniformly distributed.

## Parameters and Their Effects

### Movement Parameters

- **Velocity**: Controls agent movement speed. Higher values increase mixing.
- **Interaction Radius**: Determines the neighborhood size for alignment and selection.
- **Noise Level**: Random fluctuations in direction. Higher noise disrupts ordered structures.

### Alignment Parameters

- **Alignment Strength**: The magnitude of alignment/anti-alignment effects.
- **Genetic Threshold**: The genetic distance at which alignment switches from positive to negative.
- **Pairwise Interaction**: Toggle between pairwise interactions and average direction model.

### Evolutionary Parameters

- **Replication Rate**: Controls how quickly the population grows.
- **Mutation Rate**: Probability of genetic mutation during reproduction.
- **Mutation Strength**: Magnitude of potential genetic changes.
- **Selection Strength**: How strongly genetic compatibility affects survival.
- **Max Capacity**: Environmental carrying capacity limiting population size.

## Phase Transitions and Emergent Behaviors

The system exhibits several distinct phases:

### 1. Mixed (Disordered) Phase

When noise is high or selection strength is low, genetic parameters remain evenly distributed. The order parameter $\psi$ is close to 0, and no distinct clusters form.

### 2. Multiple Species Phase

At intermediate noise and selection values, the population separates into multiple distinct genetic clusters. These represent different "species" that maintain genetic separation through the alignment behaviors.

### 3. Monoculture (Ordered) Phase

When noise is low and selection is high, one genetic variant dominates the population. The order parameter $\psi$ approaches 1, and the population shows strong genetic homogeneity.

## Critical Phase Transitions

The model exhibits critical phase transitions where small changes in control parameters cause dramatic shifts in system behavior. These transitions are characterized by:

1. **Order-Disorder Transition**: As noise increases past a critical value, the system shifts from ordered clusters to a disordered state.
2. **Speciation Transition**: As genetic threshold decreases, the system transitions from a single species to multiple distinct genetic clusters.
3. **Population Collapse**: At extreme parameter values, unstable dynamics can lead to population crashes.

## Suggested Experiments

### Experiment 1: Noise-Induced Phase Transition

1. Start with low noise (0.1) and moderate selection strength (0.05)
2. Gradually increase noise to observe the transition from ordered to disordered states
3. Monitor the order parameter to identify the critical noise level

### Experiment 2: Speciation Dynamics

1. Set moderate noise (0.3) and genetic threshold (0.2)
2. Observe how initial random distribution evolves into distinct genetic clusters
3. Experiment with different mutation rates to see their effect on species stability

### Experiment 3: Competition and Coexistence

1. Start with high mutation rate (0.5) and low initial diversity (0.1)
2. Observe whether multiple genetic variants can stably coexist
3. Adjust selection strength to find the balance between competition and coexistence

## Implementation Details

The simulation is implemented in JavaScript and HTML5 Canvas, with the following key components:

1. **Main Simulation Loop**: Updates agent positions, directions, and genetic parameters.
2. **Agent Interaction**: Calculates alignment forces between agents based on genetic distance.
3. **Evolutionary Processes**: Handles reproduction, mutation, and selection.
4. **Visualization**: Renders agents in the simulation space and displays the genetic parameter distribution in polar coordinates.
5. **Analytics**: Calculates order parameters and identifies genetic clusters to determine the system's phase.

## Further Extensions

Possible extensions to this model include:

1. **Environmental Heterogeneity**: Adding spatial variation that favors different genetic parameters in different regions.
2. **Multi-Dimensional Genetic Space**: Extending the genetic parameter to multiple dimensions.
3. **Learning and Adaptation**: Adding the capacity for agents to adjust their behavior based on past experiences.
4. **Network Topology**: Exploring how different interaction network structures affect evolutionary dynamics.
5. **Predator-Prey Dynamics**: Adding a second type of agent that preys on the first, creating evolutionary pressure for defensive behaviors.

## References

1. Vicsek, T., Czirók, A., Ben-Jacob, E., Cohen, I., & Shochet, O. (1995). Novel type of phase transition in a system of self-driven particles. Physical Review Letters, 75(6), 1226-1229.
2. Couzin, I. D., Krause, J., James, R., Ruxton, G. D., & Franks, N. R. (2002). Collective memory and spatial sorting in animal groups. Journal of Theoretical Biology, 218(1), 1-11.
3. Joshi, J., Couzin, I. D., Levin, S. A., & Guttal, V. (2017). Mobility can promote the evolution of cooperation via emergent self-assortment dynamics. PLOS Computational Biology, 13(9), e1005732.
4. Szolnoki, A., & Perc, M. (2016). Biodiversity in models of cyclic dominance is preserved by heterogeneity in site-specific invasion rates. Scientific Reports, 6, 38608.