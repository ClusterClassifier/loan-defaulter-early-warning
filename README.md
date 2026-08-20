Behavioural Early Warning System for Loan Default
Flagging Potential Defaulters Before a Missed Payment — Using Real Bank Transaction Data
Business Problem

Banks lose significant capital to loan defaults. By the time a borrower misses their first payment, recovery options are limited and costly. The real opportunity lies before the missed payment — in the borrower's transaction behaviour.

This project answers one focused question:

"What behavioural signs does a borrower show in their bank transactions before missing a loan installment — and can those signs flag them a month in advance?"

A logistic regression model is trained to score every active loan account monthly, outputting a default risk probability. Accounts crossing a defined threshold are flagged for proactive intervention — restructuring offers, relationship manager outreach, or credit limit adjustment — before the first payment is ever missed.

Stakeholder: Credit Risk and Collections teams at retail banks and NBFCs.

Success metric: A model that flags at least 70% of eventual defaulters one month in advance, at a precision high enough that intervention resources are not wasted on low-risk accounts.

Dataset

Berka Dataset (PKDD'99 Financial Dataset) The only publicly available dataset containing real, anonymised transaction history and loan records for the same bank customers — sourced from a Czech bank covering 1993–1998.

Table	Rows	Contents
trans.asc	1,056,320	Every bank transaction per account
loan.asc	682	Loan records with repayment status
order.asc	6,471	Standing payment orders (fixed monthly obligations)
account.asc	4,500	Account characteristics and tenure
client.asc	5,369	Client demographics
district.asc	77	Regional socioeconomic indicators

Target variable (loan.status):

B — Loan running, client in default → Label 1
D — Loan finished, ended in default → Label 1
A — Loan running, no problems → Label 0
C — Loan finished, paid off → Label 0
