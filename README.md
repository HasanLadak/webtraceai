# WebTrace AI: AI-Generated Website Detector

WebTrace AI classifies a website screenshot as **AI-generated** (built with tools like v0, Cursor or Lovable) or **human-designed**. It extracts 43 hand-engineered visual features with OpenCV and runs them through a scikit-learn classifier, served by a FastAPI backend with a React + Vite drag-and-drop frontend.

> Team project for **CMPT 310 (Artificial Intelligence) at Simon Fraser University**, Summer 2025.
> Original repository: [muneebkk/webtraceai](https://github.com/muneebkk/webtraceai)

## How it works

```
Screenshot ──▶ Preprocessing ──▶ Feature extraction (43 features) ──▶ Classifier ──▶ AI / Human + confidence
                (OpenCV)          color · layout · texture · structure    (Random Forest,
                                                                          Logistic Regression,
                                                                          Decision Tree)
```

- **Color (12):** diversity, harmony, temperature, vibrancy, consistency
- **Layout (10):** edge density, contour count, symmetry, grid structure, visual hierarchy
- **Texture (8):** contrast, entropy, energy, homogeneity
- **Structure (9):** border regularity, padding consistency, element spacing, alignment

The user picks one of three models in the UI and gets a prediction, a confidence score and a breakdown of which features drove the decision.

## Dataset

A custom-labelled dataset of **150 website screenshots**:

| Source | Label | Count |
|---|---|---|
| v0 | AI | 25 |
| Cursor | AI | 25 |
| Lovable | AI | 25 |
| Hand-built sites | Human | 75 |

Labels are in [`dataset/labels.csv`](dataset/labels.csv).

## Results

The Random Forest was the strongest model on our held-out test split, at roughly **79% accuracy**. Logistic Regression scored about 74%. Because the dataset is small, these numbers vary between training runs and should be read as a proof of concept rather than a production benchmark.

## Tech stack

**ML:** Python, scikit-learn, OpenCV, scikit-image, NumPy
**Backend:** FastAPI, Uvicorn
**Frontend:** React 18, Vite, Tailwind CSS

## Running locally

See [QUICK_START.md](QUICK_START.md). Short version:

```bash
# backend
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000

# frontend (new terminal)
cd frontend
npm install
npm run dev
```

Then open http://localhost:5173 and drop in a screenshot.

## Team

- **Muhammad Muneeb Kamran** ([@muneebkk](https://github.com/muneebkk)): backend, feature extraction module, model training pipeline
- **Muhammad Hadi Ladak**: frontend–backend integration
- **Muhammad Hasan Ladak** ([@HasanLadak](https://github.com/HasanLadak)): dataset, model development and evaluation

## My contributions (Hasan)

- **Built most of the training dataset.** I collected and labelled AI-generated sites from v0, Cursor, Lovable and Framer, plus human-designed sites, and maintained `labels.csv` so the classes stayed balanced.
- **Worked with the team on feature design, model comparison and evaluation.** This covered choosing between Random Forest, Logistic Regression and a custom decision tree, and testing predictions on new screenshots.
- Merged feature branches and resolved integration issues as the project came together.

## Limitations and next steps

- **Small dataset:** 150 images limits how well the model generalizes. The next step is to scale it with automated screenshot capture (for example, Playwright).
- **Hand-crafted features:** a CNN or a pretrained vision model would likely beat them.
- **Unseen AI tools:** the model only knows the three tools it was trained on, and newer AI site builders may look different.
