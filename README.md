# vahan-asr-benchmark
Benchmarking multilingual ASR systems under noisy Indian telephony conditions.

# Vahan ASR Benchmarking Assignment

## Overview

This project benchmarks multiple Automatic Speech Recognition (ASR) systems under noisy and realistic Indian telephony-style conditions.

The objective was to evaluate how different ASR models perform on:
- Bangalore locality names
- noisy environments
- whispered and rushed speech
- far-mic audio
- phone-call-like conditions
- multilingual and code-switched speech

Deepgram was used as the primary baseline system and compared against multiple open-source alternatives.

---

# Models Evaluated

- Deepgram
- Whisper Base
- Whisper Small
- Vakyansh
- XLSR Wav2Vec2

---

# Evaluation Metrics

The following metrics were implemented and compared across all models:

| Metric | Purpose |
|---|---|
| WER | Word Error Rate |
| CER | Character Error Rate |
| Latency | Inference response time |
| RTF | Real-Time Factor |
| Entity Accuracy | Locality name preservation |
| Hallucination Rate | Frequency of incorrect/generated words |

---

# Dataset

A custom dataset of 30 audio recordings was created containing Bangalore locality names under varied recording conditions.

Conditions included:
- quiet indoor speech
- traffic noise
- far-mic recordings
- whispered speech
- low-volume speech
- rushed speech
- phone-call-like audio

The dataset intentionally simulates imperfect real-world Indian telephony conditions rather than clean studio-quality audio.

---

# Repository Structure

```text
.
├── audio/
├── notebook/
├── data/
├── report/
└── README.md
