# Model Card — Fraud Detection Ensemble v[VERSION]

> **Template Instructions**: Complete all sections before any model is deployed to production. Sections marked [REQUIRED] must be completed; [RECOMMENDED] sections should be completed where information is available. This model card should be updated at each retraining cycle and when thresholds or configurations change materially.

---

## Model Overview

| Field | Value |
|---|---|
| **Model Name** | Fraud Detection Ensemble |
| **Version** | [VERSION] |
| **Model Type** | Ensemble: Gradient Boosting + Isolation Forest + Rule Engine |
| **Training Date** | [DATE] |
| **Training Data Window** | [START DATE] to [END DATE] |
| **Deployed** | [DEPLOYMENT DATE] |
| **Model Owner** | [NAME] — Fraud Product |
| **ML Owner** | [NAME] — Data Science |
| **Last Reviewed** | [DATE] |

---

## Intended Use [REQUIRED]

### Primary Purpose

This model is designed to assess the fraud risk of financial transactions in real time, supporting three decision types:
- Auto-approve (score below the auto-approve threshold)
- Step-up authentication trigger (medium risk score)
- Analyst review queue placement (high risk score)
- Hard block (above the block threshold, or rule engine trigger)

### Intended Users

- Fraud detection system (automated inference)
- Fraud operations analysts (case review)
- Fraud product and risk functions (threshold governance)

### Out-of-Scope Uses

This model is **not** intended for:
- Credit decisioning or creditworthiness assessment
- Customer segmentation or marketing targeting
- Account closure decisions (separate model and process)
- Regulatory reporting (model outputs are inputs to human judgment, not direct regulatory outputs)

---

## Training Data [REQUIRED]

| Field | Value |
|---|---|
| **Training dataset size** | [N transactions] |
| **Training window** | [START] to [END] — [N months] |
| **Fraud rate in training data** | [X.XX%] |
| **Positive labels (fraud)** | [N] transactions |
| **Negative labels (legitimate)** | [N] transactions |
| **Label sources** | Chargebacks, analyst decisions, SAR-linked transactions |
| **Label minimum age** | [N] days (transactions labeled within [N] days excluded) |
| **Class imbalance handling** | SMOTE + cost-sensitive learning (class weight: [X]) |
| **Feature schema version** | [VERSION] |

### Data Quality Checks at Training Time

| Check | Result | Notes |
|---|---|---|
| Label completeness | [PASS / FAIL] | [Notes] |
| Feature availability > 95% | [PASS / FAIL] | [Notes] |
| Schema match | [PASS / FAIL] | [Notes] |
| Label age compliance | [PASS / FAIL] | [Notes] |
| Minimum volume | [PASS / FAIL] | [Notes] |

---

## Model Performance [REQUIRED]

### Validation Dataset

| Field | Value |
|---|---|
| **Validation set size** | [N transactions] |
| **Validation window** | [START] to [END] |
| **Fraud rate in validation set** | [X.XX%] |
| **Validation set construction** | [Time-based split / stratified sample — describe] |

### Core Metrics at Operating Threshold

| Metric | Value | Threshold Requirement | Pass/Fail |
|---|---|---|---|
| Precision | [X.XX] | > 0.82 | [PASS/FAIL] |
| Recall | [X.XX] | > 0.78 | [PASS/FAIL] |
| AUC-ROC | [X.XX] | > 0.94 | [PASS/FAIL] |
| Brier Score (calibration) | [X.XX] | < 0.05 | [PASS/FAIL] |
| F1 Score | [X.XX] | Reference only | — |

### Performance by Fraud Type

| Fraud Type | Precision | Recall | Volume (% of fraud) |
|---|---|---|---|
| Card-Not-Present (CNP) | [X.XX] | [X.XX] | [XX%] |
| Account Takeover (ATO) | [X.XX] | [X.XX] | [XX%] |
| Authorized Push Payment (APP) | [X.XX] | [X.XX] | [XX%] |
| First-Party Fraud | [X.XX] | [X.XX] | [XX%] |
| Identity / New Account Fraud | [X.XX] | [X.XX] | [XX%] |

### Threshold Configuration

| Decision Tier | Score Threshold | Expected Volume |
|---|---|---|
| Auto-approve (score below) | [X.XX] | [XX%] of transactions |
| Step-up authentication | [X.XX] to [X.XX] | [XX%] of transactions |
| Analyst review queue | [X.XX] to [X.XX] | [XX%] of transactions |
| Hard block (score above) | [X.XX] | [XX%] of transactions |

