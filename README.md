# RG-Seg

Official implementation and evaluation resources for **RG-Seg: A 3D LiDAR-Based Method for Heterogeneous Ramp-Aware Ground Segmentation**.

<p align="center">
  <img src="assets/ramp_observations.png" alt="Forward-facing and semi-observed ramp scenes" width="92%">
</p>

<p align="center"><em>Heterogeneous ramp observations considered by RG-Seg: a forward-facing ramp (left) and a semi-observed ramp (right).</em></p>

## Overview

RG-Seg is a deterministic, non-learning method for ramp-aware ground segmentation from 3D LiDAR point clouds. It combines an existing ground-segmentation backbone with a plug-and-play Heterogeneous Ramp Detection (HRD) module designed to handle both forward-facing and semi-observed ramps.

The HRD module uses polar-grid partitioning, grid-level geometric model discrimination, and eight-neighborhood region growing to identify connected ramp structures. Non-planar residuals are preserved as potential obstacle candidates rather than being merged into the ramp surface.

<p align="center">
  <img src="assets/method_overview.png" alt="RG-Seg processing framework" width="100%">
</p>

<p align="center"><em>RG-Seg pipeline: ground-surface estimation followed by polar-grid segmentation, grid-model calculation, and region-growing ramp detection.</em></p>

## Repository Status

The source code and reproducibility materials are being organized for public release. The revision release will include:

- the C++ implementation of RG-Seg and the HRD module;
- ROS Melodic and PCL build instructions;
- fixed configuration files used for the reported experiments;
- scripts for the real-world, synthetic, and SemanticKITTI evaluations;
- the common point-level Precision, Recall, and F1 evaluation code;
- provenance records for all compared baselines; and
- patches or reimplementation notes needed to reproduce the baseline results.

## Qualitative Results

The following examples illustrate the method on forward-facing and semi-observed ramps. In the point-cloud visualizations, the colors follow the manuscript convention for segmented ground, ramp, and residual structures.

<p align="center">
  <img src="assets/qualitative_results.png" alt="Qualitative comparison on forward-facing and semi-observed ramps" width="100%">
</p>

<p align="center"><em>Real-world comparison. From left to right within each row: environment, RG-Seg, R-GPF, and LR-Seg.</em></p>

<p align="center">
  <img src="assets/obstacle_residual_example.png" alt="Qualitative obstacle residual preservation example" width="88%">
</p>

<p align="center"><em>Qualitative example of non-planar residual preservation: RG-Seg separates the ramp surface from potential obstacle-candidate points.</em></p>

## Evaluation Sources

The paper evaluates one fixed RG-Seg parameter configuration on three data sources:

- 600 annotated scans sampled from 30 real-world ramp scenes;
- 3,000 synthetic scans generated with HDL-16, HDL-32, and HDL-64 sensor models; and
- 200 ramp-containing scans selected from SemanticKITTI.

The reported parameter configuration is:

| Parameter | Value |
|---|---:|
| Polar sector angle, `Delta alpha` | 30 deg |
| Laser beams per grid, `eta` | 3 |
| Region-growing threshold, `epsilon_th` | 15 deg |
| Ramp cutoff angle, `theta_max` | 45 deg |

The same resulting configuration is used across the three data sources; it is not retuned separately for an individual dataset. As stated in the paper, the evaluation data contributed to parameter selection, so the results are not presented as an independent cross-dataset generalization test.

<p align="center">
  <img src="assets/parameter_sensitivity.png" alt="Parameter sensitivity analysis on three data sources" width="92%">
</p>

<p align="center"><em>Ramp-segmentation F1 sensitivity to the four principal parameters. Dashed lines mark the selected tested values.</em></p>

## Baseline Reproducibility

The comparison includes R-GPF, CascadedSeg, PatchWork, PatchWork++, and LR-Seg. A dedicated baseline record will identify, for every method:

- the implementation actually used in the experiments;
- the upstream repository and exact version, when public code was used;
- whether the configuration came from the original paper, an official default, or an author-selected setting;
- all material modifications made for the comparison; and
- confirmation that one fixed configuration was used across the three data sources.

Methods without a suitable public implementation will be accompanied by our in-house reimplementation and a mapping from the original paper to the implemented parameters.

## Software Environment

- Ubuntu 18.04 LTS
- ROS Melodic
- Point Cloud Library (PCL)
- C++

Exact dependency versions and build commands will be included with the source release.

## Citation

The final bibliographic entry will be added after publication.

## License

A source-code license will be added with the public code release after the third-party baseline licenses have been checked.
