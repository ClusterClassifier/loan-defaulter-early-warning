# Analytical Observations — Credit Risk Analysis

**Dataset:** Berka (Czech Bank, 1993–1998) | **Loans:** 682 | **Transactions:** 1,056,320

---

## Business Question

> Can observable transaction behaviour during a loan term flag borrowers likely
> to default — before the first missed payment?

---

## Key Findings

### 1. Sanction Interest Is a Near-Certain Default Indicator

Accounts charged sanction interest (penalty transactions) during their loan term
defaulted at **90%** — compared to **0.7%** for never-penalised accounts.
This is a **128x risk multiplier**.

**Business action:** Any sanction transaction on an active loan account should
trigger an immediate collections call — no model required.

---

### 2. Overdraft Is Present in 4 Out of 5 Defaults

**78.9%** of all 76 defaulting accounts went into negative balance at some point
during their loan term. Accounts that never went negative defaulted at just **2.6%**.

**Business action:** Overdraft occurrence is the highest-weight signal in any
monthly risk scoring system. Flag immediately on first occurrence.

---

### 3. Defaulters Never Accumulate — Non-Defaulters Do

Across the full loan lifecycle, non-defaulters maintained a stable median balance
above **40,000 CZK** — growing consistently over time. Defaulters stayed persistently
below **28,000 CZK** with no accumulation.

Critically, this gap exists from **month zero** — before any repayment behaviour.
It is a pre-existing characteristic, not a consequence of loan stress.

**Business action:** Average account balance at loan origination should be a
primary underwriting criterion. Accounts below threshold warrant closer scrutiny
before approval.

---

### 4. Defaulters' Finances Are Structurally Unstable

Two independent measures confirm instability as a default characteristic:

- **Balance volatility (CV):** 0.57 for defaulters vs. 0.33 for non-defaulters.
  Defaulter boxes barely overlap with non-defaulters — this is a clean separation.
- **Income CV:** 0.91 for defaulters vs. 0.35 for non-defaulters. Income arrives
  irregularly, making consistent monthly obligations structurally difficult.

**Business action:** Volatility and income regularity should be scored at
origination from the applicant's existing account history.

---

### 5. EMI Above 15% of Account Balance Is a Risk Signal

Non-defaulters consistently maintained an EMI-to-balance ratio of **8–10%** throughout the loan term. Defaulters ranged between **13–21%** with high oscillation.

This ratio is observable every month — making it a trackable in-loan signal,
not just an origination check.

**Business action:** Monthly EMI-to-balance above 15% contributes to the
cumulative risk score. Sustained elevation across 2+ months warrants outreach.

---

### 6. Loan Overextension at Origination Predicts Default

**58.1%** of defaulters took loans exceeding **500% of their initial account balance** — compared to **27.9%** of non-defaulters in the same bracket.

**Business action:** Loans exceeding 5x the applicant's current account balance
carry disproportionate default risk. Require additional collateral or reduce
approved amount.

---

## What Does Not Predict Default

| Signal                            | Finding                                       |
| --------------------------------- | --------------------------------------------- |
| Cash withdrawal frequency         | 2.95 vs. 3.10 — negligible difference         |
| Credit-to-debit ratio (quarterly) | Groups indistinguishable from 1994 onward     |
| Quarter of loan issuance          | No temporal pattern — random oscillation      |
| Transaction frequency             | Weak — directional only, not actionable alone |

---

## Proposed Deployment — Monthly Cumulative Scoring

Every month per active loan account, score observable signals:

| Signal                             | Trigger             | Weight                       |
| ---------------------------------- | ------------------- | ---------------------------- |
| Sanction interest                  | Any occurrence      | Hard flag — immediate action |
| Overdraft                          | Any occurrence      | Highest                      |
| Balance below threshold            | Sustained 2+ months | High                         |
| EMI-to-balance above 15%           | Sustained 2+ months | Medium                       |
| Income CV above threshold          | Loan term average   | Low-medium                   |
| Balance volatility above threshold | Loan term average   | Low-medium                   |

Cumulative score crosses defined threshold → account flagged for intervention.
Weights are illustrative — logistic regression coefficients determine actual values.

---

## Documented Limitations

- 682 labeled loans — signals are validated, model generalisability requires larger data
- Czech 1990s context — thresholds need recalibration on Indian banking data
- Installment debits not recorded in transaction ledger — default label used as-is

---

*Prepared alongside `behavioural_credit_risk_analysis.ipynb`*
