# Content Opportunity Prioritization Using Search Intelligence Signals

**Author:** Asiya Akhtar  
**Lane:** Refresh / Content Opportunity Scoring  
**Repo:** https://github.com/Asiya-Akhtar/flyrank-ml  
**Date:** September 2026  

---

# 0. Abstract

This research investigates whether machine-learning models can identify historical content observations matching a defined declining-content condition and prioritize them for human review. The analysis uses the FlyRank ML Internship dataset and compares a simple rule-based baseline with Logistic Regression, Decision Tree, and Random Forest models. The models were evaluated using the same client-holdout validation design, with Precision@50 as the primary ranking metric. Random Forest achieved a measured Precision@50 of 0.748 compared with 0.240 for the baseline, approximately 3.08 times the baseline result on the selected validation split. The resulting model output is used as a ranked decision-support queue rather than an automatic content-management system.

---

# 1. Problem Framing

## Research Question

Can a machine-learning model identify content that matches the defined declining-content label and help prioritize which observations should be reviewed first?

## Decision Supported

The analysis supports a practical content-prioritization decision:

**Which content observations should a human reviewer investigate first?**

The unit of analysis is an individual content/search-performance observation.

The output is a ranked priority queue.

A human reviewer can use the queue to decide where to spend review time first.

The system is not intended to automatically publish, delete, redirect, or rewrite content.

## Why Data and ML Help

A large content dataset can contain many observations that cannot all be reviewed manually at the same time. A repeatable scoring or modeling approach can help organize these observations and focus attention on higher-priority cases.

The purpose of the model is therefore decision support rather than autonomous content management.

---

# 2. Data Safety

## Data Source

This research uses the FlyRank ML Internship dataset and the historical search-performance records available in the approved dataset release.

The data was used to prepare features, define the modeling target, train models, evaluate model performance, and generate ranked recommendations.

## Data Used

The modeling workflow uses historical signals that are appropriate for the defined decision-support task.

The project also contains current-state signals such as:

- `days_since_last_update`
- `impressions_90d`
- `avg_position`
- `ctr`

The Week-4 baseline specifically used current-state signals and excluded outcome-derived fields from the score.

## Excluded Information

Potentially problematic variables were excluded from the modeling features when they directly revealed the outcome or could introduce leakage.

In particular, outcome-derived or future-looking variables such as:

- `trend_direction`
- `trend_pct`
- `is_declining_label`

were not used as ordinary predictive features when they directly encoded the target.

Pseudonymous identifiers were also not used as predictive features.

## Public Safety

The public research artifact does not expose:

- client names;
- private URLs;
- private search queries;
- credentials;
- raw sensitive exports;
- personally identifying information.

The results are presented as measured research evidence and decision-support recommendations.

---

# 3. Baseline

Before testing machine-learning models, I created a simple rule-based baseline.

The baseline is intentionally transparent so that the machine-learning models have a clear and understandable benchmark.

## Baseline Signals

The baseline identifies two main signals.

### Staleness

An observation is flagged as stale when:

`days_since_last_update >= 90`

### Low CTR Despite Visibility

An observation is flagged when:

- `impressions_90d >= 500`
- `avg_position` is between 1 and 20
- `ctr < 0.5`

The baseline combines these signals into an action score.

The strongest cases receive additional priority when both signals are present.

## Why the Baseline Matters

The baseline provides a simple reference point.

The purpose of modeling is not to demonstrate that machine learning is automatically better. Instead, the model must provide measured improvement over a transparent rule under the same evaluation conditions.

---

# 4. Model / Analysis

## Method

I treated the problem as a classification and ranking task.

I compared several approaches:

1. Rule-based baseline
2. Logistic Regression
3. Decision Tree
4. Random Forest

The models were selected to provide a progression from a simple interpretable model to a more flexible tree-based ensemble.

## Logistic Regression

Logistic Regression provides a simple and interpretable machine-learning comparison.

It is useful for checking whether the available features contain predictive information beyond the hand-written baseline.

## Decision Tree

A Decision Tree can model nonlinear relationships and feature interactions while remaining relatively easy to inspect.

## Random Forest

Random Forest combines multiple decision trees and can capture more complex relationships and interactions between features.

It was evaluated because the task may contain nonlinear patterns that a single linear model cannot represent.

## Target

The target represents whether an observation matches the defined declining-content condition in the approved dataset.

This makes the modeling task a classification problem.

Variables that directly define or reveal the target were not used as predictive features.

---

# 5. Evaluation

## Validation Design

The models and baseline were evaluated using the same client-holdout validation design.

A client-level separation helps reduce the risk that observations from the same client appear in both the training and evaluation groups.

Using the same held-out evaluation data makes the model comparison more direct and reduces the risk of comparing methods under different conditions.

## Primary Metric

The primary ranking metric was:

**Precision@50**

Precision@50 measures how many of the highest-ranked 50 observations match the defined positive condition.

This metric is useful for the practical question because the goal is to prioritize a limited number of observations for human review.

