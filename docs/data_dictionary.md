# Data Dictionary

This document describes the datasets and fields used in the Mortgage Loan Risk Analysis Power BI project.

## Data Sources

The project uses six Excel files:

| Source file | Power BI table | Records | Purpose |
|---|---|---:|---|
| Applications.xlsx | FactApplications | 100 | Mortgage loan applications |
| Applicants.xlsx | DimApplicants | 50 | Applicant information |
| CreditHistory.xlsx | CreditHistory | 50 | Applicant credit history |
| LoanProducts.xlsx | DimProducts | 3 | Mortgage product information |
| Properties.xlsx | DimProperties | 50 | Property information |
| Underwriters.xlsx | DimUnderwriters | 5 | Underwriter information |

---

# FactApplications

The central fact table contains one record for each mortgage application.

| Column | Data type | Description |
|---|---|---|
| ApplicationID | Whole Number | Unique identifier for each application |
| ApplicantID | Whole Number | Foreign key connected to DimApplicants |
| PropertyID | Whole Number | Foreign key connected to DimProperties |
| ProductID | Whole Number | Foreign key connected to DimProducts |
| UnderwriterID | Whole Number | Foreign key connected to DimUnderwriters |
| ApplicationDate | Date | Date on which the application was submitted |
| LoanAmount | Decimal Number | Requested mortgage loan amount |
| Status | Text | Application status: Approved, Pending, or Rejected |

## Primary Key

```text
ApplicationID
```

## Foreign Keys

```text
ApplicantID
PropertyID
ProductID
UnderwriterID
ApplicationDate
```

---

# DimApplicants

This dimension contains applicant identity and credit-related information.

| Column | Data type | Description |
|---|---|---|
| ApplicantID | Whole Number | Unique identifier for each applicant |
| FirstName | Text | Applicant first name |
| LastName | Text | Applicant last name |
| Email | Text | Applicant email address |
| DateOfBirth | Date | Applicant date of birth |
| FullName | Text | Combined first and last name |
| CreditScore | Whole Number | Applicant credit score |
| Bankruptcy | Whole Number | Indicates bankruptcy history using 0 or 1 |
| LatePayments | Whole Number | Number of recorded late payments |
| Credit Band | Text | Credit-score classification |

## Primary Key

```text
ApplicantID
```

## Data Quality Note

`ApplicantID` is the unique identifier for applicants.

The `Email` field must not be used as a primary or unique key because 13 of the 50 applicant records contain duplicated email values.

---

# CreditHistory

This source table contains applicant credit history.

| Column | Data type | Description |
|---|---|---|
| CreditID | Whole Number | Credit-history record identifier |
| ApplicantID | Whole Number | Applicant identifier |
| CreditScore | Whole Number | Applicant credit score |
| Bankruptcy | Whole Number | Bankruptcy indicator using 0 or 1 |
| LatePayments | Whole Number | Number of late payments |

This table is merged into `DimApplicants` in Power Query.

After the merge, loading the original `CreditHistory` query into the data model is disabled.

---

# DimProducts

This dimension contains mortgage product information.

| Column | Data type | Description |
|---|---|---|
| ProductID | Whole Number | Unique product identifier |
| ProductName | Text | Mortgage product name |
| InterestRate | Decimal Number / Percentage | Product interest rate |
| TermMonths | Whole Number | Mortgage term in months |

## Available Products

```text
5/1 ARM
Fixed 15yr
Fixed 30yr
```

## Primary Key

```text
ProductID
```

---

# DimProperties

This dimension contains property information.

| Column | Data type | Description |
|---|---|---|
| PropertyID | Whole Number | Unique property identifier |
| Address | Text | Property address |
| City | Text | Property city |
| State | Text | Property state |
| ZipCode | Text | Property postal code |
| AppraisedValue | Decimal Number | Appraised property value |

## Primary Key

```text
PropertyID
```

## Data Type Note

`ZipCode` is stored as text because postal codes are identifiers and should not be aggregated mathematically.

---

# DimUnderwriters

This dimension contains underwriter information.

| Column | Data type | Description |
|---|---|---|
| UnderwriterID | Whole Number | Unique underwriter identifier |
| Name | Text | Underwriter name |
| Department | Text | Underwriter department |

## Primary Key

```text
UnderwriterID
```

---

# DimDate

The date dimension is created in Power BI using DAX.

| Column | Description |
|---|---|
| Date | Continuous calendar date |
| Year | Application year |
| Quarter | Calendar quarter |
| Month | Month name |
| MonthNumber | Numeric month used for sorting |
| Year-Month | Year and month label used in trend charts |

The date table covers the range of application dates and is connected to:

```text
FactApplications[ApplicationDate]
```

---

# Data Quality Summary

| Check | Result |
|---|---:|
| Applicant records | 50 |
| Application records | 100 |
| Property records | 50 |
| Product records | 3 |
| Underwriter records | 5 |
| Null values in primary keys | 0 |
| Duplicate dimension keys | 0 |
| Applicants with duplicate email values | 13 |

## Important Data Quality Finding

Although applicant identifiers are unique, the Email column contains duplicated values.

Therefore:

```text
ApplicantID must be used as the applicant key.
Email must not be treated as a unique identifier.
```
