# Network Intrusion Detection System (NIDS) with UNSW-NB15

## 📌 Project Overview
This project implements a robust **Machine Learning-based Network Intrusion Detection System (NIDS)** capable of identifying and categorising cyber threats in real-time network traffic. 

Using the **UNSW-NB15 dataset**, the system employs a **Two-Stage Classification Pipeline**:
1.  **Track A (The Gatekeeper):** A Binary Random Forest model designed to distinguish between **Normal** and **Malicious** traffic with zero-tolerance for missed attacks.
2.  **Track B (The Specialist):** A Multi-Class Random Forest model designed to categorise specific attack types (e.g., DoS, Exploits, Fuzzers) to aid in forensic analysis.

## 🛠️ Tech Stack & Key Techniques
* **Language:** Python (Pandas, NumPy, Scikit-Learn, Matplotlib, Seaborn)
* **Data Preprocessing:** * **Log Transformation** (to handle skewed high-variance features like `sbytes`).
    * **Robust Scaling** (to mitigate the impact of extreme outliers).
    * **Label Encoding** (for categorical targets).
* **Imbalance Handling:** * **Undersampling** (Binary Track) to prevent majority class bias.
    * **SMOTE** (Multi-Class Track) to generate synthetic samples for rare attacks like 'Worms' and 'Backdoors'.
* **Feature Engineering:** * **Recursive Feature Selection** using Random Forest Importance.
    * **Dimensionality Reduction** (Reduced 40+ features to the Top 20 most discriminative signals).

## 📊 Methodology

### 1. Data Cleaning & Engineering
Raw network data was cleaned and transformed. Special attention was paid to the "power law" distribution of network flow data (handled via Log Transform) and the varying scales of duration vs. packet size (handled via Robust Scaler).

### 2. Track A: Binary Classification (Safety First)
* **Goal:** Maximise Recall to ensure no attacks slip through.
* **Result:** Achieved **100% Recall** for the Attack class. 
* **Top Features:** Protocol integrity metrics like `sttl` (Time to Live) and `synack` (TCP Handshake).

### 3. Track B: Multi-Class Classification (Diagnosis)
* **Goal:** Accurately classify the type of attack.
* **Challenge:** Severe class imbalance (Generic attacks = 40k rows; Worms = 100 rows).
* **Solution:** Applied SMOTE with an 'auto' strategy to balance the training set.
* **Top Features:** Volumetric metrics like `smeansz` (Mean Packet Size) and `Sload` (Source Bits/Sec).

## 📈 Results Summary

| Model | Accuracy | Key Metric | Insight |
| :--- | :--- | :--- | :--- |
| **Binary (Track A)** | 99% | **Recall: 1.00** | Successfully detected all malicious instances (Zero False Negatives). |
| **Multi-Class (Track B)** | 77% | **F1 (Generic): 0.90** | High accuracy on volumetric attacks (DoS, Fuzzers); struggle with stealthy attacks (Backdoors). |

## 📂 File Structure
* `NIDS_Analysis.ipynb`: The main Jupyter Notebook containing the full end-to-end pipeline (EDA, Preprocessing, Modelling, Evaluation).
* `data/`: Folder containing the UNSW-NB15 training and testing CSVs.

## 🚀 Future Improvements
* Experimenting with Deep Learning (LSTM/CNN) to better capture sequential patterns in "stealthy" attacks.
* Implementing Hyperparameter Tuning (GridSearch) to optimise the Random Forest depth.
