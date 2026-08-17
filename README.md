<h1 align="center">Asmita Chakraborty</h1>

<p align="center">
  <b>Real-Time Audio DSP · Acoustics · Embedded Systems/Firmware</b><br>
  <sub>Seven years shipping audio firmware on constrained hardware — now working on the research questions underneath it.</sub>
</p>

<p align="center">
  <a href="https://orcid.org/0009-0001-5506-9588"><img src="https://img.shields.io/badge/ORCID-0009--0001--5506--9588-A6CE39?style=flat-square&logo=orcid&logoColor=white" alt="ORCID"></a>
  <img src="https://img.shields.io/badge/Bangalore-India-555?style=flat-square&logo=googlemaps&logoColor=white" alt="Location">
</p>

---
<!--
> **Applying to graduate programs in music technology, acoustics and audio signal processing for Fall 2027.**
> If you're faculty or a student in one of these labs — the projects below are the work I'd want to build on, and I'd genuinely welcome the conversation.

---
-->
## 🔬 Research Interests

**Adaptive feedback and howling suppression** in closed acoustic loops — detection, notch allocation, frequency shifting, and how much stable gain you can actually buy.
**Sound-field reconstruction with calibrated uncertainty** — when a spatial audio system says "the field here is *X*", what guarantee comes with that, and when should it refuse to answer?
**Real-time DSP on constrained hardware** — what survives the trip from a MATLAB prototype to a DSP core with a fixed cycle budget, and what quietly doesn't.
**Physical and perceptual modelling** for spatial and immersive audio.

The through-line: I've spent my career on the side of audio where the algorithm has to run — in a car cabin, a hospital corridor, a smart ring. I'm now interested in the part where it also has to be *provably* right.

---

## 📌 Research Artifacts

Both are self-contained, documented, and reproducible from a clean checkout.

### 🔊 [In-CarCommunication](https://github.com/Dhwaani/In-CarCommunication) — adaptive howling suppression in FAUST

