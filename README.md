# RCM Denial Management Analytics

An end-to-end revenue cycle analytics project: synthetic US healthcare claims data generated in Python, metrics computed with pandas, rendered as a live interactive dashboard — built around the questions an RCM team actually asks: where are we losing money, why, and what should we fix first?

**Live dashboard:** https://rohith964020.github.io/rcm-denial-analytics/

> All data is fully synthetic. Real claims data is protected health information (PHI) under HIPAA, so the project generates 8,000 claims with payer behavior, denial-category frequencies, and recovery probabilities modeled on industry benchmarks.

---

## What the dashboard answers

| Question | Where |
|---|---|
| How healthy is the revenue cycle? | KPI row: denial rate, Days in AR, open AR, 90+ share, recovery rate, clean claim rate |
| Which payers are the problem? | Denial rate + payment speed by payer |
| Why are claims being denied? | Denials by root cause, with recovery rate per category |
| Is anything changing over time? | Monthly prior-authorization denial trend |
| Where is the money stuck? | Open AR by aging bucket |

## Key findings

1. **One payer denies at 20.7% — nearly double the 14.4% book average — and pays a month slower than the best payer.** Recommendation: payer-relations escalation plus payer-specific claim-scrubbing rules, not more AR calling.
2. **Prior-authorization denials spiked ~60% in Q4**, consistent with a payer policy change. Recommendation: front-end prior-auth checklist update for the affected CPT codes. Auth denials recover only ~20% of the time, so prevention is worth ~5x cure.
3. **82% of open AR ($504K of $616K) sits past 90 days**, concentrated in high-value claims. Recommendation: work the AR queue ranked by dollar value × age, not first-in-first-out.

## How it works

```
scripts/generate_rcm_data.py   →  data/rcm_claims_data.csv   (8,000 synthetic claims)
scripts/analyze_rcm_data.py    →  dashboard_data.json        (all computed metrics)
index.html                     →  renders the dashboard (Chart.js) from the JSON
```

**Domain modeling baked into the data:** 6 payers with distinct denial rates (8–19%) and payment lags · 15 common CPT codes, high-cost procedures flagged for prior authorization · 7 denial categories weighted by real-world frequency, each with its own recovery probability encoding the hard vs. soft denial distinction (missing info ~75% recovery; timely filing ~5%) · contractual adjustments via billed vs. allowed amounts · write-offs excluded from AR aging, since write-offs leave AR by definition.

## Stack

Python (pandas, numpy) · Chart.js · vanilla HTML/JS · hosted on GitHub Pages

## RCM concepts demonstrated

Full revenue cycle flow (registration → eligibility → coding → EDI 837 → adjudication → EDI 835 → posting → AR → denial management) · hard vs. soft denials · denial root-cause categorization · Days in AR, denial rate, clean claim rate · AR aging · HIPAA/PHI awareness (hence synthetic data)