## Model vs Baseline

| Approach | Precision@50 | ROC-AUC |
|---|---:|---:|
| Baseline Rules | 0.240 | 0.500 |
| Logistic Regression | 0.400 | 0.700 |
| Decision Tree | 0.540 | 0.742 |
| Random Forest | 0.748 | — |

## Main Result

Random Forest achieved the strongest measured Precision@50:

**0.748**

The rule-based baseline achieved:

**0.240**

The Random Forest result was approximately:

**3.08× the baseline Precision@50.**

Decision Tree achieved 0.540, while Logistic Regression achieved 0.400.

Therefore, under this validation setup, Random Forest produced the strongest measured ranking performance among the tested approaches.

## Error Analysis

The ranking result should not be interpreted as perfect classification.

Some observations will still be ranked incorrectly. A high-ranked observation may not actually require a content change, while a lower-ranked observation may deserve attention.

These errors are important because the model is intended to prioritize human review rather than replace human judgment.

The result therefore supports a workflow in which reviewers inspect the evidence behind the highest-ranked observations before taking action.

---

# 6. Interpretation

The measured results suggest that the Random Forest was more effective than the simple baseline at placing observations matching the defined declining-content condition near the top of the review queue.

The improvement from the baseline to Random Forest indicates that the available historical features contain useful information for the defined ranking task under the selected validation design.

However, this is an observed and measured result, not proof of causation.

The analysis does not establish:

- why a content observation declined;
- that a particular content change will improve performance;
- that the model predicts Google's ranking algorithm;
- that the model will perform identically on future data;
- that the observed relationship is causal.

The Random Forest result should therefore be described as **directional, measured, and decision-support oriented**.

A negative result from a simpler model would also have been useful because the purpose of the comparison is to determine whether additional complexity produces measured value.

---

# 7. Ranked Recommendations

The model output can be converted into a ranked action queue.

The purpose of the queue is to help a content reviewer focus limited review time on higher-priority observations.

## Recommended Workflow

### 1. Review the highest-ranked observations first

Start with observations receiving the strongest model priority.

### 2. Check the evidence

Review the supporting signals and available context.

### 3. Decide the appropriate action

A reviewer may determine that an observation should be:

- improved;
- refreshed;
- monitored;
- investigated further;
- or left unchanged.

### 4. Keep the final decision with a human

The model should not directly publish or modify content.

### 5. Monitor the result

Continue observing relevant signals after an action is taken.

### 6. Regenerate the ranking when the data becomes stale

The ranking should be reviewed when the underlying data or assumptions change substantially.

## Action Mapping

| Content Situation | Suggested Action |
|---|---|
| Strong performance | Protect / Monitor |
| Declining performance | Review / Improve |
| Recovering performance | Monitor |
| Potentially outdated content | Refresh Review |
| Weak or unclear evidence | Human Investigation |

These are decision-support recommendations rather than automatic instructions.

## Human Review Requirements

Before acting on a recommendation, a reviewer should:

1. Check the actual content.
2. Check the current search intent.
3. Check whether the signals are still current.
4. Consider the strategic importance of the content.
5. Decide whether a refresh or improvement is actually appropriate.
6. Consider current editorial and business requirements.

The model score should never be treated as sufficient evidence by itself.

---

# 8. Reproducibility

The research artifacts are maintained in the project repository.

## Main Research Notebook

`work/notebooks/capstone.ipynb`

## Weekly Supporting Notebooks

- `work/notebooks/w01_research_question.ipynb`
- `work/notebooks/w02_ml_task_framing.ipynb`
- `work/notebooks/w03_data_contract.ipynb`
- `work/notebooks/w04_baseline_score.ipynb`
- `work/notebooks/w05_model.ipynb`
- `work/notebooks/w06_validation_audit.ipynb`
- `work/notebooks/w07_action_playbook.ipynb`

## Action Playbook

`work/notebooks/w07_action_playbook.ipynb`

The action queue is generated from the notebook rather than manually copied into the research artifact.

## Repository

https://github.com/Asiya-Akhtar/flyrank-ml

## Paper URL

The deployed paper URL is stored in:

`submission/paper_url.txt`

That file should contain exactly one line containing the final deployed paper URL.

---

# 9. Acknowledgments & Data Credit

Built on the FlyRank ML Internship dataset.

Data source: FlyRank

https://flyrank.ai

---

# Conclusion

This study tested whether machine-learning models could improve the prioritization of content observations matching a defined declining-content condition.

The Random Forest achieved the strongest measured Precision@50 at 0.748 compared with 0.240 for the rule-based baseline on the selected client-holdout validation split.

The result indicates that the Random Forest provided stronger measured ranking performance than the simple baseline under this evaluation design.

The result should nevertheless be treated as directional decision-support evidence rather than causal evidence or a prediction of search-engine behavior.

The recommended use is therefore a ranked review queue: the model identifies observations to investigate first, while a human reviewer remains responsible for deciding what action, if any, should be taken.
