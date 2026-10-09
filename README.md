# A04 — Association Rule Mining on Online Retail

Educational association-rule mining project for **HSE LLM4Rec, Week 4 (A04)**.

**Live demo:** https://ktmasteratwork.github.io/recsys_a04/  
(available after GitHub Pages is enabled for the `main` branch / repository root)

## What this project does

This browser application mines frequent itemsets and association rules from the cleaned **UCI Online Retail** transaction baskets supplied with the course starter.

The implementation completes the seven missing homework functions:

- basket deduplication by `StockCode`;
- arbitrary itemset counting;
- support, confidence, and lift;
- complete level-wise Apriori mining with downward-closure pruning;
- generation of every directed non-empty proper-subset rule split.

The implementation uses vanilla JavaScript and no association-mining library.

## Dataset

The supplied cleaned dataset contains:

- **17,080 baskets**;
- **3,653 distinct StockCodes**;
- basket identity: `InvoiceNo`;
- item identity: `StockCode`;
- `Description` is used as the readable label;
- duplicate products inside one basket count once;
- `Quantity` is not used as a weight.

## Final threshold experiment

| Run | Min support | Min confidence | Frequent itemsets | Rules |
|---|---:|---:|---:|---:|
| A | 3% | 30% | 115 | 12 |
| FINAL | 1% | 30% | 1,219 | 950 |
| B | 1% | 60% | 1,219 | 238 |

The final working configuration is **1% minimum support / 30% minimum confidence**. It was selected before inspecting the real rule outputs and is not presented as globally optimal.

## Example rules

### Rule worth investigating

`85099B — JUMBO BAG RED RETROSPOT → 22386 — JUMBO BAG PINK POLKADOT`

- joint baskets: **546**
- antecedent baskets: **1,584**
- consequent baskets: **869**
- support: **≈ 3.20%**
- confidence: **≈ 34.47%**
- lift: **≈ 6.77**

This is an association worth testing as a cross-sell hypothesis, not proof of incremental sales.

### Rule rejected for immediate business action

`21733 — RED HANGING HEART T-LIGHT HOLDER → 85123A — WHITE HANGING HEART T-LIGHT HOLDER`

- support: **≈ 2.68%**
- confidence: **≈ 67.50%**
- lift: **≈ 5.89**

The rule is statistically strong, but the products are close colour variants of the same item type. It is therefore not treated as an automatically actionable cross-sell rule without causal/business validation.

## Validation

The implementation was checked beyond the supplied UI tests.

- bundled UI tests: **11 PASS / 0 FAIL / 0 PENDING**;
- independent exact-oracle validation: **320 generated datasets**;
- randomized comparisons: **640 cases**;
- oracle mismatches: **0**;
- negative controls rejected: **16/16**;
- 8-item completeness fixture: **255 frequent itemsets**, **6,050 directed rules**, **254 rule splits** for the full 8-item set.

The student also manually verified key calculations and behavior, including a real U1 support/confidence/lift calculation, reverse-rule confidence, threshold reruns, selected rules, and the final validation run.

## Implementation notes

Apriori is deterministic and complete within the assignment contract:

- prefix joins;
- downward-closure pruning before counting;
- inverted-index posting intersections;
- no sampling;
- no hidden itemset-cardinality cap;
- raw inclusive support/confidence thresholds;
- every `2^k - 2` directed split for a frequent `k`-itemset before confidence filtering;
- lift is reported/used downstream rather than hidden inside rule generation.

## Known supplied-scaffolding limitations

The assignment starter contains several UI/test-harness limitations that were documented separately from Apriori correctness. They were not silently rewritten outside the requested homework scope, including stale selected-rule details after some reruns, narrow-screen overflow, weak assertions in two bundled tests, and over-absolute copy about reverse confidence.

## Repository structure

- `index.html` — application UI;
- `style.css` — supplied styling;
- `transactions.js` — supplied cleaned Online Retail baskets;
- `script.js` — completed A04 implementation;
- `ASSIGNMENT_README.md` — original Week 4 assignment specification from the audited starter commit;
- `VALIDATION.md` — concise validation and experiment summary.

## Run locally

No build step or server is required.

1. Download/clone the repository.
2. Open `index.html` in a browser.
3. Use the support/confidence controls and press **Run rules**.
4. Use the built-in test button to run the supplied UI checks.

## Development process

The project was developed with **OpenCode using GPT-6 Astra High**, with staged implementation, independent automated validation, and separate manual student verification. The native OpenCode session export is kept as the course audit artifact and is not published in this public repository.

## Sources

- UCI Machine Learning Repository — Online Retail: https://archive.ics.uci.edu/dataset/352/online+retail
- D. Chen, S. L. Sain, and K. Guo (2012), DOI: https://doi.org/10.1057/dbm.2012.17
- R. Agrawal and R. Srikant (1994), *Fast Algorithms for Mining Association Rules in Large Databases*: https://www.vldb.org/conf/1994/P487.PDF
- J. Han, J. Pei, and Y. Yin (2000), DOI: https://doi.org/10.1145/342009.335372

## Starter provenance

Course starter audited at commit:
`c00cfa1977dbeeb2155d3b70280cb9b382a24865`

https://github.com/dryjins/RecSys-LLMs/tree/c00cfa1977dbeeb2155d3b70280cb9b382a24865/week4
