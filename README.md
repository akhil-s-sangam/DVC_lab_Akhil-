# 📦 DVC + Google Cloud + Model Versioning

## IE-7374 MLOps Lab Submission

---

# 📌 Overview

This lab demonstrates the integration of:

* **Git** for code versioning
* **DVC (Data Version Control)** for dataset and model versioning
* **Google Cloud Storage (GCS)** as remote storage
* **Scikit-learn** for model training

Unlike the base lab, this implementation extends beyond data tracking by also versioning a trained machine learning model using DVC.

---

# 🎯 Objectives

* Track dataset versions using DVC
* Configure Google Cloud Storage as remote backend
* Train a machine learning model
* Track and version the trained model artifact
* Demonstrate dataset updates and reproducibility
* Restore previous dataset and model versions

---

# 🏗️ Project Structure

```
DVC_lab_Akhil/
│
├── data/
│   ├── CC_GENERAL.csv
│   ├── CC_GENERAL.csv.dvc
│   └── .gitignore
│
├── model/
│   ├── model.pkl
│   └── model.pkl.dvc
│
├── train.py
├── requirements.txt
├── .dvc/
├── .gitignore
└── README.md
```

---

# ⚙️ Technologies Used

* Python 3.x
* DVC
* Google Cloud Storage
* Scikit-learn
* Pandas
* Git

---

# ☁️ Google Cloud Setup

1. Created a new GCP project
2. Created a Cloud Storage bucket (`us-east1`)
3. Created a service account
4. Generated JSON key
5. Configured DVC remote:

```bash
dvc remote add -d lab2 gs://<bucket-name>
dvc remote modify lab2 credentialpath <service-account.json>
```

---

# 📊 Dataset

Dataset used:

* `CC_GENERAL.csv` (Credit Card Customer Dataset)

### 🔹 Modifications Made (Beyond Base Lab)

1. Added a new feature:

   ```
   TOTAL_SPENDING = PURCHASES + CASH_ADVANCE
   ```

2. Performed basic preprocessing:

   * Removed null values
   * Selected numerical features

3. Filtered customers with:

   ```
   CREDIT_LIMIT > 5000
   ```

These changes ensure the lab is not identical to the base repository.

---

# 🤖 Model Training

A **KMeans Clustering model** was implemented to segment customers.

### Model Training Script: `train.py`

Workflow:

1. Load dataset
2. Feature engineering
3. Train KMeans model
4. Save model as `model.pkl`

Example:

```python
from sklearn.cluster import KMeans
import pickle

model = KMeans(n_clusters=3, random_state=42)
model.fit(X)

with open("model/model.pkl", "wb") as f:
    pickle.dump(model, f)
```

---

# 🔄 DVC Tracking

### Track Dataset

```bash
dvc add data/CC_GENERAL.csv
git add data/CC_GENERAL.csv.dvc
git commit -m "Track dataset with DVC"
```

### Track Model

```bash
dvc add model/model.pkl
git add model/model.pkl.dvc
git commit -m "Track trained model"
```

### Push to Cloud

```bash
dvc push
```

This uploads:

* Dataset
* Model artifact

to Google Cloud Storage.

---

# 🔁 Simulating Dataset Update

To simulate real-world updates:

1. Modified dataset
2. Re-ran:

```bash
dvc add data/CC_GENERAL.csv
git commit -m "Updated dataset version"
dvc push
```

DVC generated a new hash for the updated dataset.

---

# ♻️ Reproducing Previous Versions

To restore an older version:

```bash
git checkout <commit-hash>
dvc checkout
```

DVC automatically restores:

* Correct dataset version
* Correct model version

This ensures full reproducibility.

---

# 📈 Key Enhancements Over Base Lab

✔ Implemented KMeans clustering model
✔ Versioned model artifact using DVC
✔ Demonstrated dataset update simulation
✔ Verified reproducibility using Git + DVC
✔ Integrated Google Cloud remote storage


# 🚀 How to Reproduce This Project

### 1️⃣ Clone Repo

```bash
git clone <repo-url>
cd DVC_lab_Akhil
```

### 2️⃣ Install Dependencies

```bash
pip install -r requirements.txt
pip install "dvc[gs]"
```

### 3️⃣ Pull Data & Model

```bash
dvc pull
```

### 4️⃣ Run Training

```bash
python train.py
```

---

# 🏁 Conclusion

This lab demonstrates a complete MLOps workflow:

* Data versioning
* Model versioning
* Cloud storage integration
* Reproducibility
* Experiment traceability

By extending the base lab with model training and artifact tracking, this implementation better reflects real-world machine learning lifecycle management.

---

# 👤 Author

Akhil Satyabodh Sangam
IE-7374 – MLOps
Spring 2026