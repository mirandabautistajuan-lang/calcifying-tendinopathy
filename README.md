# calcifying-tendinopathy
**Artificial Intelligence applied to conventional shoulder radiography for calcific tendinopathy of the rotator cuff (TCMR).**

This repository contains the experimental pipeline (Jupyter notebooks) used to develop and evaluate AI models for **binary classification of shoulder X-rays (presence vs absence of TCMR)**, including analyses of **generalization** under technical and clinical variability.

---

## Project summary

Calcific tendinopathy of the rotator cuff (TCMR) is a frequent cause of shoulder pain. Conventional radiography (X-ray) is the **first-line imaging technique** to identify calcific deposits. This project develops and evaluates an AI-based approach to detect TCMR on standard shoulder radiographs and studies the factors that affect performance and generalization in real-world settings.

### Main objective
Develop and evaluate an AI model for classifying shoulder radiographs according to the **presence/absence of TCMR**, analyzing technical and methodological factors that influence diagnostic performance and generalization.

### Specific objectives (mapped to experiments)
1. Compare a deep learning (DL) approach against classical machine learning (ML) approaches.
2. Evaluate the impact of the **acquisition/processing domain** on generalization (domain shift).
3. Analyze the effect of different **multi-channel input strategies**.
4. Study the influence of **training set size** on performance and stability.
5. Evaluate generalization to **other radiographic projections** not used for training.
6. Validate generalization to an **external dataset**.

---

## Methods (high level)

### DL backbone (classification)
All experiments use a CNN built on **VGG19 (ImageNet pretrained)** with transfer learning / fine-tuning. A **Global Max Pooling (GMP)** layer is included before the final sigmoid classifier.

### ROI cropping via automated segmentation
To optimize pre-processing and reduce background noise, a segmentation step was developed to automatically segment the humeral head and crop a standardized ROI. For the segmentation model, **U-Net++ with EfficientNet-b2 encoder** was selected as a reference configuration (good quality–cost tradeoff) and used in the preprocessing pipeline for multiple experiments.

### Explainability
Model behavior is assessed qualitatively using **Grad-CAM** to verify whether predictions rely on clinically meaningful regions (e.g., calcific deposits) rather than spurious artifacts.

---

## Repository structure

### Notebooks (recommended order)

**Data & preprocessing**
- `01_preprocess_dicom.ipynb`  
  DICOM renaming/cleaning, windowing, normalization, resizing, RGB replication, metadata extraction.
- `02_data_merge_dicom_clinical.ipynb`  
  Merge clinical/form variables with DICOM metadata; build final labels/tables.
- `03_eda_dataset_statistics.ipynb`  
  Descriptive statistics, cohort exploration.
- `04_data_build_subsets.ipynb`  
  Build datasets/splits/subsets for experiments.

**Segmentation / ROI**
- `00_Unet_training_models.ipynb`  
  Train segmentation models used for ROI cropping (U-Net family).

**Model training & embeddings**
- `05_train_cnn_model.ipynb`  
  Train the CNN classifier (baseline DL pipeline).
- `06_extract_cnn_embeddings_for_ml.ipynb`  
  Extract deep embeddings (e.g., from VGG19 + GMP) for hybrid ML models.
- `07_train_ml_on_embeddings.ipynb`  
  Train classical ML models on embeddings (+ optional clinical variables).

**Experiments (evaluation)**
- `08_eval_exp01_baseline_cnn_vs_ml.ipynb`  
  Experiment 1: CNN vs hybrid CNN+ML.
- `09_eval_exp02_station_effect_distribution_shift.ipynb`  
  Experiment 2: domain shift impact.
- `10_eval_exp03_multichannel_input_strategies.ipynb`  
  Experiment 3: multi-channel input strategies.
- `11_eval_exp04_training_set_size_ablation.ipynb`  
  Experiment 4: training size ablation.
- `12_eval_exp05_projection_shift_ap_abd.ipynb`  
  Experiment 5: projection generalization.
- `13_eval_exp06_external_institution_generalization.ipynb`  
  Experiment 6: external dataset generalization.

### Other folders
- `Databases/`  
  Local data placeholders / metadata tables (do **not** upload sensitive data).
- `Models/`  
  Trained model artifacts / checkpoints (if included, consider using releases or LFS).

---

## Ethics, privacy and data governance

This repository shares **code and workflow only**. No patient data is included.

All medical imaging and clinical data used in this project were handled under appropriate institutional approvals and data governance procedures, in line with applicable ethical standards and regulations. If you plan to reproduce these experiments, you must ensure compliance with your local policies, ethics review requirements, and data protection regulations, and you should not upload any sensitive data to this repository.

---

## Environment

This work used:
- Python 3.10 (notebooks)
- R 4.1.0 (some analyses, if applicable)

Key Python libraries typically used in this workflow include: NumPy, Pandas, pydicom, TensorFlow, OpenCV, Matplotlib, scikit-learn, tqdm.

Quick start (example):
- Create a virtual environment: `python3 -m venv .venv`
- Activate it: `source .venv/bin/activate`
- Upgrade pip: `python -m pip install --upgrade pip`
- Install dependencies (if you have a requirements file): `pip install -r requirements.txt`

---

## Notes on GitHub rendering of notebooks

GitHub may not render very large notebooks (“Sorry, this is too big to display.”).  
If you need web-friendly viewing:
- keep lightweight notebooks (e.g., cleared outputs), or
- publish HTML renders with GitHub Pages (recommended for large output-heavy notebooks).

---

## Authors

- **Juan Miranda Bautista** (lead)
- Pablo Menéndez Fernández-Miranda
