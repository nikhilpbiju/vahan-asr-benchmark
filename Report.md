# ASR Benchmarking Under Noisy Indian Telephony Conditions

## Objective

The goal of this benchmark was to evaluate how multiple ASR systems perform under realistic Indian telephony-style conditions rather than clean studio-quality speech.

Deepgram was used as the baseline system and compared against multiple open-source alternatives across noisy, multilingual, and entity-heavy conversational audio.

The benchmark specifically focused on:
- Bangalore locality recognition
- noisy and low-quality recordings
- multilingual/code-switched speech
- robustness under telephony-like conditions
- operational usability beyond raw WER

---

# Models Evaluated

| Model | Type |
|---|---|
| Deepgram Nova | Commercial production ASR |
| Whisper Base | Lightweight multilingual transformer |
| Whisper Small | Higher-capacity multilingual transformer |
| Vakyansh | Indic-specialized ASR |
| XLSR Wav2Vec2 | Open-source multilingual ASR |

The systems were intentionally chosen to compare:
- commercial vs open-source approaches
- multilingual vs Indic-specialized models
- lightweight vs larger transformer architectures

---

# Dataset Creation

A custom dataset of 30 recordings was created containing Bangalore locality names embedded within conversational Hinglish and multilingual speech.

Examples:
- “Main Koramangala side rehta hoon.”
- “Bellandur side network issue aa raha hai.”
- “Yeshwanthpur se bus mil jayegi kya?”

The recordings intentionally varied across multiple acoustic conditions:

| Condition |
|---|
| Quiet indoor speech |
| Traffic noise |
| Far-mic recordings |
| Whispered speech |
| Phone-call-like audio |
| Rushed speech |
| Low-volume speech |
| Slightly disfluent speech |

The dataset was intentionally designed to simulate imperfect real-world Indian telephony environments rather than ideal clean-speech benchmarks.

---

# Evaluation Metrics

The following metrics were implemented:

| Metric | Purpose |
|---|---|
| WER | Word-level transcription accuracy |
| CER | Character-level similarity |
| Latency | Inference response time |
| RTF | Real-Time Factor |
| Entity Accuracy | Locality name preservation |
| Hallucination Rate | Incorrect/generated words |

The benchmark intentionally went beyond WER because operational ASR systems must preserve entities reliably under noisy conditions.

---

# Benchmark Pipeline

Each recording was processed sequentially through all ASR systems.

The pipeline:
1. Loaded audio recordings
2. Ran inference across all models
3. Normalized transcripts
4. Computed evaluation metrics
5. Aggregated benchmark results into a unified dataframe

The implementation was written entirely in Python using:
- Deepgram SDK
- HuggingFace Transformers
- Whisper
- Librosa
- JiWER
- Pandas

The pipeline was designed to be fully reproducible.

---

# Key Findings

## 1. Deepgram Was The Most Stable Overall

Deepgram consistently demonstrated the strongest robustness under noisy and telephony-style conditions.

Compared to open-source systems:
- hallucinations were lower
- multilingual handling was more stable
- entity preservation was more reliable
- transcription collapse under noise was less severe

This was especially visible under:
- traffic noise
- far-mic speech
- low-volume recordings

---

## 2. Whisper Small Performed Surprisingly Well

Whisper Small produced the strongest open-source results overall.

Strengths:
- good multilingual robustness
- relatively stable under code-switching
- lower WER than Whisper Base
- reasonable entity preservation

However:
- hallucinations occasionally appeared
- noisy speech still caused semantic substitutions

Example:
- “Deepgram” became unrelated words/entities in certain recordings.

Despite this, Whisper Small was the strongest open-source competitor overall.

---

## 3. Whisper Base Hallucinated Aggressively

Whisper Base frequently produced semantic substitutions and hallucinated unrelated words under noisy conditions.

Observed behaviors included:
- unrelated entity generation
- incorrect English substitutions
- phonetic drift under rushed speech

While latency was relatively reasonable, reliability degraded significantly under difficult acoustic conditions.

---

## 4. Vakyansh Preserved Hindi Phonetics But Struggled Operationally

Vakyansh showed strong Indic phonetic behavior but struggled with multilingual conversational speech.

Observed issues:
- aggressive transliteration
- script switching
- inability to preserve English locality names
- unstable handling of code-switched speech

In several cases:
- English locality names were converted into Hindi phonetic approximations
- outputs became difficult to use operationally downstream

However, Hindi phonetic preservation was noticeably stronger than general multilingual systems.

---

## 5. XLSR Failed Under Realistic Noise Conditions

XLSR performed poorly under noisy and far-mic recordings.

Observed behaviors included:
- multilingual script corruption
- random foreign-language outputs
- severe entity corruption
- unstable transcription under low-quality audio

Under realistic telephony conditions, XLSR frequently became operationally unusable despite being functional under cleaner inputs.

---

# Most Important Insight

The most important finding from this benchmark was:

> Low WER alone was insufficient for evaluating practical ASR usability in Indian telephony scenarios.

Entity preservation, hallucination behavior, multilingual robustness, and stability under noisy conditions proved equally important.

In several cases:
- transcripts with acceptable WER still failed operationally because locality names were corrupted.
- some models produced readable transcripts but failed entity extraction entirely.
- multilingual robustness mattered more than clean benchmark performance.

This became especially important for Bangalore locality names where slight phonetic corruption could break downstream systems.

---

# Failure Analysis

Several failure patterns appeared repeatedly across models:

| Failure Type | Example Behavior |
|---|---|
| Entity corruption | “Koramangala” partially destroyed |
| Hallucinations | unrelated words/entities generated |
| Script switching | outputs unexpectedly switching scripts |
| Noise collapse | traffic audio severely degrading outputs |
| Phonetic drift | locality names becoming distorted |
| Far-mic degradation | incomplete or unstable transcription |

The benchmark intentionally exposed these weaknesses by using difficult recording conditions rather than curated clean speech.

---

# Limitations

This benchmark has several limitations:

- The dataset size was relatively small (30 recordings).
- Recordings were manually collected rather than crowd-sourced.
- Some recordings contained highly difficult acoustic conditions that may exaggerate degradation.
- Open-source models were evaluated using default inference configurations rather than extensive hyperparameter optimization.

However, the objective of the benchmark was robustness evaluation under realistic telephony-style conditions rather than leaderboard-style optimization.

---

# Recommendation

Based on the benchmark results:

## Recommended Production Baseline
### Deepgram Nova

Deepgram provided the best balance of:
- robustness
- multilingual handling
- entity preservation
- operational stability
- low hallucination behavior

under noisy Indian telephony conditions.

---

## Recommended Open-Source Alternative
### Whisper Small

Whisper Small was the strongest open-source model overall and provided:
- competitive multilingual performance
- relatively stable entity preservation
- acceptable latency
- better robustness than Whisper Base, XLSR, and Vakyansh

---

# Final Conclusion

This benchmark demonstrated that evaluating ASR systems purely through WER is insufficient for production conversational systems.

Under realistic Indian telephony conditions:
- noise robustness
- entity preservation
- multilingual stability
- hallucination resistance

became equally important operational metrics.

The strongest systems were not necessarily the ones with the lowest theoretical transcription error, but the ones that remained stable and operationally usable under difficult real-world conditions.
