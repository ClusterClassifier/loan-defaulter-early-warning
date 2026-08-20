# Business Context - The Problem this project solves
## Behavioural Early Warning System for Loan Default

---

## The Problem

Loan defaults cost banks significantly — not just in lost principal, but in recovery
costs, provisioning requirements, and regulatory scrutiny. The standard industry
response is reactive: once a borrower misses a payment, the collections process
begins. By that point, the borrower is already in distress, recovery rates are lower,
and the relationship is damaged.

The more valuable question is not *"how do we recover a defaulted loan"* but
*"how do we detect a borrower drifting toward default before they get there?"*

---

## Why This Is Hard

The obvious answer — flag borrowers whose balance drops below their installment
amount — does not work in practice. Banks process installment debits through
internal loan management systems, not the transaction ledger. The transaction
record does not show individual missed payments. A borrower can be weeks away
from default while their account activity looks entirely normal to a rules-based
monitoring system.

What the transaction ledger *does* show is behavioural drift — subtle, accumulating
signals that precede default without explicitly announcing it.

---

## The Business Question

> **"What behavioural signals does a borrower's bank account show during their
> loan term that reliably precede a default — and can those signals be scored
> monthly to flag at-risk accounts before the first payment is missed?"**

This question is answerable from data a bank already holds — its own transaction
ledger. No external data purchase, no credit bureau API call, no customer
interview required. The signal is already there. The question is whether it can
be systematically extracted and acted upon.

---

## Who This Is For

**Primary stakeholder:** Credit Risk and Collections teams at retail banks and NBFCs.

**Decision this enables:** Monthly identification of active loan accounts showing
sustained behavioural deterioration — triggering proactive outreach, payment
restructuring offers, or credit limit review — before the first missed installment.

**What changes operationally:** Instead of reacting to a missed payment, the
collections team receives a monthly flag list ranked by risk score. They contact
flagged accounts with restructuring offers while the borrower is still current.
Recovery rates are higher. Customer relationships are preserved. NPL formation
is reduced.

---

## Why a Model — Not Just Rules

A significant portion of defaulters show no explicit signal. They deteriorate quietly — balance declining, income becoming irregular, EMI consuming a growing share of their available funds — without
ever crossing into penalty or negative territory.

A logistic regression model trained on behavioural features captures this silent
deterioration. It learns the combination and weight of signals that, together,
predict default even when no single signal crosses a hard threshold. The model
and the rules work in parallel — the rules catch the obvious cases, the model
catches the subtle ones.

---

## The Proposed System

A monthly scoring pipeline operating on the bank's transaction ledger:

```
Transaction data → Feature extraction → Monthly risk score per account
                                              ↓
Score above threshold → Flag for intervention
Score below threshold → Monitor next month
```

Each month, every active loan account receives a cumulative risk score updated
with that month's behavioural signals. The score only rises — sustained
deterioration accumulates. A single bad month does not trigger a false alarm.
A pattern of deteriorating months does.

---

## Dataset

This analysis is built on the **Berka Dataset (PKDD'99)** — the only publicly
available dataset containing real, anonymised bank transaction history and loan
default outcomes for the same customers at the same institution.

| Source | Czech bank, 1993–1998 |
|---|---|
| Transactions | 1,056,320 rows across 4,500 accounts |
| Loans | 682 records with confirmed default outcomes |
| Default rate | ~11% — realistic retail banking class imbalance |

The dataset's single-institution structure mirrors exactly what a bank's internal
data warehouse holds for its own customers — making the analytical approach
directly transferable to a production banking environment, with threshold
recalibration on local data.

---

## What This Project Delivers

- Statistical analysis identifying which transaction signals separate defaulters from non-defaulters
- Logistic regression model with probability output and explainable coefficients
- Power BI dashboard - Single-screen executive view of portfolio risk and signal strength
- Deployment framework - Cumulative monthly scoring system specification

---

*Foundation of the Project - see README.md for full pipeline and setup.*