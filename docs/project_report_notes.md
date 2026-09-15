# Project Report Notes — AI-Based Intrusion Detection System

Use this as a skeleton to expand into your full MCA project report / synopsis.

## 1. Title
AI-Based Intrusion Detection System using Machine Learning on the CIC-IDS2017 Dataset

## 2. Abstract (draft)
Network intrusion detection is a critical component of modern cybersecurity
infrastructure. This project presents a machine-learning-based Network Intrusion
Detection System (NIDS) trained on the CIC-IDS2017 dataset, which contains realistic,
labeled network traffic covering both benign activity and multiple categories of
cyberattacks (DoS, DDoS, Port Scanning, Brute Force, Web Attacks, Botnets, and
Infiltration). Three supervised learning models — Random Forest, XGBoost, and a
Multi-Layer Perceptron — are trained and compared on flow-based statistical features.
Data preprocessing includes cleaning of infinite/missing values, feature scaling,
and SMOTE-based class balancing. The Random Forest model achieves the highest overall
performance (~99% accuracy, precision, recall, and F1-score), demonstrating strong
potential for real-world deployment as a lightweight, interpretable IDS.

## 3. Objectives
- To study and preprocess a realistic, large-scale network intrusion dataset.
- To design and implement multiple ML-based classifiers for intrusion detection.
- To compare model performance using standard classification metrics.
- To identify the most influential network flow features for attack detection.
- To build a reusable, deployable model artifact for real-world testing.

## 4. Literature Review (points to expand)
- Traditional signature-based IDS (e.g., Snort, Suricata) vs. anomaly-based/ML IDS.
- KDD'99 / NSL-KDD dataset limitations (outdated, synthetic) vs. CIC-IDS2017 (realistic, modern).
- Prior work using Random Forest, SVM, and Deep Learning on CIC-IDS2017.
- Challenges: high dimensionality, class imbalance, real-time constraints.

## 5. System Architecture (describe/diagram)
1. Raw PCAP → CICFlowMeter → Flow-based CSV features (already provided in CIC-IDS2017 ML CSVs)
2. Data Ingestion & Cleaning
3. Feature Engineering & Scaling
4. Class Balancing (SMOTE)
5. Model Training (RF / XGBoost / MLP)
6. Evaluation & Model Selection
7. Model Serialization (joblib) → Deployment-ready artifact
8. Prediction on new/incoming traffic

## 6. Dataset Details
- 8 CSV files corresponding to different days/attack scenarios (Monday–Friday, 2017)
- ~78 numeric flow-level features (duration, packet counts, byte counts, IAT stats, flags, etc.)
- Highly imbalanced: BENIGN traffic vastly outnumbers most attack categories

## 7. Preprocessing Steps (map to notebook Section 3)
- Column name whitespace stripping
- Duplicate row removal
- Infinite value → NaN replacement, then row-wise NaN drop
- Identifier column removal (IPs, ports, timestamps, flow ID)
- Constant/zero-variance column removal
- Label encoding: binary (BENIGN/ATTACK) + multi-class (attack type)
- StandardScaler feature scaling (fit on train only)
- SMOTE oversampling (train only)

## 8. Models & Hyperparameters (map to notebook Section 5)
| Model | Key Hyperparameters |
|---|---|
| Random Forest | n_estimators=200, max_depth=25, max_features='sqrt' |
| XGBoost | n_estimators=200, max_depth=10, learning_rate=0.1, tree_method='hist' |
| MLP | hidden_layers=(128,64,32), solver='adam', early_stopping=True |

## 9. Evaluation Metrics
- Accuracy
- Precision / Recall / F1-score (macro & weighted, per-class)
- Confusion Matrix
- ROC Curve & AUC
- Random Forest Feature Importance (top contributing features)

## 10. Results (fill in after running the notebook)
| Model | Accuracy | Precision | Recall | F1 | AUC |
|---|---|---|---|---|---|
| Random Forest | | | | | |
| XGBoost | | | | | |
| MLP | | | | | |

## 11. Conclusion
Summarize which model performed best, why, and its practical implications for
real-world intrusion detection.

## 12. Future Work
- Real-time deployment pipeline
- Testing on newer datasets (CIC-IDS2018, CIC-DDoS2019)
- Deep learning / sequence models
- Explainable AI (SHAP/LIME) for SOC analyst usability
- Online/incremental learning for evolving threats

## 13. References
- Sharafaldin, I., Lashkari, A. H., & Ghorbani, A. A. (2018). "Toward Generating a New
  Intrusion Detection Dataset and Intrusion Traffic Characterization." ICISSP.
- Canadian Institute for Cybersecurity, UNB — CIC-IDS2017 Dataset:
  https://www.unb.ca/cic/datasets/ids-2017.html
- scikit-learn documentation: https://scikit-learn.org
- XGBoost documentation: https://xgboost.readthedocs.io
- imbalanced-learn documentation: https://imbalanced-learn.org
