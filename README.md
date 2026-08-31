# FlyRank ML — Content Refresh Opportunity Model

**Applied Search Intelligence: Content Refresh Prioritization**

## Project Overview

This project is a machine-learning workflow for prioritizing existing content that may deserve a refresh review.

The system analyzes an anonymized dataset of search and engagement signals, compares a transparent baseline with several machine-learning models, and produces a ranked content-refresh queue for human review.

The goal is not to automatically change or publish content, and it is not to predict Google's ranking algorithm. Instead, the project provides a decision-support workflow that helps SEO and content teams identify which pages may be worth investigating first.

---

## Who This Is For

This project is designed for:

- SEO teams
- Content teams
- Editors
- Search analysts
- Teams managing a large library of existing web pages

It is especially useful when a team has many existing pages and needs a consistent way to prioritize potential content-refresh opportunities.

---

## Problem

Content teams may have many existing pages that could potentially benefit from updating. Reviewing every page with the same priority can be inefficient.

The question addressed by this project is:

> **Can a machine-learning model prioritize pages associated with declining performance better than a simple hand-written baseline?**

The project therefore focuses on ranking potential refresh candidates for human review rather than automatically making editorial decisions.

---

## What the System Does

The workflow:

1. Prepares the anonymized dataset.
2. Builds the feature vector and target label.
3. Creates a transparent baseline score.
4. Trains multiple machine-learning models.
5. Evaluates the candidate models.
6. Compares the models using ranking and classification metrics.
7. Selects the strongest model using Precision@50.
8. Generates a ranked refresh queue.
9. Provides supporting signals and recommended actions for human review.

The main candidate models are:

- Logistic Regression
- Decision Tree
- Random Forest

---

## Architecture

```text
                 Anonymized Search Data
                           |
                           v
                 Feature Preparation
                           |
                           v
                 Transparent Baseline
                           |
                           v
                  Model Training
          +----------------+----------------+
          |                |                |
          v                v                v
     Logistic          Decision          Random
    Regression           Tree            Forest
          |                |                |
          +----------------+----------------+
                           |
                           v
                    Model Evaluation
                           |
                           v
                     Best Model
                           |
                           v
                Ranked Refresh Queue
                           |
                           v
                     Human Review
```

---

## Pipeline

The complete pipeline follows these stages:

```text
01_prepare_features.py
        |
        v
02_baseline_score.py
        |
        v
03_train_model.py
        |
        v
04_evaluate_and_export.py
        |
        v
05_build_pdf_report.py
```

### Pipeline Stages

| Script | Purpose |
|---|---|
| `01_prepare_features.py` | Cleans the data, prepares features, and defines the target |
| `02_baseline_score.py` | Creates a transparent hand-written baseline score |
| `03_train_model.py` | Trains Logistic Regression, Decision Tree, and Random Forest |
| `04_evaluate_and_export.py` | Evaluates the models and generates the ranked queue, charts, and report |
| `05_build_pdf_report.py` | Creates a shareable PDF summary |
| `run_all.py` | Runs the complete workflow |

---

## Repository Structure

```text
flyrank-ml/
│
├── data/
│
├── docs/
│
├── notebooks/
│
├── outputs/
│
├── scripts/
│
├── skills/
│
├── work/
│   ├── notebooks/
│   ├── scripts/
│   ├── figures/
│   └── capstone_report.md
│
├── README.md
├── SETUP.md
├── GUIDE.md
├── DATA_USE.md
├── requirements.txt
└── LICENSE
```

### Important Directories

| Directory | Purpose |
|---|---|
| `scripts/` | Runnable reference pipeline |
| `notebooks/` | First-win notebooks and project notebooks |
| `work/` | Assignment notebooks, experiments, figures, and capstone work |
| `outputs/` | Generated model results, reports, queues, and charts |
| `docs/` | Core project documentation and data dictionary |
| `data/` | Anonymized project data |

---

## Setup

A stranger should be able to reproduce the project from a fresh clone by following the steps below.

### Requirements

- Python 3.10 or newer
- Git
- pip

### Clone the Repository

```bash
git clone https://github.com/Asiya-Akhtar/flyrank-ml.git
cd flyrank-ml
```

### Create a Virtual Environment

#### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

#### macOS/Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Running the Project

After installing the dependencies, run the complete pipeline with:

```bash
python scripts/run_all.py
```

