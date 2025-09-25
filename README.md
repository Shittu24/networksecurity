# Phishing Website Detection

## Project Overview
Phishing attacks are one of the most common cybersecurity threats, where malicious websites impersonate legitimate ones to steal sensitive information such as usernames, passwords, and credit card details.  
This project implements a **machine learning pipeline** to detect phishing websites using URL and webpage-based features.

The model predicts whether a website is **legitimate (1)** or **phishing (0)** based on 30+ handcrafted features extracted from URLs and domain information.

---

## Tech Stack
- **Python 3.10+**
- **Scikit-learn** – Model training & evaluation
- **MLflow** – Experiment tracking & model registry
- **DagsHub** – Remote MLflow tracking server + GitHub integration
- **FastAPI** – API for training & inference
- **Joblib/Pickle** – Model serialization
- **Logging & Exception Handling** – Custom error tracking

---

## Project Structure

├── app.py # FastAPI app entry point
├── main.py # Training pipeline runner
├── networksecurity/
│ ├── components/ # Data ingestion, validation, transformation, trainer
│ ├── entity/ # Configs & artifact entities
│ ├── pipeline/ # TrainingPipeline class
│ ├── utils/ # Helper functions & ML utilities
│ └── exception/ # Custom exception handler
├── artifacts/ # Saved preprocessor, models, metrics
├── final_model/ # Exported final model
├── requirements.txt # Dependencies
└── README.md # Project documentation


---

## Workflow
1. **Data Ingestion** – Load phishing dataset (URLs + features).  
2. **Data Validation** – Schema & missing value checks.  
3. **Data Transformation** – Preprocessing pipeline (scaling, encoding).  
4. **Model Training** – Evaluate multiple classifiers:
   - Random Forest 
   - Decision Tree 
   - Gradient Boosting
   - Logistic Regression
   - AdaBoost  
   Best model is selected based on **F1 score, Precision, Recall**.
5. **MLflow Tracking** – All metrics, params, and models logged to **DagsHub MLflow**.  
   - Example: `Random Forest v4` registered in the model registry.  
6. **Model Deployment** – FastAPI endpoints for:
   - `/train` → retrain pipeline
   - `/predict` → make predictions from new URLs  

---

## Features Used
Some features extracted from URLs/webpages:
- `having_IP_Address` → Whether the URL contains an IP address.  
- `URL_Length` → Legitimate vs suspicious URL length.  
- `Shortining_Service` → Use of services like bit.ly, tinyurl.  
- `SSLfinal_State` → Presence and validity of SSL certificate.  
- `Domain_registeration_length` → Expiry duration of domain.  
- `Google_Index` → Whether site is indexed by Google.  
- `Page_Rank`, `web_traffic`, `DNSRecord` → Domain reputation metrics.  
- `Abnormal_URL`, `Redirect`, `Iframe` → Signs of phishing pages.

---

## Results
- **Best Model**: Random Forest Classifier  
- **Metrics** (example run):
  - F1 Score: `0.95`
  - Precision: `0.94`
  - Recall: `0.96`
- Models tracked in MLflow on [DagsHub](https://dagshub.com/Shittu24/networksecurity.mlflow).

---

## How to Run

### 1. Clone repo
```bash
git clone https://github.com/Shittu24/networksecurity.git
cd networksecurity

**### 2. Create Virtual Environment**
python -m venv venv
# Activate
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate

### 3. Install Dependencies
pip install -r requirements.txt

### 4. Train Pipeline
python main.py

### 5. Run FastAPI App
uvicorn app:app --reload




