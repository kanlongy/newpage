---
layout: page
title: Windy-NavRL - Wind-resilient RL Framework for UAV Navigation
description: LSTM-PPO architecture for robust UAV navigation under wind disturbances
img: assets/img/windy-navrl.jpg
importance: 4
category: graduate
github: https://github.com/kanlongy/Windy-NavRL
---

## Overview

**Period:** Feb. 2025 - Present  
**Institution:** Carnegie Mellon University  
**Location:** Pittsburgh, USA  
**Type:** Research Project  
**Role:** Lead Developer  
**Supervisor:** Prof. Kenji Shimada

---

## Introduction

Autonomous UAV navigation in outdoor environments faces significant challenges from wind disturbances, which can cause trajectory deviations, increased energy consumption, and mission failure. This project develops a reinforcement learning framework that enables UAVs to learn wind-aware navigation policies.

**Project Objective:** Implement LSTM-PPO architecture to improve UAV navigation robustness under challenging wind conditions.

**Impact:** Contributes to safer autonomous drone operations for infrastructure inspection, search and rescue, and package delivery applications.

**Keywords:** Reinforcement Learning, Deep Learning, UAV, Robotics, LSTM-PPO

---

## Methods

### System Architecture

Developed an enhanced LSTM-PPO architecture built on the NavRL framework:

**LSTM-PPO Agent:**

- **Observation Space**: Position, velocity, goal vector, estimated wind velocity
- **LSTM Layer**: Temporal memory to capture wind pattern history
- **Policy Network**: Actor-critic architecture with shared feature extraction
- **Wind Estimator**: Real-time wind velocity estimation from UAV dynamics

**Training Infrastructure:**

- Multi-environment distributed training in parallel simulations
- Curriculum learning with gradually increasing wind intensity
- Domain randomization for varying wind field characteristics

### Simulation Environments

Implemented realistic wind simulation in both Gazebo and Isaac Sim:

**Wind Field Types:**

1. **Constant Wind**: Uniform wind field (baseline testing)
2. **Turbulent Wind**: Spatially-varying wind patterns
3. **Gust Events**: Sudden wind direction and speed changes
4. **Vortex Fields**: Rotational wind patterns near obstacles

---

## Results

The wind-aware RL policy demonstrates improved navigation performance compared to traditional PID control and standard PPO approaches across various wind conditions. The LSTM component enables the policy to learn temporal wind patterns and adapt control strategies accordingly.

**Key Observations:**

- Improved success rates in reaching navigation goals under wind disturbances
- More stable trajectories with reduced deviation from planned paths
- Better energy efficiency through wind-aware trajectory planning

**Policy Behaviors:**

- Proactive compensation for anticipated wind effects
- Efficient path planning that considers wind direction
- Quick stabilization after unexpected wind gusts

---

## My Role & Contributions

As the lead developer on this research project under Prof. Kenji Shimada's supervision, I:

✓ Designed and implemented LSTM-PPO architecture with wind state estimation  
✓ Developed realistic wind field models in Gazebo and Isaac Sim  
✓ Built distributed training infrastructure for parallel policy learning  
✓ Conducted experiments comparing different policy architectures  
✓ Analyzed navigation behaviors and wind-response strategies  
✓ Contributed to CERLAB UAV autonomy stack integration

**Technical Skills Demonstrated:** Reinforcement Learning, ROS, Gazebo, Isaac Sim, PyTorch, UAV Control, Python, C++

---

## Conclusion

This ongoing research project implements a wind-resilient RL framework for UAV navigation using LSTM-PPO architecture. The approach demonstrates improved robustness under wind disturbances through temporal modeling and wind-aware policy learning.

**Key Achievements:**

- LSTM-PPO architecture for wind-resilient navigation
- Comprehensive wind simulation framework in Gazebo and Isaac Sim
- Integration with CERLAB autonomy stack
- Ongoing real-world deployment and validation
