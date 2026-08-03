# Power Query Data Preparation

This document describes the main data preparation and transformation steps applied in Power Query.

## Query Naming

The imported Excel files were renamed as follows:

| Original file | Power BI query |
|---|---|
| Applications.xlsx | FactApplications |
| Applicants.xlsx | DimApplicants |
| CreditHistory.xlsx | CreditHistory |
| LoanProducts.xlsx | DimProducts |
| Properties.xlsx | DimProperties |
| Underwriters.xlsx | DimUnderwriters |

---

# Data Type Configuration

## FactApplications

```text
ApplicationID      Whole Number
ApplicantID        Whole Number
PropertyID         Whole Number
ProductID          Whole Number
UnderwriterID      Whole Number
ApplicationDate    Date
LoanAmount         Decimal Number
Status             Text
```

## DimApplicants

```text
ApplicantID        Whole Number
FirstName          Text
LastName           Text
Email              Text
DateOfBirth        Date
```

## CreditHistory

```text
CreditID           Whole Number
ApplicantID        Whole Number
CreditScore        Whole Number
Bankruptcy         Whole Number
LatePayments       Whole Number
```

## DimProducts

```text
ProductID          Whole Number
ProductName        Text
InterestRate       Decimal Number
TermMonths         Whole Number
```

The `InterestRate` field is formatted as a percentage after loading.

## DimProperties

```text
PropertyID         Whole Number
Address            Text
City               Text
State              Text
ZipCode            Text
AppraisedValue     Decimal Number
```

## DimUnderwriters

```text
UnderwriterID      Whole Number
Name               Text
Department         Text
```

---

# Text Cleaning

Leading and trailing spaces and non-printable characters were removed from text fields.

The following Power Query M transformation was used for applicant names and email addresses:

```powerquery
= Table.TransformColumns(
    #"Previous Step",
    {
        {
            "FirstName",
            each Text.Trim(Text.Clean(_)),
            type text
        },
        {
            "LastName",
            each Text.Trim(Text.Clean(_)),
            type text
        },
        {
            "Email",
            each Text.Trim(Text.Clean(Text.Lower(_))),
            type text
        }
    }
)
```

Similar cleaning was applied to:

```text
DimProperties[City]
DimProperties[State]
DimUnderwriters[Name]
```

---

# Full Name Column

A custom column was created in `DimApplicants`:

```powerquery
[FirstName] & " " & [LastName]
```

The new column was named:

```text
FullName
```

---

# Credit History Merge

The `CreditHistory` query was merged into `DimApplicants` using `ApplicantID`.

## Join Configuration

```text
Left table:
DimApplicants

Right table:
CreditHistory

Join key:
ApplicantID

Join type:
Left Outer
```

Equivalent M code:

```powerquery
= Table.NestedJoin(
    DimApplicants,
    {"ApplicantID"},
    CreditHistory,
    {"ApplicantID"},
    "CreditHistory",
    JoinKind.LeftOuter
)
```

The following fields were expanded from the merged table:

```text
CreditScore
Bankruptcy
LatePayments
```

After the merge, `Enable Load` was disabled for the original `CreditHistory` query to prevent it from appearing as an unnecessary independent model table.

---

# Key Validation

The following dimension keys were checked for null and duplicate values:

```text
DimApplicants[ApplicantID]
DimProperties[PropertyID]
DimProducts[ProductID]
DimUnderwriters[UnderwriterID]
CreditHistory[ApplicantID]
```

The checks confirmed:

```text
No null primary-key values
No duplicate dimension keys
```

However, the `Email` column contained 13 duplicated values and was therefore not suitable as a unique key.

---

# Record Count Validation

The record counts remained unchanged after text cleaning because Trim and Clean transformations do not remove records.

| Query | Records |
|---|---:|
| DimApplicants | 50 |
| FactApplications | 100 |
| DimProperties | 50 |
| DimProducts | 3 |
| DimUnderwriters | 5 |

---

# Data Preparation Outcome

The Power Query process produced:

- Clean and standardized text fields
- Correct data types
- Valid dimension keys
- A merged applicant and credit-history dimension
- A clean fact table
- Tables ready for star-schema modeling