An open-source [FAUST](https://faust.grame.fr) implementation of in-car communication (ICC): cabin speech picked up, processed, and replayed through the same cabin's loudspeakers — a closed electro-acoustic loop that wants to howl.

- **Filterbank howling detector** rather than FFT — FAUST expands FFT into scalar butterflies, so the filterbank is the structure that actually stays real-time
- **Self-allocating notch cascade** — slots claimed and released as howling frequencies come and go
- **Frequency shifting** via `fi.pospass` for additional loop-gain margin
- **Measured, not just compiled:** +4 dB MSG bypassed vs **+16 dB suppressed — 12 dB of added stable gain** on the reference synthetic cabin at 16 kHz, recorded in `docs/tuning.md` as a regression check
- Two bugs surfaced only by *running* it: an unnormalised cabin path with +9 dB peak gain (loop-gain readings were physically meaningless until fixed) and unstable loops reaching NaN, now soft-saturated

`lib/icc.lib` + three designs · ~700 lines of FAUST · verified against FAUST 2.70.3 · C/C++/Rust backends confirmed · CI · `CITATION.cff`

### 📐 [SoundFieldUQ](https://github.com/Dhwaani/SoundFieldUQ) — certified sound-field reconstruction

Conformal prediction gives distribution-free coverage guarantees, but breaks under covariate shift unless you know the likelihood ratio `dQ/dP` — which in general you don't.

**The observation:** in sound-field reconstruction you *do*. The covariate is spatial position, and both the calibration-microphone density and the query/listener density are designed by the experimenter. So `dQ/dP` is exactly computable from geometry — the one thing weighted conformal prediction (Tibshirani et al., 2019) normally cannot have.

| Method | Coverage (nominal 0.900) | Beyond calibration support |
|---|---|---|
| Split conformal | 0.765 | **0.283 — fails silently** |
| Exact-ratio weighted conformal | **0.901** | Abstains (infinite intervals, ~87%) |

A **placement corollary** falls out: effective sample size is maximised when *p = q*, so the optimal calibration-microphone layout is a sample from the query density.

Pure MATLAB, base install only — no toolboxes, fully synthetic image-source data, zero hardware. 60 `.m` files · ~3400 lines · 8 experiments · 10 figures · 6 test classes. Every algorithm cross-verified in Python before porting; expected values in `docs/expected_results.md`. *Paper in preparation.*

### 🎚️ [AudioDSPDesign](https://github.com/Dhwaani/AudioDSPDesign)
Full-duplex dsp chain design for 1mic voice communication studies and implementation on AudioWeaver

---

## 🏭 Where This Comes From

Seven years of production audio and systems firmware — the reason I care about cycle budgets and failure modes rather than just algorithms.

| | |
|---|---|
| **AINA Computer** | Smart-ring voice UI on QCC5181 — wideband 2-mic cVc end-fire tuning, AVC, echo cancellation and noise suppression in the Kalimba DSP |
| **HemodynamiQ** | Zephyr RTOS BLE telemetry and DFT-based bioimpedance measurement — 8192-point DFT with Hann window, per-frequency calibration |
| **Harman India** | Audio HAL/DSP lead — designed full- and half-duplex communication systems from scratch for hospital nurse-call products (Systevo), collaborating with the German team on DSP architecture across ARM Cortex-M7 and A53 |
| **Qualcomm** | AudioReach framework on an RTOS smartwatch — HFP call support, audio use-case graphs, stream Rx/Tx design with IIR/FIR filter chains |
| **AMD · L&T (Intel)** | Server platform security and Android automotive BSP — Secure Debug Unlock, microcode patching, coreboot/UEFI bring-up with sub-2s boot |

**Also:** Visiting Researcher at IISc Bangalore (CSA) — sliding-mode control with control barrier functions for multi-agent robots, and over-synchronization in CUDA programs. Full-time open-source contributor to [coreboot](https://coreboot.org) and the [OpenID Foundation](https://openid.net).

---

## 📚 Publications & Patents

- **IEEE 2026** — Author-oriented semantic plagiarism detection using transformer architectures
- **IEEE 2026** — Transformer-based authorship attribution: fine-tuning BERT and S-BERT for stylometric analysis
- **arXiv 2025** — Conversational AI dialog for medicare, powered by fine-tuning and retrieval-augmented generation
- **2024** — AI-engine-based acceleration for high-performance programmable SoC designs
- **3 published Indian patents** (Dec 2024) — semantic role labelling for sentiment analysis · RAG-based conversational AI · fine-tuned BERT for relation extraction and NER
- *In preparation* — SoundFieldUQ; neuromorphic drowsiness prediction with multi-sensor fusion

🔗 [ORCID 0009-0001-5506-9588](https://orcid.org/0009-0001-5506-9588)

---

## 🛠️ Tools

**Audio &amp; DSP** — FAUST · MATLAB · Audio Weaver · Qualcomm AudioReach · Android Audio Framework · ALSA · Kalimba DSP · IIR/FIR design, AEC, AGC, noise suppression

**Systems** — C · C++ · Python · Zephyr · FreeRTOS · ThreadX · Linux kernel · ARM Cortex-M3/M7/A53 · x86 · BLE, I2S, I2C, UART · JTAG/J-Link/Trace32 · Git, Gerrit, CI

---

## 🎹 Beyond the Code

Graduate in keyboard from Bangiya Sangeet Parishad, and in painting from Tripura Fine Arts Academy with distinction. *Dhwaani* — my handle — means resonance.

---

## 📫 Contact

Happy to talk about adaptive feedback control, spatial audio, uncertainty quantification, or getting DSP to survive on a small core.

<p align="left">
  <a href="aschakra_ashim@outlook.com"><img src="https://img.shields.io/badge/Email-EA4335?style=flat-square&logo=gmail&logoColor=white"></a>
  <a href="https://www.linkedin.com/in/YOUR-LINKEDIN"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white"></a>
  <a href="https://orcid.org/0009-0001-5506-9588"><img src="https://img.shields.io/badge/ORCID-A6CE39?style=flat-square&logo=orcid&logoColor=white"></a>
  <a href="https://github.com/Dhwaani/Portfolio"><img src="https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=astro&logoColor=white"></a>
</p>
