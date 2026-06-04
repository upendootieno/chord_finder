The Rule-Based Method: Chord Template Matching
In music theory, every standard chord is defined by a strict formula of notes. For example, a C Major chord must contain the notes C, E, and G.

Instead of training a model to "learn" what C Major sounds like, you hardcode a dictionary of ideal musical templates and compare your audio data directly to them.

1. Define the Binary Templates (The Matrix)
You create a fixed mathematical profile for every chord you want to find. In a binary template:

1 means the note should be present.

0 means the note should be silent.

Chord	C	C#	D	D#	E	F	F#	G	G#	A	A#	B
C Major Template	1	0	0	0	1	0	0	1	0	0	0	0
A Minor Template	1	0	0	0	1	0	0	0	0	1	0	0
G Major Template	0	0	1	0	0	0	0	1	0	0	0	1
2. Calculate Cosine Similarity (The Scoring Step)
For every time frame in your audio file, your extraction pipeline gives you a real-world Chroma vector. It might look messy due to overtones, like this:
[0.85, 0.05, 0.1, 0.0, 0.72, 0.12, 0.0, 0.91, 0.02, 0.1, 0.05, 0.0] (Notice how C, E, and G are highly active).

You then calculate the Cosine Similarity (or Euclidean distance) between this real-world vector and every single chord template in your database.

The math checks the angle between the two vectors:

Similarity= 
∥A∥∥B∥
A⋅B
​
 
3. Maximum Likelihood Selection
The chord template that returns the highest similarity score (closest to 1.0) wins! If the C Major template scores 0.92 and G Major scores 0.15, your algorithm outputs "C Major".

Why this works incredibly well for Guitar
This non-ML approach is incredibly fast and lightweight. It requires zero training data and can run instantly on low-power hardware.

However, it does have one massive hurdle: Harmonics (Overtones).
When a guitar plays an open low E string, the physics of the string also naturally ring out a higher B and a higher G#. This can trick your basic binary template into thinking you are playing an E Major chord when you only plucked a single note.

The Non-ML Fix: Harmonic Pitch Class Profiles (HPCP)
To make this method accurate without ML, data scientists use HPCP instead of a standard chromagram.

HPCP algorithms apply a mathematical weight that expects overtones at specific mathematical intervals (like the perfect fifth and octave).

It mathematically "subtracts" the predictable overtones, leaving you with a highly accurate representation of just the fundamental notes actually being fretted.

