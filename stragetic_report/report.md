# Strategic Analysis & Recommendations 

## Business Goal
Increase the rate at which promising street footballers are identified and signed by professional clubs, using existing data.

---

## 🔍 Key Insights from EDA

1. **Severe Class Imbalance**: ~75% of players are labeled with `club_interest = 0`, heavily skewing model performance.
2. **Weak Linear Separation**: Most features, including scores and rankings, do not show strong visual or statistical separation between classes, which why LR is not as good as Xgb.
3. **Score Fragmentation**: Many players have low behavior scores (0–1), possibly reflecting rating inconsistencies.
4. **Demographic Concentration**: High concentration of 18-year-olds (~20% of dataset), potentially introducing age-related bias.
5. **Gender Concentration**: High concentration of male players (~80% of dataset), potentially introducing gender-related bias.

---

## Data-Driven Recommendations

### 1. Introduce a Composite Talent Score
- Create a simple average of video, behavior, and coach feedback scores.
- **Why**: Individually weak predictors, but combined they yield stronger predictive power (validated via modeling).
- **Impact**: Summarizes player quality into a single metric and helps the model identify well-rounded players by consolidating performance indicators, improving classification of subtle talent profiles.

---

### 2. Address Class Imbalance via Resampling (SMOTE)
- Augment minority class samples synthetically.
- **Why**: Prevents the model from overfitting to the dominant class (players not yet in clubs).
- **Impact**: Addressed target imbalance, leading to a ~4x increase in recall for signed players, with no loss in accuracy.

---

### 3. Incorporate new synthesized Features
- Add features like:
  -  `is_adult`
  - Score range
- **Why**: Helps model key features like maturity (reaching age 18) and captures inconsistency for players with imbalanced behavior.
- **Impact**: Improved overall F1-score by ~20%.

---

## Expected Business Value

Metrics are based on hold-out test set. 
Improvements driven by SMOTE, feature engineering, and using Xgb since it's outperforming LR:

| Metric | Baseline | After Improvements |
|--------|--------|--------------------|
| Club Interest Recall | 0.36   | 0.86               |
| Signing Accuracy | 0.54   | 0.84               |

- Higher identification rate → More signings → Stronger ROI via club partnerships.
- Model ready-to-compute features allows for real-time prediction in app.

---

## Next Steps

1. Run full cross-validation and further improve features and model.
2. Incorporate MLOps best practices, e.g. data monitoring, model monitoring, etc.
3. Orchestrate data & modeling pipeline (e.g. with Airflow, kubeflow, etc.), productionalise model and deploy as microservice or other. 
4. Export model and deploy as a microservice assuming the app is a set of microservices.

## Missing steps

- Object-oriented programming paradigm. Workflow could be structured differently and better for a production-grade pipeline.
- Proper tests (e.g. unit tests) are missing due to time constraints. Basic assertions
are used in the different notebooks.

## Key challenges:
- Severe class imbalance: ~75% of players are not of interest to clubs, 
making it hard to train effective classifiers.
- Weak individual predictors: Raw scores (video, behavior, coach feedback) 
show low correlation with club interest.
- Noisy demographic signals: Age and gender distributions are skewed, 
introducing potential bias.
- Most features do not separate classes clearly, 
making simpler models like Logistic Regression underperform.
