# CHORD FINDER

## Objectives

- Extract guitar chords from noisy audio
- Clean the audio by extracting artifacts - such as human voices and background noise
- Visulize the extracted chords


## Steps

### Pre-Processing

#### 1. Trimming and Sampling

- Downsampling to 22Hz to reduce data size and frequency noise without losing chord information

- Trimming - Removing silence using amplitude threshold


#### 2. Bandpass filter

- For a standard guitar, apply a band pass filter that strictly allows 82Hz to 100Hz


#### 3. Framing and Windowing

- Splitting the audio in 20ms to 50ms long chunks

- Research whether 4s chunks is better?


#### 4. Feature Extraction

- Short Time Fourier Transformation (SFFT)
- Chroma featurer extraction: Compress entire spectrogram into chroma features





