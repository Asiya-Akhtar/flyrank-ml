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
