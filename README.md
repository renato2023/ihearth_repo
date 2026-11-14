# CardioFL: federated learning and mobile app for coronary artery disease risk assessment

Source code accompanying the manuscript *"A Mobile App for Coronary Artery Disease Risk Assessment Using Federated Learning and a Wearable Band in Adults in Lima, Peru: Development and Preliminary Evaluation Study"* (Universidad Peruana de Ciencias Aplicadas).

> **Not a medical device.** The app provides a preliminary risk signal for research purposes only and does not replace a medical evaluation.

## Repository structure

| Folder | Content |
|---|---|
| `training/` | Jupyter notebook with preprocessing, feature engineering, simulated federated training (FedAvg, 5 virtual clients, 20 rounds), internal evaluation, and the code used for the external evaluation |
| `model/` | Global model of round 20 (`global_model.tflite`, `global_model.keras`), fitted `StandardScaler` (`scaler.pkl`), class/sex encoders, ordered list of the 32 input features, and model metadata |
| `app/` | Flutter app (Android): Health Connect heart rate reading, clinical questionnaire, on-device TensorFlow Lite inference, local SQLite storage |

## Training

```bash
cd training
pip install -r requirements.txt
jupyter notebook federated_cardio_thesis.ipynb
```

The notebook expects `Z-Alizadeh sani dataset.csv` in the same folder. The Z-Alizadeh Sani dataset (Alizadehsani R, Roshanzamir M, Sani Z; UCI Machine Learning Repository, 2013; doi:10.24432/C5Q31T) is redistributed here under its CC BY 4.0 license.

Environment used for the reported run: TensorFlow 2.22.0-rc0, NumPy 2.5.4, pandas 3.1.1, seed 42.

Internal test set (n=61) results of the included model: recall 0.9535, specificity 0.6667, precision 0.8723, F1-score 0.9111, AUC-ROC 0.8178.

## External validation data

Section 12 of the notebook contains the code used to evaluate the model on data from 100 adults collected at Hospital Nacional Guillermo Almenara Irigoyen (Lima, Peru). **These data are not included** because participants did not consent to data sharing.

## App

```bash
cd app
flutter pub get
flutter run
```

Built with Flutter 3.41 / Dart 3.11, Android API 26+. Heart rate is read from Health Connect (synchronized from a Xiaomi Smart Band 10 through Mi Fitness).

**Known limitation:** the current version of the app does not ask the user for blood pressure and uses a fixed value of 130 mm Hg (see `app/lib/modelo/feature_engineering.dart`). If age, weight, or height are missing from the profile, default values (58 years, 74 kg, 165 cm) are used.

## License

MIT (see `LICENSE`). The Z-Alizadeh Sani dataset keeps its original CC BY 4.0 license.