Threshold review date: [DATE]
Threshold approved by: [Fraud Product Lead], [Risk Lead]

---

## Features [REQUIRED]

### Top 10 Features by Importance

| Rank | Feature Name | Category | Importance Score |
|---|---|---|---|
| 1 | [Feature] | [Category] | [Score] |
| 2 | [Feature] | [Category] | [Score] |
| 3 | [Feature] | [Category] | [Score] |
| 4 | [Feature] | [Category] | [Score] |
| 5 | [Feature] | [Category] | [Score] |
| 6 | [Feature] | [Category] | [Score] |
| 7 | [Feature] | [Category] | [Score] |
| 8 | [Feature] | [Category] | [Score] |
| 9 | [Feature] | [Category] | [Score] |
| 10 | [Feature] | [Category] | [Score] |

### Feature Schema Version

Feature schema: [VERSION]
Feature store: [ENVIRONMENT / TABLE REFERENCE]
Graph feature version: [VERSION]

---

## Limitations and Known Issues [REQUIRED]

### Model Limitations

**Known blind spots:**
- [ ] [Describe specific fraud types or patterns the model performs poorly on and why]
- [ ] [Describe feature coverage gaps that limit detection in specific scenarios]
- [ ] [Describe customer segments where model accuracy is lower]

**Explainability limitations:**
- SHAP values are accurate for the gradient boosting component; the isolation forest contribution to the ensemble score is not decomposable at the feature level
- Explanations may not capture the full influence of network features due to graph query depth limitations at inference time

**Adversarial robustness:**
- The model is not adversarially hardened against deliberate manipulation of observable features by informed fraudsters
- Detection of novel, unseen fraud patterns is dependent on the isolation forest component, which has higher false positive rates than the supervised model

### Operational Limitations

- Graph features degrade in availability during graph database maintenance windows; degraded-graph performance has not been fully characterized on recent data
- Model accuracy is characterized on the training distribution; performance on customer segments underrepresented in historical data may be lower
- Label quality for authorized push payment fraud is lower than other fraud types due to difficulty in distinguishing authorized fraud from legitimate transfers at transaction time

---

## Fairness Assessment [RECOMMENDED]

> Fraud models should be evaluated for disparate impact across protected characteristics. This section documents the fairness evaluation conducted prior to deployment.

### Metrics Evaluated

| Characteristic | FP Rate | FN Rate | Significant Disparity? |
|---|---|---|---|
| [Characteristic 1] | [X.XX] | [X.XX] | [Yes/No] |
| [Characteristic 2] | [X.XX] | [X.XX] | [Yes/No] |

### Findings and Mitigations

[Document any disparities found and what mitigations were applied or are planned]

---

## Regulatory Compliance [REQUIRED]

| Requirement | Status | Evidence |
|---|---|---|
| Legitimate Interests Assessment completed | [Yes/No] | [Reference] |
| SHAP explainability implemented for blocks | [Yes/No] | [Reference] |
| Human review pathway documented | [Yes/No] | [Reference] |
| Sanctions screening independent of model | [Yes/No] | [Reference] |
| Model risk management review completed | [Yes/No] | [Reference] |
| Senior Manager sign-off | [Yes/No] | [Name, Date] |

---

## Deployment Approval [REQUIRED]

| Role | Name | Sign-off Date |
|---|---|---|
| Fraud Product Lead | [Name] | [Date] |
| ML / Data Science Lead | [Name] | [Date] |
| Risk Function | [Name] | [Date] |
| Compliance | [Name] | [Date] |
| Model Risk Management | [Name] | [Date] |

---

## Shadow Deployment Results [REQUIRED]

| Metric | Current Production Model | New Model (Shadow) | Difference |
|---|---|---|---|
| Precision | [X.XX] | [X.XX] | [+/- X.XX] |
| Recall | [X.XX] | [X.XX] | [+/- X.XX] |
| Block rate | [X.XX%] | [X.XX%] | [+/- X.XX%] |
| Queue volume | [N] | [N] | [+/- N] |

Shadow deployment duration: [N] hours
Shadow period: [START DATE] to [END DATE]
Promotion decision: [PROMOTED / HELD — reason]

---

## Change Log

| Version | Date | Change Summary | Changed By |
|---|---|---|---|
| [VERSION] | [DATE] | [Summary] | [Name] |
