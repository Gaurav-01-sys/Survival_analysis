# 📊 Kaplan-Meier & Cox Proportional Hazards Survival Analysis

This project applies **survival analysis** to model customer churn using both the **Kaplan-Meier Estimator** and the **Cox Proportional Hazards (Cox-PH) Model**. The goal is to estimate customer retention over time and understand the factors influencing churn. It includes metric evaluation, survival probability estimation, feature interpretation, and diagnostic tests for modeling assumptions.

---

## 📌 Kaplan-Meier Estimator

The **Kaplan-Meier Estimator (KMF)** is a **non-parametric** method used to estimate the survival function from lifetime data.

### 🔹 Survival Function

The survival function is defined as: S(t) = P(T > t)


Where:

- `T` is the time until the event occurs (churn).
- `t_i` is the time an event occurs.
- `d_i` is the number of events at `t_i`.
- `n_i` is the number of subjects at risk just before `t_i`.

The Kaplan-Meier curve is a **step function** that decreases at each event time.

---

### 🔍 Application in Customer Churn

- Splits dataset into **70% training** and **30% testing**.
- Trains Kaplan-Meier on `tenure` and `churn status`.
- Predicts survival probabilities on test data.
- Computes churn probability as: P(Churn by t) = 1 - S(t)


---

### 📈 Evaluation Metrics

| Metric                           | Test Set  | Full Dataset |
|----------------------------------|-----------|---------------|
| **Concordance Index (C-Index)**  | 0.9886    | 0.9888        |
| **Brier Score**                  | 0.2500    | 0.2435        |

- **C-Index**: Predictive accuracy (1 = perfect, 0.5 = random).
- **Brier Score**: Measures mean squared difference between predicted and actual outcomes (lower = better).

---

### 📅 Churn Probabilities at Key Time Points

| Time (Months) | Churn Probability |
|---------------|--------------------|
| 1             | 0.0540             |
| 3             | 0.0863             |
| 6             | 0.1152             |
| 12            | 0.1568             |
| 24            | 0.2113             |

---

## ⚙️ Cox Proportional Hazards Model (Cox-PH)

The **Cox-PH model** estimates the effect of covariates (e.g., contract type, charges) on the hazard rate of churn.

### 💡 Model Equation : h(t | X) = h0(t) * exp(βX)


Where:

- `h(t | X)`: hazard at time `t` given features `X`.
- `h0(t)`: baseline hazard function.
- `β`: regression coefficients.

---

### 🔄 Kaplan-Meier vs. Cox-PH

| Kaplan-Meier                | Cox-PH Model                         |
|----------------------------|--------------------------------------|
| Non-parametric             | Semi-parametric                      |
| Descriptive survival curve | Explains why events occur            |
| No covariates              | Uses covariates to predict hazard    |

---

### 🧹 Preprocessing for Cox-PH

- Remove multicollinearity (VIF > 15)
- Drop low-variance columns (threshold < 0.1)
- Handle missing values
- Remove redundant one-hot encoded features

---

### 📈 Cox-PH Results

| Metric                           | Value   |
|----------------------------------|---------|
| **Concordance Index (C-Index)**  | 0.9265  |
| **Brier Score**                  | 0.1908  |

---

### 🔮 Churn Predictions (3 Months)

- **Probability of staying** after 3 months: `0.9153`
- **Probability of churning** within 3 months: `0.0847`

---

### 🧠 Feature Interpretations

| Feature           | Coefficient | HR   | p-value | Interpretation                                 |
|------------------|-------------|------|---------|------------------------------------------------|
| Dependents       | -0.14       | 0.87 | 0.04    | 13% lower churn risk – more likely to stay     |
| PaperlessBilling | 0.22        | 1.25 | <0.005  | 25% higher churn risk – may reflect low loyalty|

---

## ⚠️ Proportional Hazards Assumption

The **PH assumption** means hazard ratios are **constant over time**.

### ✅ Testing PH Assumption

- **Schoenfeld Residuals**: should be randomly scattered.
- **Grambsch-Therneau Test**:
  - **H0**: PH assumption holds.
  - **Ha**: PH assumption is violated.

**If p-value < 0.05**, the assumption is **violated**.

---

### 🛠️ Remedies for PH Violation

- Add **time-dependent covariates** (e.g., `log(time)`).
- Use **stratified Cox model**.
- Try **parametric models** (e.g., Weibull, Gompertz).

---

## ✅ Future Considerations

- Use survival curves to **predict churn after n months**.
- Extend analysis to **different customer segments**.
- Combine results with **marketing efforts** for retention.

---

## 🧪 Code Snippet Example

```python
# Predict survival probability at 3 months
survival_prob = kmf.predict(3)
churn_prob = 1 - survival_prob