This runs the complete workflow:

```text
Data Preparation
      ↓
Baseline Scoring
      ↓
Model Training
      ↓
Model Evaluation
      ↓
Ranked Refresh Queue
      ↓
PDF Report
```

The generated results are written to the `outputs/` directory.

---

## Example Usage

Run:

```bash
python scripts/run_all.py
```

After the pipeline completes, inspect the generated files in:

```text
outputs/
```

Important output artifacts include:

```text
outputs/
├── model_report.md
├── refresh_queue_sample.csv
├── charts/
└── other generated evaluation files
```

The ranked refresh queue provides a prioritized list of pages that may deserve further investigation.

The output is intended to support human decision-making. It does not automatically modify, publish, or remove content.

---

## Evaluation Approach

The project compares a transparent hand-written baseline against several machine-learning models.

The candidate models are:

1. Logistic Regression
2. Decision Tree
3. Random Forest

The evaluation uses model performance metrics to determine which approach provides the most useful prioritization.

The primary ranking metric is **Precision@50**.

---

## V2 Evaluation Results

The v2 evaluation showed a substantial improvement over the baseline when measuring the quality of the highest-priority recommendations.

| Model | Precision@50 |
|---|---:|
| Baseline Rules | 0.240 |
| Logistic Regression | 0.400 |
| Decision Tree | 0.540 |
| **Random Forest** | **0.740** |

### Baseline

The transparent baseline achieved:

**Precision@50 = 0.240**

### Random Forest

The Random Forest model achieved:

**Precision@50 = 0.740**

This represents approximately a three-times improvement over the baseline on the Precision@50 metric.

The exact model value can vary depending on the execution environment and library versions, so the notebooks should be treated as the live source of the current evaluation result.

---

## Why Precision@50?

Precision@50 was selected as the primary model-selection metric because the practical purpose of the system is to help a human reviewer decide which pages should be investigated first.

The system does not need to treat every page equally.

Instead, it needs to prioritize a smaller number of potentially useful candidates near the top of the queue.

For this reason, Precision@50 directly reflects the practical goal of the project:

> **How useful are the highest-priority recommendations for human review?**

---

## Model Selection

Random Forest was selected as the preferred model because it produced the strongest Precision@50 result among the evaluated models.

The comparison was:

```text
Baseline Rules       0.240
Logistic Regression  0.400
Decision Tree        0.540
Random Forest        0.740
```

The Random Forest model is therefore used as the strongest approach for producing the prioritized refresh queue.

The result should still be interpreted as decision support rather than an automatic decision.

---

## Refresh Queue

The final workflow produces a ranked refresh queue.

The queue is intended to help a human reviewer understand:

- which pages should be investigated first
- how strongly the model prioritizes each page
- what signals contributed to the recommendation
- what type of review may be appropriate

The generated queue can contain information such as:

- rank
- score
- model probability
- recommended action
- reason codes
- supporting search signals
- engagement signals
- trend information

Example recommendation categories include:

```text
monitor
refresh
refresh_and_review_ctr
refresh_and_review_engagement
expand_and_refresh
```

These recommendations are not automatic editorial decisions.

A human reviewer should evaluate the page and its context before taking action.

---

## Design Decision

One important design decision was using **Precision@50** as the primary model-selection metric.

The actual purpose of this project is prioritization.

If a content team has hundreds or thousands of pages, the most immediate question is not simply whether the model can classify every page correctly.

The more practical question is:

> **Which pages should the team investigate first?**

Precision@50 was therefore chosen because it focuses evaluation on the quality of the highest-ranked recommendations.

This connects the evaluation metric directly to the real-world use of the system.

---

## Limitations

This project has several important limitations.

### 1. Limited Features

The model can only learn from the features available in the anonymized dataset.

Information that is not represented in the dataset cannot be considered by the model.

### 2. Association Does Not Prove Causation

The model identifies patterns associated with declining performance.

It does not prove why a page declined.

A high model score should therefore be treated as a signal for investigation rather than proof that a particular change will improve performance.

### 3. Human Review Is Required

The ranked queue is a decision-support tool.

A human SEO or content professional should review the recommendations before making editorial decisions.

### 4. It Does Not Predict Google's Algorithm

The model should not be described as a model of Google's ranking algorithm.

It learns patterns from the available dataset and uses those patterns to prioritize potential content-refresh opportunities.

### 5. Generalization

