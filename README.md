# The Rule-Based Method: Chord Template Matching

In music theory, every standard chord is defined by a strict formula of notes. For example, a **C Major** chord must contain the notes **C, E, and G**.

Instead of training a machine learning model to "learn" what a C Major chord sounds like, you can hardcode a dictionary of ideal musical templates and compare your audio data directly to them.

---

## 1. Define the Binary Templates (The Matrix)

Create a fixed mathematical profile for every chord you want to identify.

In a binary template:

- **1** means the note should be present.
- **0** means the note should be absent.

| Chord | C | C# | D | D# | E | F | F# | G | G# | A | A# | B |
|---------|---|----|---|----|---|---|----|---|----|---|----|---|
| C Major | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
| A Minor | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| G Major | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 |

These templates represent the ideal pitch-class distribution for each chord.

---

## 2. Calculate Cosine Similarity (The Scoring Step)

For every time frame in the audio file, the feature extraction pipeline produces a real-world **Chroma vector**.

A typical example might look like:

```text
[0.85, 0.05, 0.10, 0.00, 0.72, 0.12, 0.00, 0.91, 0.02, 0.10, 0.05, 0.00]
```

Notice that the values for **C**, **E**, and **G** are significantly larger than the others.

The next step is to compare this observed chroma vector against every chord template in the database using **Cosine Similarity** (or alternatively Euclidean Distance).

Cosine Similarity measures how closely two vectors point in the same direction:

\[
\text{Similarity} = \frac{A \cdot B}{\|A\|\|B\|}
\]

Where:

- \(A\) = observed chroma vector
- \(B\) = chord template vector

A similarity score close to **1.0** indicates a strong match.

---

## 3. Maximum Likelihood Selection

After computing similarity scores for all chord templates, select the chord with the highest score.

Example:

| Chord Template | Similarity Score |
|---------------|------------------|
| C Major | 0.92 |
| G Major | 0.15 |
| A Minor | 0.41 |

Since **C Major** has the highest score, the algorithm outputs:

```text
C Major
```

This is effectively a maximum-likelihood decision rule:

> The chord template that best explains the observed chroma vector is selected as the predicted chord.

---

# Why This Works So Well for Guitar

This rule-based approach has several advantages:

- No training data required
- Extremely fast inference
- Easy to understand and debug
- Runs efficiently on low-power hardware
- Produces surprisingly strong results for many chord-recognition tasks

However, there is one major challenge:

## Harmonics (Overtones)

When a guitar string vibrates, it does not produce only a single frequency.

For example, an open low **E** string naturally generates:

- The fundamental note (**E**)
- Higher octaves of **E**
- The perfect fifth (**B**)
- Other harmonic components such as **G#**

As a result, a chromagram may contain energy in several pitch classes even when only a single note is played.

A simple binary-template matcher may therefore incorrectly classify a single note as a full chord.

---

# The Non-ML Fix: Harmonic Pitch Class Profiles (HPCP)

To improve accuracy without using machine learning, many audio-analysis systems use **Harmonic Pitch Class Profiles (HPCP)** instead of a standard chromagram.

HPCP algorithms explicitly model the expected harmonic structure of musical notes.

Instead of treating every detected frequency equally, they apply weighted contributions based on known harmonic relationships, such as:

- Octaves
- Perfect fifths
- Other harmonic intervals

This process reduces the influence of predictable overtones and emphasizes the true fundamental pitches.

As a result, the resulting pitch-class representation more accurately reflects the notes that are actually being played.

### Benefits of HPCP

- More robust chord detection
- Better handling of guitar overtones
- Reduced false chord activations
- Improved performance without requiring machine learning

In practice, HPCP often provides a much cleaner input representation for rule-based chord recognition systems than a standard chromagram.

