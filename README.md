# FlyRank ML — Content Refresh Opportunity Model

**Applied Search Intelligence: Content Refresh Prioritization**

## Project Overview

This project is a machine-learning workflow for prioritizing existing content that may deserve a refresh review.

The system analyzes an anonymized dataset of search and engagement signals, compares a transparent baseline with several machine-learning models, and produces a ranked content-refresh queue for human review.

The goal is not to automatically change content or predict Google's ranking algorithm. The goal is to provide a practical decision-support tool that helps SEO and content teams decide which pages should be investigated first.

---

## Who This Is For

This project is designed for:

- SEO teams
- Content teams
- Editors
- Search analysts
- Teams managing a large library of existing pages

It is most useful when a team has many pages to review and needs a consistent way to prioritize potential refresh opportunities.

---

## Problem

Content teams may have many existing pages that could potentially benefit from updating, but reviewing every page with the same priority is inefficient.

The question addressed by this project is:

> **Can a machine-learning model prioritize pages associated with declining performance better than a simple hand-written baseline?**

The project therefore focuses on ranking the most useful candidates for human review rather than attempting to automate editorial decisions.

---

## What the System Does

The workflow:

1. Prepares the anonymized dataset.
2. Builds the feature vector and target label.
3. Creates a transparent baseline score.
4. Trains multiple machine-learning models.
5. Evaluates the candidate models.
6. Selects the strongest model using Precision@50.
7. Generates a ranked refresh queue.
8. Provides recommended actions and supporting signals for human review.

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
          +-------------+-------------+
          |             |             |
          v             v             v
      Logistic      Decision       Random
     Regression       Tree         Forest
          |             |             |
          +-------------+-------------+
                        |
                        v
                 Model Evaluation
                        |
                        v
                  Random Forest
                        |
                        v
              Ranked Refresh Queue
                        |
                        v
                  Human Review

Pipeline

The main pipeline is:

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

The scripts perform the following tasks:

Script	Purpose
01_prepare_features.py	Cleans the data, builds features, and defines the target
02_baseline_score.py	Creates a transparent hand-written baseline score
03_train_model.py	Trains Logistic Regression, Decision Tree, and Random Forest
04_evaluate_and_export.py	Evaluates the models and generates the ranked queue
05_build_pdf_report.py	Builds a shareable PDF report
run_all.py	Runs the complete workflow
Repository Structure
flyrank-ml/
├── data/
├── docs/
├── notebooks/
├── outputs/
├── scripts/
├── skills/
├── work/
├── README.md
├── SETUP.md
├── GUIDE.md
├── DATA_USE.md
└── requirements.txt

The work/ directory contains my assignment notebooks, experiments, figures, and capstone work.

Setup
Requirements
Python 3.10 or newer
Git
pip
Clone the repository
git clone https://github.com/Asiya-Akhtar/flyrank-ml.git
cd flyrank-ml
Create a virtual environment

On Windows:

python -m venv .venv
.venv\Scripts\activate

On macOS/Linux:

python3 -m venv .venv
source .venv/bin/activate
Install dependencies
pip install -r requirements.txt
Running the Project

Run the complete pipeline with:

python scripts/run_all.py

The pipeline prepares the data, creates the baseline, trains the candidate models, evaluates them, and generates the ranked refresh queue and supporting outputs.

Results are written to the outputs/ directory.

Important outputs include:

outputs/model_report.md
outputs/refresh_queue_sample.csv
outputs/charts/
Example Usage

After running:

python scripts/run_all.py

the project generates a ranked refresh queue.

The queue contains information such as:

rank
score
model probability
recommended action
reason codes
supporting search/engagement signals
trend information

Example action categories include:

monitor
refresh
refresh_and_review_ctr
refresh_and_review_engagement
expand_and_refresh

The queue is designed to help a human reviewer decide which pages to investigate first.

It is not an automatic content-publishing system.

V2 Evaluation Results

The models were evaluated using several metrics, with Precision@50 used as the primary model-selection metric.

Model	ROC AUC	Average Precision	Precision@50	Recall	F1
Baseline Rules	0.627	0.468	0.240	—	—
Logistic Regression	0.700	0.522	0.400	0.567	0.566
Decision Tree	0.742	0.575	0.540	0.716	0.634
Random Forest	0.750	0.618	0.740	0.744	0.640
Model Selection

Random Forest was selected because Precision@50 was the primary selection metric.

The baseline achieved a Precision@50 of 0.240, while Random Forest achieved 0.740.

The practical reason for choosing Precision@50 is that the goal is to make the highest-priority recommendations useful to a human reviewer. The system does not need to treat every page equally; it needs to prioritize the most useful candidates near the top of the queue.

The results therefore show a substantial improvement over the transparent baseline on the primary ranking metric.

Note: Precision@50 is not the same as overall accuracy. The 0.740 result means the evaluated top-50 recommendations had a Precision@50 of 0.740.

Limitations

This system has several important limitations.

1. Limited available features

The dataset is anonymized and limited to the features available in the provided data. The model cannot use contextual information that is not represented in the dataset.

2. Association does not prove causation

The model identifies patterns associated with declining performance. It does not prove why a page declined.

A high-risk score should therefore be treated as a signal for investigation rather than proof that a page needs a specific change.

3. Human review is required

The ranked queue is a decision-support tool. Recommendations should be reviewed by an appropriate SEO or content professional before action is taken.

4. It does not predict Google's algorithm

The model should not be interpreted as a model of Google's ranking algorithm.

It learns patterns from the available dataset and uses those patterns to prioritize potential refresh opportunities.

5. Generalization

Model performance may change when the data distribution, features, or execution environment changes.

Data Safety

The project uses an anonymized dataset.

The repository should not contain:

client names
private domains
private URLs
page titles
private keywords
credentials
other confidential client information

Results should be described using terms such as:

observed
measured
directional
decision-support

The project does not make unsupported causal claims about search rankings.

AI Transparency

AI tools were used during development as assistants for code interpretation, debugging, documentation, and workflow planning.

I reviewed the generated work, ran the project, inspected the outputs, and verified the evaluation results myself.

AI supported parts of the development process, but the final project decisions, testing, evaluation interpretation, documentation review, and submission preparation were checked by me.

Design Decision

One important design decision was selecting Precision@50 as the primary model-selection metric.

The practical purpose of the system is to help a reviewer decide which pages to investigate first. Therefore, the quality of the highest-ranked recommendations matters more than treating every page equally.

This is why Precision@50 was prioritized when selecting the final model.

Future Improvements

Potential future improvements include:

adding more contextual features
testing temporal validation on additional time periods
improving model calibration
adding more explainability to individual recommendations
monitoring model performance over time
incorporating reviewer feedback into future model versions
building a dashboard for the refresh queue
Project Documentation

Additional project documentation is available in:

SETUP.md — environment and setup instructions
GUIDE.md — project and workflow guide
DATA_USE.md — data-use and safety guidance
work/ — assignment notebooks, experiments, figures, and capstone work
outputs/ — evaluation results and generated artifacts
