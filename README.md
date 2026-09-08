# Tin Whisker Simulation

A Unity/C# engineering simulation developed as an Auburn University team project
to model the risk of detached metallic whiskers bridging conductors on printed
circuit boards.

The application uses Monte Carlo simulation, Unity physics, and 3D visualization
to model whisker behavior and analyze potential electrical bridging events.

## My Contribution: Heatmap Visualization

My primary contribution was redesigning and debugging the simulation's heatmap
visualization system.

The existing heatmap highlighted individual conductor objects when bridging
events occurred. I worked on a new approach intended to visualize bridge
activity across the entire PCB surface.

My work included:

- Reworking the heatmap logic around Unity `Texture2D`
- Translating 3D bridge-event positions into 2D texture coordinates
- Generating texture data from simulation events
- Applying the generated texture to the PCB material
- Integrating the heatmap with the existing Unity UI toggle
- Debugging GameObject, Renderer, Material, and UI-state references
- Troubleshooting coordinate-system and rendering issues within an inherited codebase

The texture-based heatmap remained a prototype at the end of the project.
One of the primary technical challenges was correctly mapping Unity world-space
coordinates into normalized PCB texture-space coordinates.

## Technologies

- C#
- Unity
- Monte Carlo Simulation
- 3D Visualization
- Git / GitHub
- CSV / Excel Data Analysis

## Project Context

Tin whiskers are microscopic metallic filaments that can form on electronic
components. If a whisker bridges two conductive regions on a printed circuit
board, it can create an unintended electrical connection.

This simulation was designed to help engineers model whisker behavior and
estimate bridging risk under different operating conditions.

## Engineering Challenges

The heatmap work involved debugging interactions across several layers of the
application:

Simulation Data → 3D Coordinates → Texture Coordinates → Texture Generation →
PCB Material → Unity UI

Key challenges included:

- World-space to texture-space coordinate conversion
- Existing GameObject dependencies
- Unity Inspector references
- Material and renderer state
- UI toggle synchronization

## What I Learned

This project gave me hands-on experience working within an existing technical
system rather than building in isolation. It required understanding how
simulation logic, visualization, UI state, and Unity object references interacted
and debugging issues across those layers.

## Project Attribution

This repository is a fork of the shared Auburn University team repository.

The overall simulation was developed collaboratively across multiple student
teams. The sections above describe my primary individual contribution to the
project.
