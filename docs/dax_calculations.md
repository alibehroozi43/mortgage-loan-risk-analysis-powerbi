# DAX Calculations

This document contains the calculated tables, calculated columns, and measures used in the Mortgage Loan Risk Analysis Power BI dashboard.

# Date Table

## DimDate

The date table dynamically covers the minimum and maximum application dates.

```DAX
DimDate =
VAR MinDate =
    MIN ( FactApplications[ApplicationDate] )

VAR MaxDate =
    MAX ( FactApplications[ApplicationDate] )

RETURN
ADDCOLUMNS (
    CALENDAR ( MinDate, MaxDate ),
    "Year", YEAR ( [Date] ),
    "Quarter", "Q" & QUARTER ( [Date] ),
    "Month", FORMAT ( [Date], "MMMM" ),
    "MonthNumber", MONTH ( [Date] ),
    "Year-Month", FORMAT ( [Date], "YYYY-MM" )
)
```

After creating the table:

```text
Sort Month by MonthNumber.
Mark DimDate as the date table.
Create a relationship with FactApplications[ApplicationDate].
```

# Calculated Columns

## 1. Loan-to-Value Ratio

Table:

```text
FactApplications
```

Calculated column:

```DAX
LTV =
DIVIDE (
    FactApplications[LoanAmount],
    RELATED ( DimProperties[AppraisedValue] )
)
```

### Description

Loan-to-Value, or LTV, compares the requested loan amount with the appraised property value.

```text
LTV = Loan Amount / Appraised Property Value
```

The column should be formatted as a percentage.

## 2. LTV Band

Table:

```text
FactApplications
```

Calculated column:

```DAX
LTV Band =
SWITCH (
    TRUE(),
    FactApplications[LTV] <= 0.8, "Low",
    FactApplications[LTV] <= 1.0, "Medium",
    "High"
)
```

### Classification

| LTV Band | Rule |
|---|---|
| Low | LTV less than or equal to 80% |
| Medium | LTV greater than 80% and less than or equal to 100% |
| High | LTV greater than 100% |

## 3. Credit Band

Table:

```text
DimApplicants
```

Calculated column:

```DAX
Credit Band =
SWITCH (
    TRUE(),
    DimApplicants[CreditScore] < 600, "Poor",
    DimApplicants[CreditScore] < 650, "Fair",
    DimApplicants[CreditScore] < 700, "Good",
    DimApplicants[CreditScore] < 750, "Very Good",
    "Excellent"
)
```

### Classification

| Credit Band | Credit-score range |
|---|---|
| Poor | Below 600 |
| Fair | 600 to 649 |
| Good | 650 to 699 |
| Very Good | 700 to 749 |
| Excellent | 750 or above |

## 4. Risk Level

Table:

```text
FactApplications
```

Calculated column:

```DAX
Risk Level =
VAR CS =
    RELATED ( DimApplicants[CreditScore] )

VAR BK =
    RELATED ( DimApplicants[Bankruptcy] )

VAR LP =
    RELATED ( DimApplicants[LatePayments] )

VAR LTVVal =
    FactApplications[LTV]

VAR IsHigh =
    CS < 600
        || BK = 1
        || LP >= 4
        || LTVVal > 1.0

VAR IsMedium =
    CS < 700
        || LP >= 2
        || LTVVal > 0.8

RETURN
SWITCH (
    TRUE(),
    IsHigh, "High Risk",
    IsMedium, "Medium Risk",
    "Low Risk"
)
```

### High-Risk Conditions

An application is classified as High Risk when at least one of the following conditions is true:

```text
Credit Score below 600
Bankruptcy indicator equals 1
Late Payments greater than or equal to 4
LTV greater than 100%
```

### Medium-Risk Conditions

An application is classified as Medium Risk when it is not High Risk and at least one of the following is true:

```text
Credit Score below 700
Late Payments greater than or equal to 2
LTV greater than 80%
```

The order of conditions is important because High Risk must be evaluated before Medium Risk.

# Measures Table

A dedicated measure table can be created using:

```DAX
Measures = {}
```

All dashboard measures can then be stored in this table.

# Core Measures

## 1. Total Applications

```DAX
Total Applications =
COUNTROWS ( FactApplications )
```

Counts all mortgage applications.

## 2. Unique Applicants

```DAX
Unique Applicants =
DISTINCTCOUNT ( FactApplications[ApplicantID] )
```

Counts the number of distinct applicants represented in the application table.

## 3. Total Requested Amount

```DAX
Total Requested Amount =
SUM ( FactApplications[LoanAmount] )
```

Calculates the total value of all requested mortgage loans.

## 4. Average Loan Amount

```DAX
Average Loan Amount =
AVERAGE ( FactApplications[LoanAmount] )
```

