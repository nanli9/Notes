---
aliases: [blend shapes, blendshapes, facial animation, facial rig, morph targets]
tags: [topic, Animation]
---

# Blend Shapes

Linear combination of pre-sculpted target shapes to drive facial animation and other detailed deformations. The standard for facial rigs in film and games.

## Course Notes

### Linear Blend Shape Facial Rig
- **Neutral face:** $n \in \mathbb{R}^{3 \cdot \text{numVertices}}$
- **Blend shapes:** $b_i \in \mathbb{R}^{3 \cdot \text{numVertices}}$ (each is a full mesh)
- **Blended result:** $x = n + \sum_{i=1}^{N} (b_i - n) \cdot \alpha_i(t)$
- Weights: $1 \geq \alpha_i \geq 0$ (typically, though can exceed for exaggeration)

### Corrective Blend Shapes
- Pairwise combination artifacts → add correction terms
- $x = n + \sum_{i=1}^{N} \alpha_i(t)(b_i - n) + \sum_{i < j} \beta_{ij}(t)(C_{ij} - n)$
- $C_{ij}$ are corrective shapes that fix artifacts when shapes $i$ and $j$ are both active

### Uncanny Valley
- As model complexity increases: perceived quality rises, then dips sharply near photorealism (uncanny valley), then rises again
- Foundress → increasing realism → uncanny dip → full realism
