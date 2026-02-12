# Behavior → VAS Formulas

This file documents the mathematical relationships used to compute Arousal, Valence, and Social Engagement from bird behavioral measures.

Let:

- M = Movement intensity
- B = Body posture
- D = Directionality
- A = Arousal
- V = Valence
- S = Social Engagement
- w_B, w_D, w_M = weights for Valence direction factor


## 1. Arousal

Arousal reflects the activation level (how excited the bird is) and depends on the absolute values of movement and posture:

$$
A = 0.5 \cdot |M| + 0.5 \cdot |B|
$$

Range: 0 → 5


## 2. Valence

Valence combines the directional tendency (T) and activation (A) to produce a continuous score:

1. Compute directional tendency:

$$
T = w_B \cdot B + w_D \cdot D + w_M \cdot M
$$

2. Compute Valence:

$$
V = \frac{T}{5} \cdot A
$$

Range: -5 → 5

> Default weights: w_B = 0.6, w_D = 0.25, w_M = 0.15  
> Clip V to [-5, 5]


## 3. Social Engagement

Social Engagement combines posture polarity and directionality:

$$
S = 0.5 \cdot B + 0.5 \cdot D
$$

Range: -5 → 5
