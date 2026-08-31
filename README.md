<h1 align="center">Ashmita Chakraborty</h1>

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
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![MATLAB](https://img.shields.io/badge/MATLAB-e16737?style=for-the-badge&logo=mathworks&logoColor=white)
![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)

An open-source [FAUST](https://faust.grame.fr) implementation of in-car communication (ICC): cabin speech picked up, processed, and replayed through the same cabin's loudspeakers — a closed electro-acoustic loop that wants to howl.

- **Filterbank howling detector** rather than FFT — FAUST expands FFT into scalar butterflies, so the filterbank is the structure that actually stays real-time
- **Self-allocating notch cascade** — slots claimed and released as howling frequencies come and go
- **Frequency shifting** via `fi.pospass` for additional loop-gain margin
- **Measured, not just compiled:** +4 dB MSG bypassed vs **+16 dB suppressed — 12 dB of added stable gain** on the reference synthetic cabin at 16 kHz, recorded in `docs/tuning.md` as a regression check
- Two bugs surfaced only by *running* it: an unnormalised cabin path with +9 dB peak gain (loop-gain readings were physically meaningless until fixed) and unstable loops reaching NaN, now soft-saturated

`lib/icc.lib` + three designs · ~700 lines of FAUST · verified against FAUST 2.70.3 · C/C++/Rust backends confirmed · CI · `CITATION.cff`

### 📐 [SoundFieldUQ](https://github.com/Dhwaani/SoundFieldUQ) — certified sound-field reconstruction
![MATLAB](https://img.shields.io/badge/MATLAB-e16737?style=for-the-badge&logo=mathworks&logoColor=white)
![LaTeX](https://img.shields.io/badge/LaTeX-008080?style=for-the-badge&logo=latex&logoColor=white)
![Signal Processing](https://img.shields.io/badge/Signal_Processing-00599C?style=for-the-badge)

Conformal prediction gives distribution-free coverage, but under covariate shift the
guarantee needs the likelihood ratio `dQ/dP`. Weighted conformal prediction
(Tibshirani et al., 2019) is exact when that ratio is known — in most applications it
has to be estimated, and estimation error is where coverage quietly goes.

**In sound-field reconstruction it doesn't have to be estimated.** The covariate is
spatial position, and both the calibration-microphone density and the query/listener
density are chosen by the experimenter — so `dQ/dP` is available in closed form from
geometry, with no density-ratio estimation step. Known-ratio settings aren't unique to
acoustics (randomised designs and importance sampling have them too).


| Method | Coverage (nominal 0.900) | Beyond calibration support |
|---|---|---|
| Split conformal | 0.765 | **0.283 — fails silently** |
| Exact-ratio weighted conformal | **0.901** | Abstains (infinite intervals, ~87%) |

Two things follow from having the ratio exactly rather than approximately. Where
`dQ/dP` is genuinely unbounded — query points outside the calibration support — the
method **abstains** with infinite intervals instead of silently under-covering; an
estimated ratio cannot separate a true singularity from estimator blow-up. And because
effective sample size is maximised when *p = q*, the optimal calibration-microphone
layout is a sample from the query density — a **placement** result, not just an
inference one.

Pure MATLAB, base install only — no toolboxes, fully synthetic image-source data, zero hardware.

### 🎚️ [AudioDSPDesign](https://github.com/Dhwaani/AudioDSPDesign)
![Audio Weaver](https://img.shields.io/badge/Audio_Weaver-DSP_Concepts-005A9C?style=for-the-badge)
![AudioDSP](https://img.shields.io/badge/AudioDSP-Signal_Processing-008080?style=for-the-badge)

Full-duplex dsp chain design for 1mic voice communication chain on AudioWeaver
### 🎛️ [StabilityGAN](https://github.com/Dhwaani/StabilityGAN) — Data-Driven Feedback Control via Metric Surrogates

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)

A MetricGAN-inspired framework for acoustic feedback suppression that replaces non-differentiable loop bifurcation points with a learned surrogate predictor (`StabilityNet`) to directly optimize notch filter allocation.

* **Differentiable MSG Estimation:** Trains a neural surrogate to estimate continuous Maximum Stable Gain (MSG) headroom, enabling end-to-end policy optimization where physical loop oscillation prevents direct gradient backpropagation.
* **Group Delay & Phase Shift Dynamics:** Quantifies how notch bank group delay alters loop phase alignment. High-order filters (Length 513) destabilize the loop (**−8.00 dB** stable gain) by shifting howling to adjacent frequencies, whereas lower-order filters (Length 31) achieve **+7.75 dB** of added stable gain.
* **Oracle-Placed Benchmarks:** Isolates physical phase delay from detection errors by testing against ground-truth howling frequencies, proving filter length trade-offs are physical rather than algorithmic.

---

### 🎥 [AV-TalkerRFS](https://github.com/Dhwaani/AV-Talker-RFS) — Audio-Visual Spatial Talker Tracking via Random Finite Sets

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

A multi-modal audio-visual tracking framework leveraging Random Finite Set (RFS) filter dynamics for dynamic spatial speaker localization and state estimation under visibility shifts.

* **Random Finite Set Formulation:** Handles varying talker counts, track creation, and track loss without explicit target-to-observation data association overhead or track-switching failures under multi-speaker overlap.
* **Audio-Visual Sensor Fusion:** Fuses spatial audio Direction-of-Arrival (DoA) vectors with visual detection bounding boxes to maintain continuous trajectory estimation during severe visual occlusions.
* **Validation Scope & Upper Bounds:** Evaluated under measurement-level baseline sweeps to establish theoretical upper bounds across visibility regimes prior to full end-to-end evaluation on real-world datasets like AVA-AVD.
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
  <a href="mailto:aschakra_ashim@outlook.com"><img src="https://img.shields.io/badge/Email-0078D4?style=flat-square&logo=maildotru&logoColor=white"></a>
  <a href="https://www.linkedin.com/in/YOUR-LINKEDIN"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white"></a>
  <a href="https://orcid.org/0009-0001-5506-9588"><img src="https://img.shields.io/badge/ORCID-A6CE39?style=flat-square&logo=orcid&logoColor=white"></a>
  <a href="https://github.com/Dhwaani/Portfolio"><img src="https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=astro&logoColor=white"></a>
</p>