Model performance may change when the data distribution, available features, or execution environment changes.

The reported results therefore describe performance on the evaluated dataset and validation setup.

### 6. Dataset Scope

The project uses an anonymized dataset provided for the internship workflow.

The results should not automatically be assumed to generalize to every website, industry, search environment, or future dataset.

---

## Data Safety

This project uses anonymized data.

The repository should not contain:

- client names
- private domains
- private URLs
- private page titles
- private keywords
- credentials
- confidential client information

The project should use language such as:

- observed
- measured
- directional
- decision-support

The project does not claim to predict Google's ranking algorithm.

Private client data should never be added to this public repository.

Private client information should also not be pasted into third-party AI tools.

---

## Reproducibility

The project is designed so that another person can reproduce the workflow from the repository instructions.

The basic process is:

```bash
git clone https://github.com/Asiya-Akhtar/flyrank-ml.git
cd flyrank-ml
pip install -r requirements.txt
python scripts/run_all.py
```

Experiments should use fixed random seeds where applicable.

Any environment-specific differences in results should be documented rather than hidden.

The notebooks and generated evaluation outputs provide additional evidence of the work performed.

---

## AI Transparency

AI tools were used during development as assistants for code interpretation, debugging, documentation, reasoning, and workflow planning.

I reviewed the generated work, ran the project, inspected the outputs, and checked the evaluation results myself.

AI assisted parts of the development process, but the final project decisions, testing, interpretation of results, documentation review, and submission preparation were checked by me.

---

## What I Built

The project combines:

- data preparation
- feature engineering
- baseline scoring
- machine-learning model training
- model evaluation
- ranking
- recommendation generation
- documentation
- reproducible project structure

The final workflow turns model predictions into a ranked content-refresh queue intended for human review.

---

## What I Learned

The main lesson from this project is that the machine-learning model is only one part of a useful ML system.

The complete workflow matters:

```text
Problem Framing
      ↓
Data Preparation
      ↓
Baseline
      ↓
Model
      ↓
Evaluation
      ↓
Recommendation
      ↓
Human Review
```

I also learned that the evaluation metric should match the actual decision the system is designed to support.

For this project, prioritizing the most useful pages for review made Precision@50 an important metric.

---

## Future Improvements

Potential future improvements include:

- adding additional contextual features
- testing the model on additional time periods
- improving probability calibration
- adding more detailed explanations for individual recommendations
- monitoring model performance over time
- incorporating reviewer feedback
- testing additional model families
- improving recommendation reason codes
- creating a dashboard for the ranked refresh queue
- evaluating the workflow on additional datasets

---

## Assignment Work

The assignment notebooks are stored in:

```text
work/notebooks/
```

The workspace contains notebooks and materials covering:

- research question
- ML task framing
- data contract
- feature leakage checks
- signal audit
- baseline scoring
- model development
- validation audit
- action playbook
- capstone

The main capstone notebook is:

```text
work/notebooks/capstone.ipynb
```

The `work/` directory also contains:

```text
work/
├── notebooks/
├── scripts/
├── figures/
└── capstone_report.md
```

This directory contains the project-specific experiments and capstone materials.

---

## Documentation

Additional documentation is available in the repository:

- **`SETUP.md`** — setup, GitHub, Colab, and data-access instructions
- **`GUIDE.md`** — repository guide and explanation of the project files
- **`DATA_USE.md`** — data-use and safety requirements
- **`docs/`** — ML framework, lane guide, tooling guide, and data dictionary
- **`work/`** — assignment notebooks, experiments, figures, and capstone
- **`outputs/`** — generated model results and artifacts

---

## FL-09 Demo

The FL-09 demo is a 3–5 minute live demonstration of the project.

The demo shows the real project running rather than using presentation slides.

The demonstration covers:

1. The project structure
2. The end-to-end workflow
3. The model evaluation
4. The ranked refresh queue
5. The Precision@50 design decision
6. One limitation of the system

**Demo Video:**

PASTE YOUR 3–5 MINUTE DEMO VIDEO LINK HERE

---

## Final Submission

**GitHub Repository:**

https://github.com/Asiya-Akhtar/flyrank-ml

**FL-09 Demo Video:**

PASTE YOUR DEMO VIDEO LINK HERE

---

## License

The project code is provided under the repository license.

Data-use and safety requirements are documented in `DATA_USE.md`.
