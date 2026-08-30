# Praia Tech Solutions — RevOps Dashboard

A Revenue Operations portfolio project: a mock B2B sales dataset (leads → opportunities → closed revenue) modeled as a working CRM, analyzed with a KPI dashboard, and interpreted through an executive RevOps lens.

**Goal:** demonstrate the full RevOps reasoning chain — `CRM data → metrics → analysis → insight → business decision` — not just spreadsheet formatting.

## Repo structure

```
praia-tech-revops/
├── README.md
├── praia_tech_revops_dashboard.xlsx   # full workbook: 5 tabs, live formulas, dashboard
└── data/
    ├── opportunities.csv              # 40 deals: source, stage, value, probability, dates, owner
    ├── leads.csv                      # 45 top-of-funnel leads: source, status, conversion, owner
    ├── reps.csv                       # 3-person sales team: role, monthly target, region
    └── targets.csv                    # management targets: revenue, coverage, win rate, conversion
```

The `.xlsx` is the primary artifact — every dashboard number is a live formula (`SUMIFS`, `COUNTIFS`, `IFERROR`), not a hardcoded value, so it recalculates if the underlying data changes. The CSVs in `/data` are the same records in flat form, for anyone who wants to re-analyze the raw data independently.

## Dashboard snapshot

| Metric | Value | Target | Status |
|---|---|---|---|
| Total Opportunities | 40 | — | — |
| Total Pipeline (open) | $373,000 | $300,000 | ✅ On Track |
| Weighted Pipeline | $211,850 | $100,000 | ✅ On Track |
| Closed Won Revenue | $52,000 | $100,000 | ⚠️ At Risk |
| Closed Lost Revenue | $17,500 | — | — |
| Win Rate | 66.7% | 25% | ✅ On Track |
| Average Deal Size | $11,062.50 | $10,000 | ✅ On Track |
| Lead → Opportunity Conversion | 71.1% | 60% | ✅ On Track |
| Pipeline Coverage | 3.73x | 3.0x | ✅ On Track |

**By sales rep**

| Rep | Pipeline | Weighted Pipeline | Closed Won | Opportunities | Win Rate |
|---|---|---|---|---|---|
| Carlos | $148,500 | $87,750 | $19,000 | 13 | 100% |
| Ana | $114,000 | $67,050 | $0 | 13 | 0% |
| Sidney | $110,500 | $57,050 | $33,000 | 14 | 100% |

**By funnel stage**

| Stage | Opportunities | Pipeline | Weighted Pipeline |
|---|---|---|---|
| Discovery | 7 | $42,500 | $8,500 |
| Qualified | 8 | $81,500 | $24,450 |
| Proposal | 8 | $101,500 | $60,900 |
| Negotiation | 8 | $147,500 | $118,000 |
| Closed Won | 6 | $52,000 | — |
| Closed Lost | 3 | $17,500 | — |

**By acquisition channel**

| Source | Leads | Opportunities | Conversion | Pipeline Generated |
|---|---|---|---|---|
| Referral | 12 | 11 | 91.7% | $124,500 |
| Cold Outreach | 9 | 8 | 88.9% | $114,000 |
| LinkedIn | 12 | 11 | 91.7% | $99,000 |
| Website | 12 | 10 | 83.3% | $105,000 |

## Executive analysis

**1. Is there enough pipeline to hit the $100K revenue target?**
Yes. $373K in open pipeline covers the $300K coverage target (3.73x vs. the 3.0x required).

**2. Does the weighted forecast support hitting the target?**
The weighted pipeline ($211,850) is more than double the monthly target — a strong signal. But only $52,000 (52% of target) is closed won so far, and several close dates fall past the current period. The volume is there; **timing of closure** is the actual risk, not pipeline size.

**3. Where is the funnel bottleneck?**
Discovery + Qualified together hold 15 open opportunities (~40% of the funnel) but only $32,950 in weighted pipeline (~15% of the total). A large share of deals sit early in the funnel with low win probability — the constraint is moving deals from Discovery/Qualified into Proposal, not generating more top-of-funnel volume.

**4. How is the sales team performing?**
Carlos carries the largest pipeline ($148,500) and weighted pipeline ($87,750). Sidney and Carlos both show a 100% win rate. The one number that actually needs a conversation: **Ana has $0 in closed-won revenue, and every lost deal in the dataset (3 of 3) belongs to her** — that's the most actionable line in the whole table, not a footnote.

**5. Which acquisition channel is most efficient?**
Referral — highest conversion rate (91.7%) tied with LinkedIn, but generates the most pipeline value ($124,500) on the same lead volume. Cold Outreach converts almost as well (88.9%) on fewer leads (9) and is worth testing at higher volume before shifting more budget to other channels.

## Tech notes

- Built with Python (`openpyxl`) and validated with a LibreOffice headless recalculation pass — zero formula errors across 154 formulas.
- All dashboard KPIs reference the raw data tabs directly; nothing in the `Dashboard` tab is a typed-in number.
- One data-integrity fix made during build: a target-reference formula for Average Deal Size originally pointed at the wrong row in the Targets tab (comparing a currency value against a percentage target). Documented as a cell comment in the workbook.

## Author

Sidney (Samurai Sidney Evangelista Antunes Praia) — CRM Consultant building toward Revenue Operations, based in Angola, relocating to Portugal in 2027.
