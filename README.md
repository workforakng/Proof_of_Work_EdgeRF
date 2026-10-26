# EdgeRF: Sub-Megabyte Drone Remote Controller Identification & DroneRF Re-Evaluation
## Official Review Artifact & Reproducible Proof of Work (PoW)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/workforakng/c12dd851dde546e38b662cca22217501/Proof_of_Work_EdgeRF.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![IEEE Target](https://img.shields.io/badge/IEEE-RadarConf%202027-red.svg)](https://radarconf2027.org)

This standalone repository serves as an **interactive, independent Proof of Work (PoW)** for peer review of our IEEE submission:
> **"EdgeRF: Sub-Megabyte Drone Controller Identification at Low SNR and a Leakage-Aware Re-evaluation of DroneRF"**  
> *Kalpita Dutta, Akshar Nath Gorain, Sourodyuti Biswas Sanyal*

---

### 1. Zero-Friction 1-Click Verification

To guarantee complete review transparency without requiring reviewers to download 12+ GB of raw radio-frequency captures or install heavy local dependencies, the accompanying notebook runs end-to-end in **Google Colab** in under 2 minutes:

👉 **[Open `Proof_of_Work_EdgeRF.ipynb` in Google Colab](https://colab.research.google.com/gist/workforakng/c12dd851dde546e38b662cca22217501/Proof_of_Work_EdgeRF.ipynb)**

---

### 2. Dataset Authenticity & Provenance Verification

The experimental evaluation is performed on the standard public benchmark datasets:
1. **Drone Remote Controller RF Signal Dataset (IEEE DataPort / ResearchGate):**
   - **ResearchGate Reference:** [Publication 349251847](https://www.researchgate.net/publication/349251847_Drone_Remote_Controller_RF_Signal_Dataset)
   - **IEEE DataPort Identifier:** `doi:10.21227/nm02-c923` (Ezuma et al., 2020; Ozturk et al., 2021)
   - **Hardware Setup:** Keysight Infiniium DSO-S 404A oscilloscope sampling at **20 GSa/s** (250 $\mu$s bursts, 5,000,000 samples per record) over 0–10 GHz.
   - **Class Coverage:** 17 physical remote controllers from 8 manufacturers (DJI, Spektrum, Futaba, Graupner, HobbyKing, FlySky, Turnigy, Jeti), including 2 distinct unit pairs of the same model (Matrice 600 and Phantom 4 Pro).
   - **Volume:** 17,548 total records (2,633 holdout test captures).
2. **DroneRF Dataset (Mendeley Data):**
   - **DOI:** `10.17632/f4c2b4n7zk.1` (Al-Sa'd et al., 2019)
   - **Hardware Setup:** 2x USRP-2943R SDRs monitoring 2.4 GHz ISM band at 40 MS/s.

---

### 3. What This Proof of Work Verifies (Paper Tables & Figures)

The notebook executes forward inference and formal statistical tests, replicating all reported findings:

| Paper Section | Target Metric / Test | Verified Result | SOTA Comparison / Verdict |
| :--- | :--- | :---: | :--- |
| **Table I** | RC-17 Identification (Native SNR $\approx 51\dB$) | **99.89%** | $+4.29\%$ over Ezuma et al. ($95.60\%$) |
| **Table I** | RC-17 Identification ($-10\dB$ In-Band SNR) | **96.02%** | SOTA at extreme noise ($10\dB$ below noise floor) |
| **Table I** | RC-15 Identification (Native SNR) | **100.00%** | $+0.30\%$ over Ozturk et al. ($99.70\%$) |
| **Table I** | RC-15 Identification ($-10\dB$ In-Band SNR) | **99.76%** | $+7.76\%$ over Ozturk et al. ($92.00\%$) |
| **Table II** | Wilson 95% Score Confidence Interval | **$[99.67\%, 99.96\%]$** | $N=2633$, non-overlapping with prior works |
| **Table II** | McNemar's Paired Significance Test | **$\chi^2 = 107.21, p = 3.99\times 10^{-25}$** | Discordance: 115 vs 2 ($OR = 57.50$) |
| **Table II** | Wilcoxon Signed-Rank Test Across SNR Curve | **$W = 15.0, p = 0.0312$** | Statistically dominates per-SNR CNNs ($p < 0.05$) |
| **Table II** | Multi-Class Cohen's Kappa & MCC | **$\kappa = 0.9988, \text{MCC} = 0.9988$** | Near-perfect inter-class reliability |
| **Table III** | KAN Attribution Faithfulness (Token Deletion) | **$74.2$ pt drop** (12.8% acc) | **$3.82\times$ to $5.21\times$** more faithful than MLP ($19.5$ pt drop) |
| **Table IV** | Quantized Edge Deployment (INT8 ONNX) | **0.59 MB footprint** | **1.51 ms** latency on ARM Cortex-X2 / 1.96 ms on Xeon |
| **Section V-B** | DroneRF Data Leakage (Random vs Temporal Holdout) | **$89.23\% \to 46.40\%$** | Paired $t=68.66, p=2.12\times 10^{-4}$ (proves leakage) |

---

### 4. Code & Intellectual Property Policy

This review artifact is designed strictly for **validation and peer evaluation**:
- Contains complete forward-pass model definitions, ONNX quantized runtimes, token deletion engines, and statistical hypothesis tests.
- Does not expose internal training pipelines or optimizer checkpoints to protect intellectual property prior to formal conference publication.
- Upon acceptance at IEEE RadarConf 2027, the complete end-to-end preprocessing, training, and deployment repository will be made publicly available.
