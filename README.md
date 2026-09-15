# 🛡️ AI-Based Intrusion Detection System (IDS)
### Machine Learning on the CIC-IDS2017 Dataset — MCA Final Year Project

## 📖 Overview
This project implements a complete, end-to-end **Network Intrusion Detection System (NIDS)**
using classical machine learning models trained on the **CIC-IDS2017** dataset. It classifies
network traffic flows as either **BENIGN** or an **ATTACK** (with an optional multi-class
attack-type breakdown), based on ~78 statistical flow features extracted via CICFlowMeter.

Three models are trained and compared:
1. **Random Forest** (primary/recommended model — ~99%+ accuracy)
2. **XGBoost**
3. **Multi-Layer Perceptron (MLP)** Neural Network

## 📂 Project Structure
```
IDS_Project/
│
├── notebook/
│   └── AI_IDS_CICIDS2017.ipynb    # Main Colab notebook (run this)
│
├── saved_model/                   # Trained model artifacts get saved here after running the notebook
│   ├── rf_ids_binary_model.pkl    # (generated) Trained Random Forest model
│   ├── scaler.pkl                 # (generated) Fitted StandardScaler
│   ├── label_encoder_binary.pkl   # (generated) Fitted LabelEncoder
│   └── feature_columns.pkl        # (generated) List of feature columns used
│
├── docs/
│   └── project_report_notes.md    # Notes you can expand into a full project report
│
├── requirements.txt                # Python dependencies (for local/offline use)
├── .gitignore
└── README.md                       # You are here
```

## 🚀 How to Run (Google Colab — Recommended)
1. Go to [Google Colab](https://colab.research.google.com/).
2. Click **File → Upload notebook** and upload `notebook/AI_IDS_CICIDS2017.ipynb`.
3. (Optional but recommended) Go to **Runtime → Change runtime type** and select a
   **High-RAM** runtime, since the combined CIC-IDS2017 CSVs are large (~2.8M rows).
4. Run the cells **top to bottom**.
   - The notebook downloads the dataset via **`kagglehub`** (`chethuhn/network-intrusion-dataset`,
     a CIC-IDS2017 mirror). You'll need a **Kaggle API Token**:
     1. Go to [kaggle.com/settings](https://www.kaggle.com/settings) → **API** section →
        **"Create New Token"**. Copy the token (starts with `KGAT_...`) — it's only shown once.
     2. **Recommended:** store it in **Colab Secrets** (🔑 icon in the left sidebar) under the
        name `KAGGLE_API_TOKEN` — the notebook automatically picks it up from there, so it's
        never hardcoded or visible in the notebook file.
     3. **Fallback:** paste the token directly into the `KAGGLE_API_TOKEN` variable in the
        download cell (remove it again before sharing the notebook).
   - Alternatively, use the **manual Google Drive upload** cell if you've already
     downloaded `MachineLearningCVE.zip` from the
     [official UNB CIC-IDS2017 page](https://www.unb.ca/cic/datasets/ids-2017.html).
5. The notebook will preprocess the data, train all 3 models, generate evaluation
   plots/tables, and save the best model (Random Forest) to `/content/saved_model/`.
6. Download the saved model files from Colab and place them into this project's
   `saved_model/` folder for future reuse.

## 🖥️ How to Run Locally (Jupyter)
```bash
# 1. Create and activate a virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch Jupyter
jupyter notebook notebook/AI_IDS_CICIDS2017.ipynb
```
> Note: The `google.colab.userdata` Secrets lookup is Colab-specific and will fail
> locally (the notebook falls back to the hardcoded `KAGGLE_API_TOKEN` variable in
> that case — set it directly, or set the `KAGGLE_API_TOKEN` environment variable
> yourself before running `kagglehub.dataset_download(...)`). Alternatively, manually
> download the CIC-IDS2017 "MachineLearningCVE" CSVs from the UNB site linked above,
> place them in a folder, and set `DATASET_DIR` to that folder's path.

## 📊 Dataset
- **Name:** CIC-IDS2017 (MachineLearningCSV version)
- **Source:** Canadian Institute for Cybersecurity, University of New Brunswick
- **Link:** https://www.unb.ca/cic/datasets/ids-2017.html
- **Kaggle mirror:** search `cicids2017` on Kaggle
- **Size:** ~2.8 million labeled network flow records, ~78 features per flow
- **Classes:** BENIGN + 14 attack types (DoS, DDoS, PortScan, Brute Force, Web Attacks,
  Botnet, Infiltration, Heartbleed, etc.)

## 🧪 Methodology
1. **Data Loading:** Combine all 8 daily CSV files into one dataframe.
2. **Cleaning:** Remove duplicates, handle infinite/NaN values, drop identifier/constant columns.
3. **Labeling:** Create both binary (`BENIGN`/`ATTACK`) and multi-class targets, label-encoded.
4. **Scaling:** `StandardScaler`, fit only on the training split (no leakage).
5. **Class Imbalance:** `SMOTE` oversampling applied only to the training set.
6. **Modeling:** Random Forest, XGBoost, and MLP trained on the balanced training set.
7. **Evaluation:** Accuracy, Precision, Recall, F1 (macro & weighted), Confusion Matrix,
   ROC-AUC, and Random Forest Feature Importance — all computed on the untouched,
   real-distribution test set.
8. **Deployment Demo:** Best model + preprocessing objects saved via `joblib`, with a
   working prediction example on new/unseen samples.

## 📈 Expected Results
| Model          | Accuracy | Precision (macro) | Recall (macro) | F1-score (macro) |
|----------------|----------|--------------------|-----------------|--------------------|
| Random Forest  | ~0.99+   | ~0.98+             | ~0.98+          | ~0.98+             |
| XGBoost        | ~0.99    | ~0.97+             | ~0.97+          | ~0.97+             |
| MLP            | ~0.97+   | ~0.95+             | ~0.95+          | ~0.95+             |

*(Exact numbers vary slightly by run/seed/hardware.)*

## 🛠️ Tech Stack
- Python 3
- pandas, numpy
- scikit-learn
- XGBoost
- imbalanced-learn (SMOTE)
- matplotlib, seaborn
- joblib

## 🚧 Limitations & Future Work
- CIC-IDS2017 is from 2017 and doesn't include the most recent attack patterns.
- SMOTE-generated synthetic minority samples may not perfectly reflect real rare attacks.
- Not yet tested on live/streaming traffic — only static, pre-extracted flow features.
- Future extensions: test on CIC-IDS2018/CIC-DDoS2019, real-time deployment with
  CICFlowMeter, deep learning (LSTM/CNN) on raw packet sequences, SHAP/LIME
  explainability, and online/incremental learning.

## 👤 Author
[Your Name] — MCA Final Year Project

## 📜 License
This project is for academic/educational purposes. CIC-IDS2017 dataset usage is
subject to the terms specified by the Canadian Institute for Cybersecurity (UNB).
