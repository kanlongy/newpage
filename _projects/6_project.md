---
layout: page
title: WindyRL
description: Temporal Reinforcement Learning for Vision-Based UAV Navigation in Windy Environments
img: assets/img/wdnavrl.gif
importance: 4
category: graduate
github: https://github.com/kanlongy/Windy-NavRL
---

## Overview

[Code](https://github.com/kanlongy/Windy-NavRL.git) | [Paper (ongoing)]() | [Video](https://drive.google.com/file/d/1A6-UCVXQLrekkfOmB5D9DYdLtQGsF_rT/view)

**Duration:** May 2025 – Jan 2026  
**Domain:** Reinforcement Learning, UAV Navigation, Wind Disturbance Robustness  
**Simulation:** Isaac Sim, Gazebo  
**Core Methods:** PPO, Transformer-based temporal policy, domain randomization  
**Platform:** Quadrotor UAV (velocity-command abstraction)

WindyRL is a vision-based reinforcement learning framework for robust UAV navigation under stochastic wind disturbances. Built on top of NavRL, WindyRL addresses a key challenge in real-world flight: **wind-induced partial observability**, where disturbances cannot be directly inferred from a single visual observation.

Instead of estimating wind explicitly or relying on wind sensors, WindyRL enables the policy to implicitly infer wind effects from temporal state histories, achieving strong zero-shot sim-to-sim and sim-to-real transfer.

**Key highlights:**
- Camera-only (RGB-D) perception
- Temporal policy learning (LSTM & Transformer)
- Disturbance-aware training via wind randomization
- Zero-shot transfer: Isaac Sim → Gazebo → Real UAV

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure1.png" title="Wind disturbance effect" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1: Motivating example — wind gusts degrading UAV flight stability
</div>

---

## Motivation

Vision-based UAV navigation systems typically assume benign dynamics. However, wind introduces persistent, temporally correlated disturbances that:

- Are **not directly observable** from images
- **Accumulate over time**
- **Break the Markov assumption** used by feed-forward policies

This makes UAV navigation under wind a **partially observable control problem**, where single-frame policies are insufficient.

WindyRL addresses this by combining:
1. **Disturbance-aware training** (wind randomization)
2. **Temporal policy architectures** that reason over short histories

---

## Key Contributions

- Developed a temporal policy architecture that infers wind effects from state history using a Transformer encoder
- Introduced an acceleration-based wind model (F = m · a_wind) for mass-consistent training across platforms
- Achieved 78% navigation success under 3.0 m/s² wind disturbance in sim-to-sim transfer (vs. 62% baseline)
- Demonstrated zero-shot sim-to-real transfer on a real quadrotor under induced wind conditions
- Released open-source codebase for reproducible research

---

## System Architecture

WindyRL follows the NavRL design philosophy, retaining its perception and control abstractions while introducing a temporal modeling branch.

**Pipeline overview:**
- **Inputs:** RGB image, depth image, and quadrotor internal states
- **Perception:** Construct a local static occupancy representation
- **Static Feature Encoder:** CNN encodes obstacle geometry
- **Temporal Encoder:** Aggregates recent state history (LSTM or Transformer)
- **Policy:** PPO-based actor–critic outputs high-level velocity commands

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure2.png" title="WindyRL framework" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Overall WindyRL framework
</div>

---

## Wind Disturbance Modeling

To expose the policy to realistic aerodynamic effects, WindyRL introduces randomized wind disturbances during training.

### Acceleration-Based Wind Model

Instead of applying wind as velocity-dependent drag, WindyRL directly samples wind acceleration, ensuring mass-invariant motion-level perturbations:

$$\mathbf{F}_{\text{wind}} = m \cdot \mathbf{a}_{\text{wind}}$$

This improves robustness and generalization across platforms.

Wind is applied as **piecewise-constant segments**, independently sampled for each parallel simulation environment.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure3.png" title="Wind direction distributions" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure4.png" title="Wind magnitude sequences" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3 & 4: (Left) Sampled wind direction distributions. (Right) Example wind magnitude sequences over time.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_algorithm1.png" title="Wind generation algorithm" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Algorithm 1: Acceleration-based random wind generation
</div>

---

## Temporal Policy Learning

Wind effects are latent and must be inferred from motion over time. WindyRL explores two temporal architectures:

### LSTM-Based Recurrent Policy

- Maintains an internal memory state
- Integrates motion cues such as drift and acceleration
- Effective but prone to training instability

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure5.png" title="LSTM temporal encoder" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 5: LSTM-based temporal feature extractor
</div>

### Transformer-Based Policy (Final Model)

- Uses a **sliding window (ring buffer)** of recent states
- Applies **causal self-attention** to reason over temporal dependencies
- More stable training and stronger robustness

The Transformer encodes the last **N = 20 states (3.2 s history)** and outputs a compact temporal embedding for the policy.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure6.png" title="Transformer temporal encoder" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 6: Transformer-based temporal feature extractor
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_algorithm2.png" title="Ring buffer algorithm" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Algorithm 2: Ring buffer for temporal state history
</div>

---

## Training Setup

| Parameter | Value |
|-----------|-------|
| Simulator | NVIDIA Isaac Sim |
| RL Algorithm | Proximal Policy Optimization (PPO) |
| Parallelism | 1024 UAVs |
| Environment | ~350 static obstacles |
| Curriculum | Wind acceleration: 0.5 → 3.5 m/s² |
| Hardware | NVIDIA A100 GPU |

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdnavrl.gif" title="Isaac Sim parallel training" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 7: Large-scale parallel RL training in Isaac Sim (1024 UAVs)
</div>

### Curriculum Training Performance

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_table1.png" title="Curriculum training performance" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 1: Curriculum training performance under increasing wind
</div>

---

## Experiments and Results

### Sim-to-Sim Evaluation (Isaac Sim → Gazebo)

The trained policy is transferred **without retraining** to a Gazebo simulator with:
- Different physics engine
- Different quadrotor model
- Injected wind disturbances

A **paired evaluation protocol** compares zero-wind vs windy trajectories.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure8.png" title="Gazebo environment" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure9.png" title="Trajectory comparison" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 8 & 9: (Left) Gazebo evaluation environment. (Right) Trajectory comparison under wind disturbance.
</div>

### Navigation Success Rate Comparison

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_table2.png" title="Navigation success rate" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2: Navigation success rate comparison
</div>

| Condition | WindyRL | NavRL (Baseline) |
|-----------|---------|------------------|
| No wind | 82% | 80% |
| 3.0 m/s² wind | **78%** | 62% |

Evaluation conducted on **50 navigation tasks** in a 40 m × 40 m arena with fixed obstacle layout.

**Results:**
- Comparable performance to NavRL in calm conditions
- Significantly higher success rates under strong wind

---

### Sim-to-Real Flight Tests

WindyRL is deployed **zero-shot** on a real quadrotor without fine-tuning.

**Test Setup:**
- Indoor cluttered environment
- Lateral wind generated by external fans
- Wind magnitude verified with an anemometer

**Compared to a non-temporal baseline, WindyRL shows:**
- Smoother recovery
- Reduced drift
- More stable forward progress

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/wdrl_figure10.png" title="Real-world flight tests" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 10: Real-world flight tests under induced wind
</div>

*Note: Real-world results are preliminary but representative of policy behavior under physical wind disturbances.*

---

## Key Results Summary

| Metric | Result |
|--------|--------|
| Robust navigation under strong wind | ✓ |
| Zero-shot sim-to-sim transfer | ✓ |
| Zero-shot sim-to-real transfer | ✓ |
| No explicit wind estimation | ✓ |
| No additional sensors required | ✓ |
| Transformer outperforms LSTM | ✓ |

---

## Conclusion

WindyRL demonstrates that combining **disturbance-aware training** with **temporal representation learning** is a practical and effective strategy for robust vision-based UAV navigation.

By preserving NavRL's simplicity while significantly improving wind tolerance, WindyRL provides a strong foundation for reliable autonomous flight in real-world, windy environments.

---

## Reproducibility

1. **Training:** Configure wind curriculum parameters and launch parallel PPO training in Isaac Sim
2. **Evaluation:** Export policy weights and run paired navigation tasks in Gazebo (with/without wind)
3. **Deployment:** Deploy velocity-command policy on ROS-compatible quadrotor platform

