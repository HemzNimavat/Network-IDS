# saved_model/

This folder is where trained model artifacts get placed.

After running the notebook (`notebook/AI_IDS_CICIDS2017.ipynb`) in Google Colab,
download these generated files from `/content/saved_model/` in Colab and place them here:

- `rf_ids_binary_model.pkl` — trained Random Forest binary classifier
- `scaler.pkl` — fitted StandardScaler used during training
- `label_encoder_binary.pkl` — fitted LabelEncoder for BENIGN/ATTACK labels
- `feature_columns.pkl` — list of feature column names, in the order the model expects

These files are required together to correctly preprocess and run predictions on new data.
