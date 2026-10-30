# Lead Scoring Workbench

A bank sells term deposits by phone and calls customers at random. This project predicts **which customers are
likely to say "yes"**, so the bank can call the best customers first.

- **Backend:** FastAPI (`app.py`)
- **Frontend:** HTML, CSS and JavaScript (`templates/`, `static/`)
- **Dataset:** `data/raw/bank.csv` (11,162 customers)

## How to run

In a terminal inside this folder:

```bash
.venv/Scripts/python app.py
```

Then open <http://127.0.0.1:5000> in your browser.

| Other commands | |
|---|---|
| Train all models again | `.venv/Scripts/python -m src.pipelines.train_pipeline` |
| Run the tests | `.venv/Scripts/python -m pytest` |

If the `.venv` folder is missing, create it first:

```bash
python -m venv .venv
```

```bash
.venv/Scripts/python -m pip install -r requirements.txt
```

## Dataset

- **Predict:** `deposit`, did the customer say yes or no? (47% said yes)
- **Inputs:** age, job, marital status, education, balance, loans, contact type, number of calls, past campaign result
- **Removed:** `duration` (call length). It is only known after the call, so using it would be cheating.

## Models

| Type | Model | Code |
|---|---|---|
| Classification | Logistic Regression | `src/components/classifiers.py` |
| Classification | Decision Tree | `src/components/classifiers.py` |
| Classification | Random Forest | `src/components/classifiers.py` |
| Regression | Simple Linear Regression | `src/components/regressors.py` |
| Regression | Multiple Linear Regression | `src/components/regressors.py` |

Regression predicts a customer's **age** by default, because the dataset has no other number worth predicting.

## Results

Tested on 2,233 customers the models did not train on (20% of the data).

| Classification | Accuracy | F1 | ROC-AUC |
|---|---|---|---|
| Logistic Regression | 0.695 | 0.636 | 0.758 |
| Decision Tree | 0.686 | 0.630 | 0.728 |
| **Random Forest** | **0.723** | **0.681** | **0.776** |

| Regression (age) | R² | Average error |
|---|---|---|
| Simple Linear | 0.01 | 9.7 years |
| **Multiple Linear** | **0.50** | **6.9 years** |

## Folders

```
ML/
├── app.py            # backend: connects the website to the models
├── templates/        # the web page
├── static/           # web page style and scripts
├── data/raw/         # the dataset
├── src/components/   # the code: data, models, training, scores
├── src/pipelines/    # train all models / predict new customers
├── models/           # saved trained models
├── reports/          # score tables saved by training
├── notebooks/        # data exploration
├── tests/            # automated tests
└── configs/          # settings (test size, random seed)
```
