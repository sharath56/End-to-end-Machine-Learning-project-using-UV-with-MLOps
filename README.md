## End-to-end Machine Learning project using UV with MLOps


### ✅ **Project Overview**

**Goal:** Train a model on the Iris dataset, serve predictions using Flask, and deploy it with GitHub Actions CI/CD and Docker .

---

## 📝 Full `README.md` — Machine Learning Project with `uv`

````markdown
# 🧠 ML Project Tutorial: End-to-End with uv, Flask, and CI/CD

This tutorial demonstrates how to:

1. 🎯 Initialize a Python project using `uv`
2. ⚙️ Install and manage dependencies
3. 🧪 Train a machine learning model
4. 🌐 Deploy it using Flask
5. 🚀 Automate tests and deployments using GitHub Actions
6. 🐳 (Optional) Package with Docker for production

---

## 🔧 Prerequisites

- Python ≥ 3.8
- [uv](https://github.com/astral-sh/uv): `pip install uv`
- GitHub account (for CI/CD)
- Optional: Docker installed for production packaging

---

## 1️⃣ Initialize Your Project

```bash
uv init iris-classifier
cd iris-classifier
````

This creates:

```
.
├── main.py
├── pyproject.toml
├── .python-version
└── README.md
```

---

## 2️⃣ Add ML & Web Dependencies

```bash
uv add scikit-learn flask joblib
```

---

## 3️⃣ Train and Save a Model

### ✏️ Create `main.py`

```python
# main.py

from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier
import joblib

# Load data
X, y = load_iris(return_X_y=True)

# Train model
model = RandomForestClassifier()
model.fit(X, y)

# Save model
joblib.dump(model, "model.pkl")

print("✅ Model trained and saved to model.pkl")
```

### ▶️ Run training

```bash
uv run main.py
```

---

## 4️⃣ Serve Model with Flask

### 📁 Create `app/server.py`

```bash
mkdir app
touch app/__init__.py app/server.py
```

### ✏️ Edit `app/server.py`

```python
# app/server.py

from flask import Flask, request, jsonify
import joblib

app = Flask(__name__)
model = joblib.load("model.pkl")

@app.route("/predict", methods=["POST"])
def predict():
    data = request.json["input"]
    prediction = model.predict([data])
    return jsonify(prediction=prediction.tolist())

if __name__ == "__main__":
    app.run(port=5000)
```

### ▶️ Run the API

```bash
uv run -- python app/server.py
```

### ✅ Test the API

```bash
curl -X POST http://localhost:5000/predict -H "Content-Type: application/json" \
    -d '{"input": [5.1, 3.5, 1.4, 0.2]}'
```

Expected response:

```json
{"prediction": [0]}
```

---

## 5️⃣ Automate with GitHub Actions

### 📁 Create GitHub workflow

```bash
mkdir -p .github/workflows
touch .github/workflows/ci.yml
```

### ✏️ `.github/workflows/ci.yml`

```yaml
name: CI Pipeline

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install uv
      run: pip install uv

    - name: Sync environment
      run: uv sync

    - name: Train model
      run: uv run main.py

    - name: Check model file
      run: ls -lh model.pkl
```

Push to GitHub to trigger the workflow:

```bash
git init
git remote add origin https://github.com/sharath56 End-to-end-Machine-Learning-project-using-UV-with-MLops.git
git commit -m "Initial commit"
git push -u origin main
```

---

## 6️⃣ (Optional) Build with Docker

### 🐳 Create `Dockerfile`

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY . .

RUN pip install uv && uv sync

EXPOSE 5000

CMD ["uv", "run", "--", "python", "app/server.py"]
```

### 🔧 Build and Run

```bash
docker build -t iris-api .
docker run -p 5000:5000 iris-api
```

---

## 💡 MLOps Tips

* 🧪 Add automated tests (`pytest`) for model and API
* 📦 Use DVC or MLflow to track models
* 🔁 Retrain models on schedule with GitHub Actions or Airflow
* 🔐 Store secrets with GitHub Actions Secrets

---

## 📘 Resources

* [uv Documentation](https://github.com/astral-sh/uv)
* [Flask](https://flask.palletsprojects.com/)
* [scikit-learn](https://scikit-learn.org/)
* [GitHub Actions](https://docs.github.com/en/actions)

---

## 🏁 Done!

You now have a complete end-to-end ML project:
✅ Managed with `uv`
✅ Trained with `scikit-learn`
✅ Served with `Flask`
✅ Automated with `GitHub Actions`
✅ (Optional) Dockerized for deployment

```

## Author : SHARATH VN !!!!^_^

