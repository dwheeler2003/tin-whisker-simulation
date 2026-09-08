# Heatmap Visualization Contribution

## Overview

My primary contribution to the Tin Whisker Simulation project was working on
a redesign of the simulation's heatmap visualization system.

The simulation tracks instances where detached metallic whiskers bridge
conductive regions on a printed circuit board (PCB). The goal of the heatmap
was to make those events easier to interpret visually.

## The Problem

The existing heatmap changed the color of individual conductor GameObjects
when bridging activity occurred.

While this could identify specific conductors, it produced a fragmented
visualization and made it difficult to understand the spatial distribution
of bridge activity across the PCB as a whole.

The goal was to move toward a continuous visualization across the PCB surface.

## Proposed Architecture

The redesigned approach followed this data flow:

Simulation bridge events

→ 3D Unity world coordinates

→ 2D PCB texture coordinates

→ Texture2D pixel data

→ PCB material

→ Heatmap visualization

The intent was to represent low bridge activity with green regions and higher
bridge activity with increasingly intense colors.

## Texture-Based Approach

Rather than changing the material of individual conductor objects, I worked on
generating a Unity `Texture2D` dynamically.

A simplified version of the prototype logic was:

```csharp
heatmapTexture = new Texture2D(
    textureResolution,
    textureResolution,
    TextureFormat.RGBA32,
    false
);

colors = new Color32[
    textureResolution * textureResolution
];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = Color.green;
}
int x = Mathf.FloorToInt(
    (pos.x + 0.5f) * textureResolution
);

int y = Mathf.FloorToInt(
    (pos.z + 0.5f) * textureResolution
);

int index = y * textureResolution + x;

if (index >= 0 && index < colors.Length)
{
    colors[index] = Color.red;
}
heatmapTexture.SetPixels32(colors);
heatmapTexture.Apply();

pcbMaterial.mainTexture = heatmapTexture;
```
## UI Integration

I also worked on integrating heatmap generation with the simulation's existing
UI toggle.

The intended workflow was:

1. User runs the simulation
2. Bridge positions are recorded
3. User activates the Heatmap control
4. The heatmap texture is generated from bridge-event positions
5. The texture is displayed over the PCB

This required coordinating simulation data, UI state, GameObject references,
Renderer components, and Material objects.

## Main Technical Challenge

The largest challenge was coordinate conversion.

Simulation events existed in Unity's 3D world-space coordinate system, while
the generated heatmap existed in a normalized 2D texture-space coordinate
system.

Correctly translating between these systems required accounting for:

- PCB dimensions
- PCB origin
- Position and orientation
- Texture resolution
- Coordinate normalization

The initial prototype assumed a simplified coordinate relationship that did not
generalize correctly to the actual PCB geometry.

## Debugging Challenges

Several issues made integration difficult:

- Missing or disconnected Unity Inspector references
- Existing GameObject dependencies from earlier development
- Material and Renderer assignment issues
- UI state synchronization
- Interactions with previous heatmap logic
- World-space / texture-space misalignment

Because the project was built on top of software developed by previous student
teams, debugging also required understanding dependencies that were not always
obvious from the individual scripts.

## Result

The texture-based heatmap remained a prototype at the end of the project and
was not fully functional in the final application.

However, the development work demonstrated the feasibility of moving from
object-based highlighting toward a spatial visualization system and exposed
the key architectural problem that would need to be solved: reliable
world-to-texture coordinate mapping.

## What I Would Change

If I continued development today, I would:

1. Establish an explicit PCB-local coordinate system before generating the heatmap.
2. Convert bridge positions from world space into PCB-local space.
3. Normalize local coordinates against the PCB's actual bounds.
4. Convert normalized coordinates into texture pixels.
5. Separate heatmap generation from UI state management.
6. Build and test the visualization first on a minimal PCB scene.
7. Add debug visualization for coordinate transformations before integrating
   the full simulation.

## What I Learned

This feature required working across multiple layers of an existing system:

- Simulation data
- C# application logic
- Coordinate systems
- Real-time visualization
- Unity GameObjects
- Materials and rendering
- UI state

The experience reinforced the importance of understanding an entire system
when troubleshooting a feature rather than treating code components in
isolation.
