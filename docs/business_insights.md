# Business Insights

This document summarizes the main analytical findings and management recommendations derived from the Mortgage Loan Risk Analysis Power BI dashboard.

## Executive Summary

The analysis covers:

- 100 mortgage applications
- 50 applicants
- 50 properties
- 3 loan products
- 5 underwriters
- Application activity between 2022 and 2023

The most important portfolio risk is the high share of applications with a Loan-to-Value ratio above 100%.

A total of 48 applications have an LTV above 100%, and 24 of them are still pending.

This means that the largest unresolved group in the portfolio also carries a major risk indicator.

---

# 1. Application Growth

## Applications by Year

| Year | Applications |
|---|---:|
| 2022 | 42 |
| 2023 | 58 |

Application volume increased from 42 in 2022 to 58 in 2023.

This represents approximately:

```text
38% growth
```

## Interpretation

The increase suggests growing demand for mortgage products.

However, a larger application volume may also increase:

- Underwriter workload
- Pending-case volume
- Processing time
- Operational risk
- Need for standardized decision criteria

---

# 2. Requested Loan Amount Growth

## Total Requested Amount by Year

| Year | Total requested amount |
|---|---:|
| 2022 | $16,621,984 |
| 2023 | $23,883,123 |

The total requested amount increased by approximately:

```text
44%
```

## Interpretation

The increase in requested loan value was larger than the increase in application count.

This may indicate:

- Higher average loan amounts
- More expensive properties
- A change in product mix
- Greater financial exposure
- Higher portfolio risk

---

# 3. Product Approval Performance

## Approval Rate by Product

| Product | Approximate approval rate |
|---|---:|
| 5/1 ARM | 70% |
| Fixed 30yr | 59% |
| Fixed 15yr | 57% |

The `5/1 ARM` product achieved the highest approval rate.

The `Fixed 15yr` product had the lowest approval rate among the three products.

## Interpretation

The difference in approval rates may reflect:

- Different applicant profiles
- Different qualification requirements
- Different loan amounts
- Different risk characteristics
- Different product-review criteria

The product-level results should be interpreted together with application volume and requested amount.

---

# 4. Underwriter Approval Performance

## Highest Approval Rate

`Underwriter 1` achieved an approval rate of approximately:

```text
91%
```

This underwriter reviewed:

```text
18 applications
```

Of these applications:

```text
11 received a final decision
```

## Interpretation

The approval rate is significantly higher than the rates of some other underwriters.

This difference should be reviewed carefully.

A high approval rate may reflect:

- A stronger applicant portfolio
- Different product assignments
- Different regional assignments
- Different risk exposure
- Inconsistent decision criteria

The dashboard does not prove the cause of the variation, but it identifies the need for further review.

---

# 5. Geographic Loan Exposure

## State with the Highest Requested Amount

Texas recorded the highest total requested loan amount:

```text
Approximately $13,310,025
```

Texas was substantially ahead of Florida, which ranked second.

## Interpretation

The high concentration of requested loan value in Texas creates geographic exposure.

Management should monitor:

- Approval rate in Texas
- Average LTV in Texas
- Pending applications in Texas
- High-risk applications in Texas
- Product distribution in Texas

---

# 6. Applications Above 100% LTV

A total of:

```text
48 out of 100 applications
```

have a Loan-to-Value ratio above 100%.

This represents:

```text
48%
```

of the portfolio.

## Status of Above-100% LTV Applications

| Status | Applications |
|---|---:|
| Pending | 24 |
| Approved | 15 |
| Rejected | 9 |
| Total | 48 |

## Interpretation

Half of the applications with LTV above 100% are still pending.

The large pending group suggests that the portfolio contains a significant unresolved risk concentration.

The results do not show that every high-LTV application is rejected.

Instead, they show that a large share remains undecided.

## Main Portfolio Risk

```text
48% of applications have an LTV above 100%, and 24 of these applications are still pending.
```

This is the most important risk insight identified by the dashboard.

---

# 7. Credit Score and Approval Rate

The results do not show a fully linear relationship between Credit Score and Approval Rate.

For example:

- The `Very Good` Credit Band has an approval rate of approximately 80%.
- The `Poor` Credit Band still has an approval rate of approximately 64%.

## Interpretation

Higher Credit Scores generally appear to support better approval outcomes, but the relationship is not perfectly consistent.

Possible reasons include:

- Small sample size
- Loan-to-Value differences
- Bankruptcy history
- Late-payment history
- Product differences
- Underwriter differences
- Pending applications

The analysis is based on 100 applications, so group-level percentages may fluctuate substantially.

---

# 8. Bankruptcy and Application Status

For applicants with a bankruptcy history:

| Status | Share |
|---|---:|
| Approved | 36% |
| Pending | 40% |
| Rejected | 24% |

The approval share for applicants without a bankruptcy history is approximately:

```text
42%
```

## Interpretation

Applicants with bankruptcy history have a slightly lower approval share.

However, the difference is not large enough to conclude that bankruptcy alone determines the application result.

Other variables, including LTV, Credit Score, Late Payments, product, and underwriter, may also influence the result.

---

# 9. Applicant with the Most Applications

The applicant with:

```text
ApplicantID = 19
```

submitted:

```text
7 applications
```

This is the highest number of applications submitted by any applicant in the dataset.

## Interpretation

Applicants with multiple applications should be reviewed to understand whether they represent:

- Multiple properties
- Repeat financing attempts
- Product switching
- Rejected and resubmitted applications
- Possible duplicate business processes

---

# 10. Product with the Highest Requested Amount

