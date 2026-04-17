# High_Prep_PS3
# GNSS Spoof Detection using Hybrid Anomaly Detection

## Problem Understanding

Global Navigation Satellite Systems (GNSS) are critical for navigation, timing, and synchronization across multiple domains such as aviation, maritime systems, and telecommunications. However, GNSS signals are inherently weak and susceptible to **spoofing attacks**, where malicious signals manipulate position, velocity, or timing information.

The objective of this project is to **detect spoofed GNSS signals** by analyzing inconsistencies in:

* Signal behavior (Doppler, phase, power)
* Temporal dynamics (time-based variations)
* Satellite-wise patterns (PRN-based analysis)

Due to the absence of labeled training data, the problem is approached as an **unsupervised anomaly detection task**, where spoofed signals are treated as deviations from normal GNSS behavior.

---

## Feature Engineering

Feature engineering is driven by **GNSS signal physics and domain knowledge**.

### Signal-Based Features

* **EC/LC Ratio** → Detects distortion in correlation peaks
* **PIP/PQP Ratio** → Captures abnormal signal power distribution
* **Signal Power** → Combined strength of signal components

### Time-Based Features

* **Time Difference (RX_time - TOW)** → Detects timing inconsistencies and replay attacks

### Temporal Features (Per Satellite - PRN)

* **Doppler Difference** → Sudden changes indicate spoofing
* **Phase Difference** → Detects discontinuities in signal phase
* **Pseudorange Difference** → Captures geometric inconsistencies

### Rolling Statistics

* **Doppler Rolling Std** → Measures signal stability over time
* **CN0 Rolling Mean** → Identifies abnormal signal strength patterns

These features collectively capture **multi-dimensional anomalies**, which are essential since spoofing affects multiple parameters simultaneously.

---

## Model Architecture

A **hybrid anomaly detection framework** is used:

### 1. Rule-Based Detection

Domain-driven thresholds are applied to identify:

* Extreme Doppler shifts
* Sudden phase changes
* Abnormally high signal strength

### 2. Isolation Forest (Unsupervised ML)

* Detects anomalies in high-dimensional feature space
* Works well with unlabeled data
* Captures complex, non-linear patterns

### 3. Hybrid Decision

Final prediction is obtained by combining:

* Rule-based flags
* Isolation Forest anomaly outputs

This ensures both **interpretability** and **robustness**.

---

## Training Methodology

Since labeled data is not available, a **fully unsupervised pipeline** is adopted:

1. Data is sorted by **PRN and time** to preserve temporal relationships
2. Feature engineering is applied using domain-specific transformations
3. Missing and extreme values are handled using median imputation
4. Features are standardized for model stability
5. Isolation Forest is trained on the dataset to learn normal patterns
6. Anomaly scores are converted into:

   * Binary predictions (`spoofed`)
   * Confidence scores (normalized anomaly score)

---

## Justification of Design Decisions

### Why Unsupervised Learning?

* No labeled training data was provided
* Spoofing is inherently an anomaly detection problem

### Why Isolation Forest?

* Efficient for large datasets
* Handles high-dimensional data well
* Does not assume data distribution

### Why Rule-Based System?

* Incorporates **first-principle GNSS knowledge**
* Improves interpretability
* Acts as a strong baseline

### Why Hybrid Approach?

* ML captures complex patterns
* Rules capture domain-specific anomalies
* Combination improves overall robustness

### Why Temporal + PRN-Based Features?

* GNSS signals evolve over time
* Each satellite (PRN) behaves independently
* Spoofing introduces inconsistencies in these patterns

---

## Output Format

The final submission contains:

* `time` → Timestamp from dataset
* `spoofed` → Binary prediction (0 = normal, 1 = spoofed)
* `confidence` → Likelihood score (0 to 1)

---

## Conclusion

This solution leverages a **combination of domain knowledge and machine learning** to detect GNSS spoofing in the absence of labeled data. By modeling signal behavior, temporal dynamics, and satellite-wise inconsistencies, the system achieves a **robust and scalable spoof detection framework**.

---
