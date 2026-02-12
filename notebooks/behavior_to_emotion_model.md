# Chirp Behavior-to-Emotion Mapping Model (Behavior → VAS)

## 1. Project Goal
Chirp aims to analyze the vocalizations and behavior of pet Peach-faced Lovebirds to infer their emotional state along three dimensions: **Valence**, **Arousal**, and **Social Engagement**. The goal is to help owners better understand and care for their birds.


## 2. Behavioral Measures (Inputs / Features)

| Feature | Range | Positive / Negative Meaning | Description |
|---------|-------|----------------------------|------------|
| **M = Movement Intensity** | -5 → 5 | |M| large → high activity / arousal | Measures how active the bird is. High activity indicates excitement but does not directly determine emotional valence. |
| **B = Body Posture** | -5 → 5 | B > 0 → confident, display behavior, proactive<br>B < 0 → aggressive, defensive, “fluffed up” | Captures posture polarity and intensity. Influences both emotion direction and activation level. |
| **D = Directionality** | -5 → 5 | D > 0 → oriented toward other birds<br>D < 0 → oriented toward humans | Captures social orientation. Influences social engagement and partially affects valence direction. |

> **Note:** All behavioral measures are continuous, ranging from -5 to 5 for flexible modeling of bird emotion.


## 3. Target Measures (Outputs / VAS Dimensions)

| Dimension | Range | Meaning | Computation |
|-----------|-------|---------|------------|
| **Arousal (A)** | 0 → 5 | Activation level / intensity | A = 0.5 × |M| + 0.5 × |B| |
| **Valence (V)** | -5 → 5 | Positive/negative mood × intensity | First compute directional tendency T, then combine with activation A:<br>T = w1 × B + w2 × D + w3 × M<br>V = (T / 5) × A |
| **Social Engagement (S)** | -5 → 5 | Social orientation | S = 0.5 × B + 0.5 × D |

> Example weights: w1 = 0.6 (posture), w2 = 0.25 (directionality), w3 = 0.15 (movement intensity).  
> These weights can be adjusted based on empirical data or expert calibration.


## 4. Model Relationships (Behavior → VAS)

- **A (Activation):** Reflects intensity of bird activity, independent of emotional sign.  
- **T (Directional Factor):** Combines posture, directionality, and movement to indicate mood direction.  
- **V (Valence):** Combines intensity (A) with direction (T) to produce a continuous score from -5 to 5.  
- **S (Social Engagement):** Combines posture and directionality to measure social intent, independent of Valence.


## 5. Key Design Principles

1. **Body Posture (B)** is the main factor influencing valence direction but does not solely determine it. It also influences arousal.
2. **Movement Intensity (M)** primarily affects activation level (A) but contributes slightly to valence direction.  
3. **Directionality (D)** affects both valence direction and social engagement, but not activation.  
4. **Activation (A)** is magnitude-only; it reflects “how strongly” the bird is expressing behavior.  
5. **Continuous Scoring:** All inputs and outputs are continuous (-5 to 5 for V and S, 0-5 for A) to allow fine-grained modeling.  
6. **Scalability:** The model structure allows easy adjustment of weights or inclusion of new behavioral measures as more data becomes available.


## 6. Notes for Future Improvement
- Weights (w1, w2, w3) can be calibrated using larger datasets or regression against human-labeled VAS scores.  
- Additional behavioral measures can be incorporated to better capture subtle social or emotional signals.  
- Noise reduction or audio pre-processing may improve feature extraction for automated emotion prediction.