The `Fixed 15yr` product generated the highest total requested loan amount:

```text
Approximately $14,931,556
```

This amount was slightly higher than the total for the `5/1 ARM` product.

## Interpretation

The `Fixed 15yr` product is financially important because it combines:

- High application volume
- High requested amount
- The lowest approval rate among the three products

This combination makes it a priority area for further investigation.

---

# 11. High-Volume Product with Lower Approval

The `Fixed 15yr` product received:

```text
36 applications
```

It also had the lowest approval rate:

```text
Approximately 57%
```

## Interpretation

The product attracts substantial demand but converts fewer decided applications into approvals.

Possible areas for review include:

- Eligibility criteria
- Interest-rate competitiveness
- Applicant credit quality
- Average LTV
- Property values
- Underwriter assignment
- Documentation requirements

---

# 12. Underwriter Risk and Approval Rate

The dashboard suggests an association between the number of High Risk applications and lower underwriter approval rates.

| Underwriter | High-risk applications | Approximate approval rate |
|---|---:|---:|
| Underwriter 1 | 15 | 91% |
| Underwriter 2 | 22 | 47% |
| Underwriter 3 | 18 | 42% |

## Interpretation

Underwriters 2 and 3 handled more High Risk applications and recorded lower approval rates.

However, Underwriter 1 also handled 15 High Risk applications while maintaining a very high approval rate.

This suggests that risk exposure alone may not fully explain underwriter performance differences.

Further analysis should control for:

- Product mix
- State
- LTV
- Credit Score
- Bankruptcy
- Late Payments
- Pending applications
- Application assignment rules

---

# 13. Credit Band with the Most Rejections

The `Poor` Credit Band recorded:

```text
9 rejected applications
```

This was the highest rejection count among the Credit Bands.

## Interpretation

This result is directionally consistent with expectations because lower Credit Scores generally indicate higher credit risk.

However, the dataset should be interpreted carefully because the number of applications in each Credit Band may differ.

---

# 14. Risk Level with the Most Pending Applications

The `High Risk` category contains:

```text
33 pending applications
```

This is the largest pending group across the risk categories.

## Interpretation

A large number of High Risk applications remain unresolved.

This may indicate:

- Longer review processes
- Additional documentation requirements
- Manual escalation
- Underwriting uncertainty
- Greater operational backlog

Management should prioritize this group for review.

---

# 15. Late Payments and Application Status

The analysis does not show a strong and consistently decreasing approval rate as Late Payments increase.

Approval rates fluctuate approximately between:

```text
23% and 50%
```

across different Late Payment groups.

## Interpretation

The relationship is not strong enough to support a definite conclusion.

The likely reasons include:

- Small numbers within each group
- Interaction with Credit Score
- Interaction with Bankruptcy
- Different LTV levels
- Product differences
- Underwriter differences

More data would be required for a reliable statistical conclusion.

---

# Management Recommendations

## Recommendation 1: Prioritize Pending High-LTV Applications

There are 24 pending applications with an LTV above 100%.

These cases combine:

- Unresolved status
- High financial exposure
- A major risk indicator

Management should create a priority-review workflow for these applications.

### Suggested Actions

- Add an escalation flag
- Assign review deadlines
- Track average decision time
- Review missing documentation
- Separate applications by risk severity
- Monitor weekly resolution progress

---

## Recommendation 2: Standardize Underwriter Decision Criteria

Approval rates vary substantially across underwriters.

The observed range is approximately:

```text
42% to 91%
```

This variation may indicate different case assignments, but it may also indicate inconsistent review standards.

### Suggested Actions

- Compare application mix by underwriter
- Review underwriting guidelines
- Conduct decision-calibration sessions
- Audit a sample of approved and rejected applications
- Create common risk-review checklists
- Monitor approval rates after adjusting for risk level

---

## Recommendation 3: Review the Fixed 15yr Product

The `Fixed 15yr` product combines:

- The highest application volume
- The highest requested amount
- The lowest approval rate

### Suggested Actions

- Review customer eligibility rules
- Compare applicant profiles with other products
- Examine average LTV and Credit Score
- Review documentation requirements
- Investigate product pricing
- Consider alternative product recommendations

The `5/1 ARM` product may be considered as an alternative for some applicants because it currently has a higher approval rate.

---

# Important Limitations

The analytical results should be interpreted with the following limitations:

- The dataset contains only 100 applications.
- Some category-level percentages are based on small samples.
- The Risk Level is a rule-based dashboard classification.
- The analysis does not establish causal relationships.
- Pending applications do not yet have a final outcome.
- Underwriter performance is not adjusted for application complexity.
- Product performance is not adjusted for applicant risk.
- The dataset does not include income, debt-to-income ratio, employment, or repayment outcomes.

---

# Recommended Future Analysis

Future versions of the project could include:

- Application processing-time analysis
- Pending-case aging
- Approval-rate normalization by risk level
- Underwriter workload comparison
- Debt-to-income analysis
- Applicant income analysis
- Loan default prediction
- Portfolio stress testing
- Geographic concentration risk
- Product profitability
- Customer resubmission analysis
- Drill-through applicant profiles
- Automated risk alerts

---

# Final Conclusion

The portfolio shows strong growth in both application volume and requested loan value.

However, the most important issue is the concentration of unresolved High-LTV applications.

The dashboard indicates that:

```text
48% of all applications have an LTV above 100%.
24 of these applications are still pending.
```

Reducing this pending high-risk backlog should be the first operational priority.

At the same time, the organization should review underwriter decision consistency and investigate the performance of the high-volume `Fixed 15yr` product.
