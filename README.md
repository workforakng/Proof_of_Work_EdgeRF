# EdgeRF: Sub-Megabyte Drone Remote Controller Identification at Low SNR
## Official Experimental Verification & Benchmark Suite (IEEE RadarConf 2027)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/workforakng/c12dd851dde546e38b662cca22217501/Proof_of_Work_EdgeRF.ipynb)
[![Target: IEEE RadarConf 2027](https://img.shields.io/badge/IEEE-RadarConf%202027-red.svg)](https://radarconf2027.org)
[![Dataset: IEEE DataPort](https://img.shields.io/badge/IEEE%20DataPort-10.21227%2Fss99--8d56-blue.svg)](https://dx.doi.org/10.21227/ss99-8d56)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

This standalone repository provides an **interactive, fully reproducible benchmark and verification suite** for the research paper:

> **"EdgeRF: Sub-Megabyte Drone Controller Identification at Low SNR and a Leakage-Aware Re-evaluation of DroneRF"**  
> **Authors:**  
> * **Kalpita Dutta** (`kalpita.d@technoindiaeducation.com`, `kalpita03@ieee.org`) — *ORCID: [0000-0002-1224-215X](https://orcid.org/0000-0002-1224-215X)*  
> * **Akshar Nath Gorain** (`akshar241001001213@technoindiaeducation.com`) — *ORCID: [0009-0002-0083-3654](https://orcid.org/0009-0002-0083-3654)*  
> * **Sourodyuti Biswas Sanyal** (`sourodyuti241001001246@technoindiaeducation.com`) — *ORCID: [0009-0003-3809-9448](https://orcid.org/0009-0003-3809-9448)*  
> *Department of Computer Science and Engineering, Techno India University, West Bengal, India*  
> 
> **Citation:**  
> K. Dutta, A. N. Gorain, and S. B. Sanyal, *"EdgeRF: Sub-Megabyte Drone Controller Identification at Low SNR and a Leakage-Aware Re-evaluation of DroneRF,"* in *Proc. IEEE Radar Conference (RadarConf)*, Bangalore, India, 2027.

---

### 1. Interactive 1-Click Verification

To ensure seamless evaluation without requiring reviewers to download 124+ GB of raw RF captures or configure specialized hardware environments, the accompanying notebook runs end-to-end on both **Google Colab** and **Kaggle** in under 1 minute:

👉 **[Open `Proof_of_Work_EdgeRF.ipynb` in Google Colab](https://colab.research.google.com/gist/workforakng/c12dd851dde546e38b662cca22217501/Proof_of_Work_EdgeRF.ipynb)**

* **Zero Hardcoded Values:** Every metric, table, figure, and significance test is dynamically evaluated live directly from the serialized ONNX deployment engine (`edgerf_rc17_int8.onnx`), test partition manifests, and confusion matrices.
* **Live Model Execution:** Runs forward inference live through ONNX Runtime, measuring real host execution times and testing input burst vectors.

---

### 2. Dataset Authenticity & Hardware Provenance

All experimental results are evaluated against the open-access **IEEE DataPort** benchmark:

* **Dataset Title:** *Drone Remote Controller RF Signal Dataset* (NCSU MPACT Lab)
* **IEEE DataPort DOI:** [`10.21227/ss99-8d56`](https://dx.doi.org/10.21227/ss99-8d56)
* **ResearchGate Reference:** [Publication 349251847](https://www.researchgate.net/publication/349251847_Drone_Remote_Controller_RF_Signal_Dataset) (Ezuma et al., 2020; Ozturk et al., 2021)
* **Acquisition Rig:** Keysight Infiniium DSO-S 404A oscilloscope sampling at **20 GSa/s** (250 $\mu$s burst duration, 5,000,000 samples/burst) across 0–10 GHz.
* **Emitter Coverage:** 17 physical remote controllers from 8 manufacturers (DJI, Spektrum, Futaba, Graupner, HobbyKing, FlySky, Turnigy, Jeti), including unit pairs of identical drone models (Matrice 600 #1 vs #2, Phantom 4 Pro #1 vs #2).
* **Test Partitions:** $N = 2,633$ for the 17-controller task; $N = 2,331$ for the 15-controller protocol.

---

### 3. Experimental Findings & Replicated Paper Artifacts

The notebook dynamically executes the test suite to verify the empirical claims of the paper:

| Paper Section | Evaluated Metric / Target | Empirical Result | Scientific Baseline / Significance |
| :--- | :--- | :---: | :--- |
| **Section 3** | Live Forward Model Execution (INT8 ONNX) | **100% Live** | Evaluates $N=2,633$ test manifest; exactly 3 errors |
| **Table I** | 17-Controller Accuracy (Native SNR $\approx 51\text{ dB}$) | **99.89%** | $+4.29\%$ over Ezuma et al. 2020 ($95.60\%$) |
| **Table I** | 17-Controller Accuracy ($-10\text{ dB}$ In-Band SNR) | **96.04%** | Operates $10\text{ dB}$ below in-band noise floor |
| **Table I** | 15-Controller Accuracy (Native SNR) | **100.00%** | Unified model vs. dedicated CNNs ($99.70\%$) |
| **Table I** | 15-Controller Accuracy ($-10\text{ dB}$ In-Band SNR) | **99.80%** | $+7.80\%$ over Ozturk et al. 2021 at $+10\text{ dB}$ ($92.00\%$) |
| **Figure 4** | Emitter Disambiguation Matrix ($-10\text{ dB}$ In-Band) | **100% Inter-Make** | Errors strictly confined to DJI same-model units |
| **Table II** | Wilson 95% Score Confidence Interval | **$[99.67\%, 99.96\%]$** | $N=2,633$, non-overlapping with prior literature |
| **Table II** | McNemar Paired Test (vs. Ezuma et al. 2020) | **$\chi^2 = 107.21, p = 4.00\times 10^{-25}$** | Discordance: 115 vs 2 ($OR = 57.50$, decisive rejection) |
| **Table II** | Wilcoxon Signed-Rank Test (vs. Ozturk et al. 2021) | **$W = 15.0, p = 0.0312$** | Statistically dominates per-SNR CNNs ($p < 0.05$) |
| **Table II** | Inter-Class Reliability (Cohen's $\kappa$ & MCC) | **$\kappa = 0.9988, \text{MCC} = 0.9988$** | Near-perfect multi-class correlation |
| **Table III** | KAN Explainability (Top-5% Salience Deletion) | **$74.2$ pt drop** (to $12.76\%$) | **$3.82\times$ to $5.21\times$** higher concentration than MLP |
| **Table IV** | Quantized Edge Deployment Footprint | **0.59 MB** | INT8 static QDQ binary on disk |
| **Table IV** | Single-Thread Execution Latency | **1.08 ms** / sample | Measured on CPU (sub-2 ms edge target satisfied) |
| **Section V-B** | DroneRF Leakage Audit (Random vs Temporal) | **$89.23\% \to 46.40\%$** | Paired $t=68.66, p=2.12\times 10^{-4}, d=36.15$ |

---

### 4. Repository Structure

```
├── Proof_of_Work_EdgeRF.ipynb           # Primary verification notebook (Colab & Local)
├── Proof_of_Work_EdgeRF_Kaggle.ipynb    # Kaggle notebook mirror
├── README.md                            # Verification documentation and benchmarks
└── test_artifacts/
    ├── edgerf_rc17_int8.onnx            # Serialized INT8 ONNX deployment engine (0.59 MB)
    ├── test_rc17_eval_manifest.npz      # Full test partition ground truth and predictions (N=2,633)
    ├── provenance_metadata.json         # IEEE DataPort specifications & DroneRF leakage audit
    ├── results_dronerc.json             # Empirical waterfall accuracy dictionaries across SNR sweep
    ├── results_heads_dronerc.json       # KAN vs. MLP token deletion fidelity evaluations
    ├── results_dronerf.json             # DroneRF random vs. temporal multi-seed evaluations
    ├── cm_r17_random_nfft4096_native.npy# Empirical 17-class confusion matrix (Native SNR)
    ├── cm_r17_random_nfft4096_-10dB.npy # Empirical 17-class confusion matrix (-10 dB In-Band)
    ├── cm_r17_random_nfft4096_0dB.npy   # Empirical 17-class confusion matrix (0 dB In-Band)
    ├── cm_r17_blocked_native.npy        # Empirical 17-class confusion matrix (Capture-order split)
    ├── cm_r15_random_native.npy         # Empirical 15-class confusion matrix (Native SNR)
    ├── cm_r15_random_-10dB.npy          # Empirical 15-class confusion matrix (-10 dB In-Band)
    └── cm_r15_random_0dB.npy            # Empirical 15-class confusion matrix (0 dB In-Band)
```

---

### 5. Intellectual Property & Code Release Policy

This review repository is dedicated strictly to **peer-review verification and audit reproducibility**:
* It packages the complete forward-pass inference runtime, serialized ONNX models, evaluation manifests, and hypothesis test scripts.
* Proprietary training procedures and uncompiled raw feature pipelines are retained in our private lab repository during the review process.
* Upon acceptance at IEEE RadarConf 2027, the full end-to-end training codebase, dataset loaders, and quantization recipes will be open-sourced under an MIT license.
