---
name: procedural-low-poly-world
description: Production standard for building procedural 3D low-poly worlds, village builders, animals, trees, and environmental diorama props in Three.js with flat shading and zero asset dependencies.
version: 1.0.0
author: Hermes & Antigravity
license: MIT
metadata:
  hermes:
    tags: [3d-graphics, threejs, procedural-modeling, low-poly, diorama, webgl]
    related_skills: [procedural-3d-anime-figure, canvas-game-development]
---

# Core Philosophy
This system is dedicated to creating highly stylized, low-polygon dioramas entirely procedurally within a Three.js WebGL environment. The goal is visual fidelity through geometric simplicity and material contrast, achieving a high-quality aesthetic without relying on external high-poly assets.

# Technical Directives

## 1. The Material Isolation Rule (CRITICAL)
**NEVER** mutate shared materials. When multiple meshes share a single `THREE.MeshLambertMaterial` instance, changing one property (like `color`) affects all meshes using that instance.
*   **Rule:** Always instantiate distinct materials for every unique component part (e.g., `deerBodyMat`, `deerBellyMat`, `deerAntlerMat`).

## 2. Ground-Up Stacking Rule
All characters and creatures must be constructed and positioned relative to a definitive vertical baseline ($y=0$). This ensures correct scaling and grounding.
*   **Sequence:** Soles/Hooves $\to$ Legs $\to$ Torso $\to$ Neck $\to$ Head $\to$ Accessories.

## 3. Lighting & Camera Standard
*   **Sky:** Soft atmospheric gradient background (`#dbeafe`).
*   **Key Light:** Warm Directional Light (`0xfffaed`, Intensity 1.3) positioned high and to the side.
*   **Fill Light:** Cool Ambient Light (`0xd9eeff`, Intensity 0.8).
*   **Shadows:** Enabled and using `THREE.PCFSoftShadowMap` for realistic grounding.
*   **Base:** Circular island geometry with a bright, low-poly grass top.

# Four Master Low-Poly Asset Blueprints

## 1. Village Builder Male
*   **Base:** Boots ($\#1e293b$) $\to$ Legs ($\#334155$) $\to$ Torso ($\#455a64$ Linen) $\to$ Leather Apron ($\#935116$).
*   **Upper Body:** Neck ($\#f5c6a5$ Skin) $\to$ Head ($\#f5c6a5$ Skin) $\to$ Mustache/Beard ($\#3e2723$).
*   **Headwear:** Brim (Cylinder, $\#935116$) + Crown (Cone, $\#935116$).
*   **Limbs:** Segmented arms; Right arm holds the hammer.
*   **Tools:** Hammer with wooden handle ($\#795548$) and slate/iron head ($\#475569$).

## 2. Low-Poly Pine Tree
*   **Trunk:** Tapered Cylinder ($\#4e342e$).
*   **Foliage:** Three stacked, progressively smaller cones using distinct, deep forest green hues ($\#2e7d32$, $\#388e3c$, $\#43a047$).

## 3. Low-Poly Deer / Animal
*   **Body:** Torso ($\#934b1f$) + Cream Belly Inset ($\#f6ede0$).
*   **Head/Neck:** Tapered forms, highly articulated structure.
*   **Antlers:** Procedural branching using bone-white material ($\#ede4d3$), with main beams and side tines.
*   **Limbs:** Four slender legs ($\#b45309$) with dark hooves ($\#262626$).
*   **Tail:** Small, perky tail.

## 4. Faceted Granite Rocks / Boulders
*   **Geometry:** Icosahedron or highly faceted shapes.
*   **Material:** Multi-tone slate grays ($\#64748b$, $\#475569$) to simulate natural variation.
*   **Arrangement:** Clustered composition rather than isolated objects.

