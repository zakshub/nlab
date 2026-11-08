# NeuroLab — Project Status & Roadmap Tracker

**Repository:** [https://github.com/zakshub/nlab](https://github.com/zakshub/nlab)  
**Local Workspace:** `/root/2026-portfolio/neurolab`  
**Current Milestone:** Milestone 0 — Technical Feasibility, Scientific Validation & First Publication  
**Status:** **Scientific Validation Complete — Published to GitHub Baseline**  
**Last Updated:** November 2026  

---

## 1. Project Overview

NeuroLab is an interactive, browser-based computational neuroscience laboratory based on the publicly available adult male fruit fly (*Drosophila melanogaster*) connectome (**MaleCNS v1.0**), released collaboratively by HHMI Janelia Research Campus (FlyEM Project), Google Research, the University of Cambridge, and the MRC Laboratory of Molecular Biology.

The platform enables users to:
1. Explore nanoscale 3D neuronal morphologies and synaptic connectivity from the whole-CNS connectome.
2. Simulate realistic neural dynamics (spiking, synaptic inhibition/excitation, and network oscillations) using curated canonical subcircuits.
3. Conduct virtual electrophysiology experiments (optogenetic/current stimulation, synaptic lesions, and pharmacological receptor modulation).

---

## 2. Milestone Progress

| Milestone | Phase / Objective | Status | Target Deliverables |
| :--- | :--- | :--- | :--- |
| **Milestone 0** | **Technical Feasibility, Scientific Validation & Publication** | **COMPLETED** | System audit, dataset licensing verification, ultrastructural metrics disambiguation, [docs/TECHNICAL_FEASIBILITY.md](file:///root/2026-portfolio/neurolab/docs/TECHNICAL_FEASIBILITY.md), baseline [PROJECT_STATUS.md](file:///root/2026-portfolio/neurolab/PROJECT_STATUS.md), Git commit & push. |
| **Milestone 1** | **Connectome Ingestion & Subcircuit Fixtures** | *Pending Approval* | Lightweight neuPrint API client, 4 canonical subcircuit datasets (Ring Attractor, Mushroom Body, Giant Fiber, Motion Detection), SWC parser & level-of-detail downsampler. |
| **Milestone 2** | **Simulation Core & Electrophysiology Engine** | *Queued* | Vectorized Leaky Integrate-and-Fire (LIF) and Izhikevich simulation engine (NumPy), neurotransmitter kinetics (ACh, GABA, Glu), WebSocket real-time state streamer. |
| **Milestone 3** | **Interactive 3D Visualizer & Laboratory UI** | *Queued* | Three.js WebGL 3D neuron canvas, membrane potential action potential glow shaders, live spike raster plots, oscilloscope traces, stimulus controls. |
| **Milestone 4** | **Virtual Experiments, Verification & Documentation** | *Queued* | Pre-built lab modules (Compass ring attractor drift, associative olfactory memory, escape reflex arc), benchmarking, and documentation. |

---

## 3. Environment & Hardware Constraints Baseline

An empirical audit of the development environment on `2026-11-08` established the following operational boundaries:

| Resource | Value / Limit | Operational Constraint |
| :--- | :--- | :--- |
| **CPU Architecture** | `aarch64` (ARM Cortex-A55/A78/X1, 8 cores) | Avoid heavy C++ compiled simulation builds; use native Python vectorized engines. |
| **RAM** | 11 GiB Total / **~816 MiB Available** | Memory quota $\le 200\text{ MB}$ for server runtime; stream data instead of accumulating history. |
| **Storage** | 110 GiB Total / **1.6 GiB Available (99% full)** | **Zero full dataset downloads.** Store only compact subcircuit fixtures ($\le 15\text{ MB}$ total). |
| **GPU** | No dedicated NVIDIA GPU / No CUDA | Offload 3D rendering to client browser WebGL/WebGPU. |
| **Runtimes** | Python 3.13.13, Node.js v26.3.1, npm 11.17.0 | Verified available for backend service and frontend build pipelines. |

---

## 4. Key Architectural Decisions (ADRs)

1. **ADR-001: Subcircuit-Centric Modeling over Whole-CNS Ingestion**
   * *Decision:* The full 166,000-neuron / 310-million synapse dataset will not be ingested locally. NeuroLab will operate on **curated functional subcircuits** (Central Complex Ring Attractor, Mushroom Body Olfaction, Giant Fiber Escape, Motion Vision).
   * *Rationale:* Whole-brain biophysical simulation is computationally infeasible on local hardware and functionally uncalibrated in current neuroscience literature. Subcircuits provide direct educational and research value within memory limits.
2. **ADR-002: Vectorized Python Core + Browser WebGL Visualization**
   * *Decision:* Implement a lightweight Vectorized LIF engine in pure Python (NumPy) with WebSocket streaming, paired with a Three.js / WebGL client frontend.
   * *Rationale:* Minimizes server footprint (<150 MB RAM, <20 MB disk), eliminates C++ build fragility on PRoot `aarch64`, and utilizes client GPU hardware for 60fps 3D rendering.
3. **ADR-003: Dual-Mode Connectome Access (Offline-First Fixtures + Live API)**
   * *Decision:* Bundle pre-extracted, downsampled JSON/Parquet fixtures for the 4 canonical circuits while offering an optional live `neuPrint` API bridge for custom user queries.
   * *Rationale:* Guarantees the application works 100% offline without requiring external API tokens or internet connectivity, while retaining open-ended extensibility.
4. **ADR-004: Clear Separation of Structural Count from Synaptic Conductance**
   * *Decision:* Document that integer synapse counts ($N_{ij}$) do not equal physiological conductances ($g_{ij}$), and provide user-adjustable scaling parameters for quantal conductance and release probability.
   * *Rationale:* Maintains scientific integrity and prevents users from mistaking anatomical approximations for experimental electrophysiology.

---

## 5. Next Actions (Pending Human Approval)

1. Review published Milestone 0 documentation on GitHub.
2. Grant explicit authorization to proceed to **Phase 1: Connectome Subcircuit Fixtures & Ingestion Pipeline**.
