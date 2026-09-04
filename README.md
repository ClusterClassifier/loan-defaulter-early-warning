# Behavioural Early-Warning Signals for Loan Default

> An analysis of whether bank-account behaviour during a loan term is associated with eventual loan default.

## Overview

Banks usually react after a borrower misses a payment, when recovery is more difficult and the customer relationship has already deteriorated. This project examines whether transaction-ledger behaviour can reveal earlier signs of financial stress.

Using the Berka / PKDD'99 financial dataset, the analysis compares accounts that eventually defaulted with those that did not. It focuses on observable patterns in balance stability, income regularity, affordability pressure, overdrafts, and sanction interest.

This repository is an **analytical and feature-engineering project**. It does **not** present a trained prediction model as a reliable or deployable solution.

## Business Question

> Which behavioural signals in a borrower's bank transactions are associated with eventual loan default during the loan term?

The aim is to translate transaction history into evidence that Credit Risk and Collections teams could investigate before a missed payment occurs. The project identifies and quantifies signals; it does not claim to make production decisions for individual borrowers.

## Why There Is No Final Prediction Model

The dataset contains 682 labelled loan records, including only 76 defaults. That is sufficient for exploratory comparison and feature engineering, but too limited to establish a robust, generalisable predictive model.

In particular:

- a small default cohort makes estimates sensitive to a few accounts;
- all records come from one Czech bank between 1993 and 1998;
- the ledger does not directly identify the date of first missed repayment;
- synthetic expansion was evaluated and excluded because it did not preserve the real joint relationships between loan and transaction behaviour.

Logistic regression was explored as an interpretable baseline only. Its results must not be interpreted as validated deployment performance, credit approval logic, or a production risk score.

## Dataset

The project uses the Berka financial dataset (PKDD'99), an anonymised Czech-bank dataset covering 1993–1998.

| Table              | Scale                  | Purpose                                                              |
| ------------------ | ----------------------:| -------------------------------------------------------------------- |
| `loans.csv`        | 682 loan records       | Loan amount, term, installment, and eventual-default outcome         |
| `transactions.csv` | 1,056,320 transactions | Account balances, credits, debits, operations, and transaction dates |

`account_id` connects each loan to its transaction history. The outcome variable, `defaulter`, indicates whether the account eventually defaulted during the observed loan term; it does not identify the month of default.

## Analytical Approach

1. Audit the loan and transaction data, including date ranges, null patterns, category mappings, and join-key integrity.
2. Restrict transactions to each account's loan period.
3. Build one account–loan-month observation from the transaction ledger.
4. Calculate behavioural indicators such as average balance, balance volatility, credit consistency, affordability pressure, overdraft occurrence, sanction occurrence, and transaction activity.
5. Compare defaulting and non-defaulting accounts using distributions, lifecycle plots, and statistical tests.
6. Translate findings into a transparent early-warning framework for further validation on a larger institution-specific dataset.

## Key Findings

The analysis identifies several high-value signals associated with eventual default:

| Signal               | Observed pattern                                                                                                                        |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Sanction interest    | Accounts with sanction interest defaulted at about 90%, compared with about 0.7% for accounts without it.                               |
| Overdraft            | 78.9% of defaulting accounts entered negative balance; accounts that never went negative had a 2.6% default rate.                       |
| Balance accumulation | Non-defaulters maintained a median balance above 40,000 CZK, while defaulters remained below 28,000 CZK without sustained accumulation. |
| Balance volatility   | Median balance volatility was higher for defaulters (0.57) than non-defaulters (0.33).                                                  |
| Income consistency   | Income variability was higher for defaulters (income CV 0.91) than non-defaulters (0.35).                                               |
| EMI pressure         | Defaulters generally had an EMI-to-balance ratio of 13–21%, versus 8–10% for non-defaulters.                                            |
| Loan overextension   | 58.1% of defaulters had loans greater than five times their initial balance, compared with 27.9% of non-defaulters.                     |

Cash-withdrawal frequency and total transaction frequency showed weak standalone separation. They are retained as contextual features, not presented as primary risk signals.

## Interpreting the Findings

These results support a two-layer analytical framework:

- **Immediate review signals:** sanction interest and an account entering overdraft.
- **Sustained-deterioration signals:** persistently low balance, irregular income, increasing balance instability, and elevated EMI-to-balance pressure.

The framework is a business interpretation of observed associations. It requires further validation, governance review, and recalibration before it could inform real lending or collections decisions.

## Project Outputs

- A cleaned and documented Berka loan and transaction dataset.
- Loan-period transaction analysis and account–month feature engineering.
- Comparative visualisations of defaulting versus non-defaulting account behaviour.
- Statistical observations describing the strongest early-warning associations.
- A Power BI dashboard communicating portfolio-level risk patterns and signal strength.

## Limitations

- **Small labelled sample:** 682 loans and 76 default outcomes are not enough for reliable model generalisation.
- **Historical single-bank context:** the dataset represents one Czech bank from 1993–1998; values and thresholds cannot be transferred directly to another bank or country.
- **No missed-payment timestamp:** installment debits and the first missed payment are not directly available in the ledger.
- **Association is not causation:** sanctions, overdrafts, and other signals may be correlated with default without independently causing it.
- **No synthetic-data claim:** synthetic expansion was not retained because it failed fidelity checks on important loan–behaviour relationships.

## Responsible Use

This repository is for educational and analytical use. It should not be used to approve, deny, price, or collect a real customer's loan. A production system would require a larger representative dataset, temporal validation, fairness assessment, calibration, monitoring, policy approval, and human oversight.

# 
