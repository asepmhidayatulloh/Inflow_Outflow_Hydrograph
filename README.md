# Inflow and Outflow Hydrograph Simulation

A Python simulation of an inflow–outflow hydrograph, converted from an Excel-based hydrological model into an interactive Jupyter/Google Colab notebook.

The model demonstrates how incoming discharge (**inflow**) is routed through a storage system to produce a delayed, attenuated release (**outflow**), and visualizes both as hydrographs over time.

![Inflow and Outflow Hydrograph](hydrograph_plot.jpg)

## Overview

This project reproduces a reservoir/channel routing hydrograph model originally built in Excel, using Python for computation and Matplotlib for visualization. Given a peak discharge and a set of shape/routing parameters, the model computes:

- **I(t)** — the inflow hydrograph, following a `[1 - cos(αt)]·e^(kt)` shape, normalized so its peak exactly equals the specified peak discharge.
- **O(t)** — the outflow hydrograph, obtained by routing the inflow through a linear reservoir/channel storage model (numerical integration).

An interactive widget lets you adjust the input parameters and see the hydrographs update in real time — similar to pressing **F9** to recalculate in the original Excel sheet.

## Formulas

**Inflow hydrograph:**

```
I(t) = (Ip / 2) · [1 − cos(αt)] · e^(kt)      for 0 ≤ t ≤ τ
I(t) = 0                                       for t > τ
```

**Outflow hydrograph (routed via linear storage):**

```
O(t) = (1 / e^(kt)) · ∫₀ᵗ k · I(t) · e^(kt) dt
```

**Angular frequency:**

```
α = 2π / τ
```

> **Note:** the raw shape function is normalized so that `max(I(t)) = Ip` exactly, and forced to zero beyond the time base `τ` (since `1 − cos(αt)` is periodic and would otherwise rise again past one full cycle).

### Input variables

| Symbol | Description | Unit |
|---|---|---|
| `Ip` | Peak discharge | m³/s |
| `thau (τ)` | Time base | hours |
| `k` | Storage/routing coefficient | 1/hour |
| `dt` | Simulation time step | hours |

### Output variables

| Symbol | Description | Unit |
|---|---|---|
| `I(t)` | Inflow discharge | m³/s |
| `O(t)` | Outflow discharge | m³/s |
| `Time` | Simulation duration | hours |
ct is open-source and available for educational and research use. Feel free to fork, modify, and adapt it for your own hydrological analysis.
