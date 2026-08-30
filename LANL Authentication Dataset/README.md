# LANL Comprehensive Multi-Source Cyber-Security Events

## 🎯 Purpose
The LANL dataset was created to provide researchers with **realistic, de-identified enterprise network telemetry** that includes **actual red-team campaign ground truth**. Unlike synthetic or purely simulated datasets, LANL captures genuine attacker progression (lateral movement, privilege escalation, and persistence) within a real corporate environment over an extended period. 

In our multimodal LSTM pipeline, this dataset serves as the primary source for the **Auth Branch**, providing the deepest, most realistic signal for detecting lateral movement and initial access.

---

## 📊 Key Characteristics
- **Duration**: 58 consecutive days of continuous network activity.
- **Environment**: Real corporate network (Los Alamos National Laboratory), not a controlled lab simulation.
- **Scale**: Massive volume (tens of millions of events). The `auth.txt` file alone is ~7.2 GB compressed.
- **Anonymization**: Usernames, computer names, and IP addresses are hashed/anonymized to protect privacy, but relational mappings (e.g., User A logging into Computer B) remain intact and highly valuable for graph/sequence modeling.

---

## 📁 Core Files & Features

### 1. `auth.txt.gz` (~7.2 GB) – *The Primary Auth Signal* ❌ Pending
Contains Windows authentication events. Key features include:
- `date` & `time`: Exact timestamp of the event.
- `user`: The anonymized user account initiating the action.
- `pc`: The source computer.
- `dcom`: The destination computer.
- `auth_type`: The authentication package used (e.g., Kerberos, NTLM).
- `logon_type`: How the logon occurred (e.g., Interactive, Network, Service).
- `outcome`: **Success** or **Fail** (critical for calculating fail/success ratios).

> **Status:** Download token expired. Re-submit the form at [csr.lanl.gov/data/cyber1/](https://csr.lanl.gov/data/cyber1/) to get a fresh link.

### 2. `redteam.txt.gz` (~4.8 KB) – *The Ground Truth* ✅ Downloaded | Check GitHub Repo Releases
The most valuable file in the dataset. It contains the exact timestamps and details of the actual red-team operations. 
- **Use Case**: This is used to map "Normal" authentication sequences to specific MITRE tactics (e.g., flagging the first anomalous `user@host` pair as *Initial Access*, and subsequent hops as *Lateral Movement*).
- **Location:** Stored locally in `data/lanl/redteam/`.

### 3. `flows.txt.gz` (~1.1 GB) – *Network Context* ✅ Downloaded
NetFlow data providing network-level context to complement the auth events. Key features include:
- Source/Destination IPs and Ports.
- Protocol (TCP/UDP).
- Bytes and Packets transferred.
- Duration of the flow.
- **Location:** Stored in GitHub Releases (too large for standard repo storage).

---

## ⚠️ Important Handling Notes for Our Pipeline
1. **Do Not Load Raw**: The 7.2 GB `auth.txt.gz` will crash standard Pandas. It must be read in `chunks` or processed with memory-efficient tools (e.g., Polars, Dask, or chunked Pandas).
2. **Aggregation Required**: The LSTM cannot ingest raw, individual auth events. We must aggregate this data into fixed time-buckets (e.g., 1-hour windows) per `user@host` entity to create features like `Total Events`, `Failed Login Ratio`, and `Distinct Destinations`.
3. **Chronological Integrity**: The 58-day timeline must be split strictly by day (e.g., Days 1–40 Train, 41–50 Val, 51–58 Test) to prevent future-state data leakage.

---

## 📋 Download Checklist
- [x] `redteam.txt.gz` — stored in `data/lanl/redteam/`
- [x] `flows.txt.gz` — stored in GitHub Releases
- [ ] `auth.txt.gz` — awaiting new download token from LANL