Calculates the average requested mortgage amount.

# Application Status Measures

## 5. Approved Applications

```DAX
Approved Applications =
CALCULATE (
    [Total Applications],
    FactApplications[Status] = "Approved"
)
```

## 6. Rejected Applications

```DAX
Rejected Applications =
CALCULATE (
    [Total Applications],
    FactApplications[Status] = "Rejected"
)
```

## 7. Pending Applications

```DAX
Pending Applications =
CALCULATE (
    [Total Applications],
    FactApplications[Status] = "Pending"
)
```

## 8. Decided Applications

```DAX
Decided Applications =
[Approved Applications]
    + [Rejected Applications]
```

Counts applications that have received a final decision.

Pending applications are excluded.

# Status Rate Measures

## 9. Approval Rate

```DAX
Approval Rate =
DIVIDE (
    [Approved Applications],
    [Decided Applications]
)
```

The denominator contains only applications with a final decision.

## 10. Rejection Rate

```DAX
Rejection Rate =
DIVIDE (
    [Rejected Applications],
    [Decided Applications]
)
```

## 11. Pending Rate

```DAX
Pending Rate =
DIVIDE (
    [Pending Applications],
    [Total Applications]
)
```

# Loan Amount Measures

## 12. Approved Loan Amount

```DAX
Approved Loan Amount =
CALCULATE (
    [Total Requested Amount],
    FactApplications[Status] = "Approved"
)
```

## 13. Rejected Loan Amount

```DAX
Rejected Loan Amount =
CALCULATE (
    [Total Requested Amount],
    FactApplications[Status] = "Rejected"
)
```

# LTV and Risk Measures

## 14. Average LTV

```DAX
Average LTV =
AVERAGE ( FactApplications[LTV] )
```

## 15. High Risk Applications

```DAX
High Risk Applications =
CALCULATE (
    [Total Applications],
    FactApplications[Risk Level] = "High Risk"
)
```

## 16. High Risk Rate

```DAX
High Risk Rate =
DIVIDE (
    [High Risk Applications],
    [Total Applications]
)
```

## 17. Applications per Applicant

```DAX
Applications per Applicant =
DIVIDE (
    [Total Applications],
    [Unique Applicants]
)
```

## 18. Average Credit Score

```DAX
Average Credit Score =
AVERAGEX (
    FactApplications,
    RELATED ( DimApplicants[CreditScore] )
)
```

This measure evaluates the credit score associated with each application.

An applicant with multiple applications therefore contributes multiple application-level observations.

## 19. Applications Above 100% LTV

```DAX
Applications Above 100% LTV =
CALCULATE (
    [Total Applications],
    FactApplications[LTV] > 1.0
)
```

## 20. Above 100% LTV Rate

```DAX
Above 100% LTV Rate =
DIVIDE (
    [Applications Above 100% LTV],
    [Total Applications]
)
```

# Recommended Formatting

## Percentage Measures

Format the following measures as percentages with one decimal place:

```text
Approval Rate
Rejection Rate
Pending Rate
Average LTV
High Risk Rate
Above 100% LTV Rate
```

## Currency Measures

Format the following measures as currency with a thousands separator:

```text
Total Requested Amount
Average Loan Amount
Approved Loan Amount
Rejected Loan Amount
```

## Whole-Number Measures

Format the following measures as whole numbers:

```text
Total Applications
Unique Applicants
Approved Applications
Rejected Applications
Pending Applications
Decided Applications
High Risk Applications
Applications Above 100% LTV
```

# Measure Usage by Report Page

## Executive Overview

Recommended measures:

```text
Total Applications
Total Requested Amount
Approval Rate
Pending Applications
Average Loan Amount
Average LTV
```

## Risk Analysis

Recommended measures:

```text
High Risk Applications
High Risk Rate
Applications Above 100% LTV
Above 100% LTV Rate
Average Credit Score
Average LTV
```

## Underwriter and Product Performance

Recommended measures:

```text
Total Applications
Approved Applications
Rejected Applications
Pending Applications
Approval Rate
Total Requested Amount
Average Loan Amount
Average LTV
High Risk Applications
```

# Important Interpretation Notes

## Approval Rate

The Approval Rate is calculated using:

```text
Approved Applications / Decided Applications
```

Pending applications are not included in the denominator.

This prevents unresolved cases from artificially reducing the approval rate.

## Average Credit Score

The Average Credit Score measure is calculated at the application level.

Applicants with more than one application contribute their credit score more than once.

## Risk Level

Risk Level is a rule-based analytical classification created for this project.

It should be interpreted as a dashboard segmentation rule rather than a production credit-scoring model.

## LTV Above 100%

An LTV above 100% means the requested loan is greater than the appraised property value.

This condition is used as one of the High Risk indicators in the dashboard.
