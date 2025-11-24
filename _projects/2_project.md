---
layout: page
title: Si Piezosensor for MEMS Micromirror Angle Control
description: MEMS sensor design and fabrication at Tohoku University
img: assets/img/research2.png
importance: 2
category: undergraduate
related_publications: false
---

## Overview

**Period:** Apr. 2023 - Aug. 2023  
**Institution:** S. Tanaka Laboratory, Tohoku University  
**Location:** Sendai, Japan  
**Type:** Undergraduate Research  
**Supervisors:** Prof. Shuji Tanaka & Assist. Prof. Andrea Vergara

---

## Introduction

MEMS (Micro-Electro-Mechanical Systems) micromirrors are critical components in optical applications such as laser scanning displays, LiDAR systems, and biomedical imaging. Two-dimensional (2D) piezoelectric micromirrors require precise angle control on both axes, but the slow axis often lacks effective feedback sensing due to mechanical design constraints.

**Project Objective:** Design and fabricate an integrated Si piezoresistive angle sensor for the slow axis of a 2D piezoelectric micromirror to enable closed-loop feedback control and improve positioning accuracy.

**Impact:** This research contributes to the development of high-precision optical MEMS devices with applications in autonomous vehicles (LiDAR), medical imaging (endoscopy), and augmented reality displays.

**Keywords:** MEMS, Piezoresistors, Micromirror, Sensor Design, Finite Element Analysis, Fabrication

---

## Methods

### Sensor Design

I designed a piezoresistive angle sensor structure specifically for the slow axis of the 2D piezoelectric micromirror:

**Design Approach:**

- Integrated Si piezoresistors at the hinge locations where mechanical stress is maximized during mirror rotation
- Optimized sensor geometry to balance sensitivity and mechanical robustness
- Designed two prototype structures: cantilever and meandering configurations for comparative analysis

**Finite Element Analysis (FEA):**

- Performed stress distribution analysis using COMSOL Multiphysics
- Simulated piezoresistive output under various deflection angles
- Validated sensor placement and geometry optimization

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/research2-dt.gif" title="FEA simulation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Finite element analysis simulation showing stress distribution during mirror deflection
</div>

### Fabrication Process

I gained hands-on experience with the complete MEMS fabrication workflow at Tohoku University's cleanroom facility:

**Silicon-on-Insulator (SOI) Wafer Processing:**

1. **Deposition**: Thin-film metal deposition for electrical contacts
2. **Photolithography**: UV exposure and pattern transfer using photoresist
3. **Doping**: Boron diffusion to create piezoresistive regions
4. **Etching**: Deep reactive ion etching (DRIE) to define device structure
5. **Dicing**: Wafer separation into individual device chips
6. **Wire Bonding**: Electrical connection to package pins
7. **Packaging**: Device encapsulation and protection

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/research2.png" title="Fabricated MEMS devices" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fabricated MEMS devices showing cantilever and meandering piezoresistor structures
</div>

---

## Results

### Device Characterization

Successfully fabricated and characterized multiple prototype devices:

**Key Findings:**

- Piezoresistive sensors successfully detected angular deflection of the slow axis
- Output signal showed good correlation with simulation predictions
- Meandering structure demonstrated enhanced sensitivity compared to simple cantilever design
- Sensor integration did not significantly impact mirror mechanical performance

---

## Key Achievements

- Novel piezoresistive sensor design for slow-axis angle detection
- Complete hands-on MEMS fabrication experience from design to testing
- Validation of sensor performance through experimental characterization
- Enhanced understanding of the relationship between mechanical design and sensor sensitivity

**Technical Skills Gained:** MEMS Design, Finite Element Analysis (COMSOL), Cleanroom Fabrication, Photolithography, Thin-Film Deposition, Wire Bonding, Device Characterization
