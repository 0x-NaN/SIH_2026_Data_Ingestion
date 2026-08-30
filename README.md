# Cyber Threat Forecasting: Multimodal LSTM Pipeline

This repository contains the data ingestion, preprocessing, and modeling pipeline for a multimodal LSTM designed to forecast cyber attacks (infiltration probability and MITRE stage) before compromise completes. 

The architecture utilizes two distinct branches:
1. **Flow Branch:** Ingests network traffic (CTU-13, DAPT2020, LANL flows).
2. **Auth Branch:** Ingests aggregated authentication events (LANL auth).

---

## Phase 1: Data Acquisition

### 1. LANL Comprehensive Multi-Source Cyber-Security Events
*Status: `flows.txt.gz` and `redteam.txt.gz` downloaded. `auth.txt.gz` pending.*
- **Action Required:** Your download token expired. Go to [csr.lanl.gov/data/cyber1/](https://csr.lanl.gov/data/cyber1/), submit the form again to generate a **new token/link**, and download `auth.txt.gz` (7.2 GB).
- **Files needed:** 
  - `auth.txt.gz` (Authentication events - *Critical for Auth Branch*)
  - `flows.txt.gz` (Network flows - *For Flow Branch*)
  - `redteam.txt.gz` (Ground truth red-team timeline - *For Label Mapping*)

### 2. CTU-13 (Botnet C2 Traffic)
- **Source:** [imfaisalmalik/CTU13-CSV-Dataset](https://github.com/imfaisalmalik/CTU13-CSV-Dataset)
- **Action:** Download the repository as a ZIP and extract. We are using the pre-converted CICFlowMeter CSVs to avoid raw PCAP parsing.
- **Files needed:** `CTU13_Attack_Traffic.csv`, `CTU13_Normal_Traffic.csv`.

### 3. DAPT2020 (Full APT Lifecycle)
- **Source:** [Kaggle: DAPT2020](https://www.kaggle.com/datasets/sowmyamyneni/dapt2020)
- **Action:** Download via Kaggle UI or CLI (`kaggle datasets download -d sowmyamyneni/dapt2020`).
- **Files needed:** The extracted CSV files containing stage-labeled APT traffic.

---

## Phase 2: Directory Structure

Once downloaded, organize your data into the following structure to ensure the ingestion scripts work seamlessly:

```text
data/
├── lanl/
│   ├── auth.txt.gz
│   ├── flows.txt.gz
│   └── redteam.txt.gz
├── ctu13/
│   ├── CTU13_Attack_Traffic.csv
│   └── CTU13_Normal_Traffic.csv
└── dapt2020/
    └── [extracted dapt2020 csv files]
