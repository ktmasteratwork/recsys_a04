# Validation summary

This file summarizes the verification evidence for the final A04 implementation.

## Supplied tests

Final bundled UI test result:

- PASS: 11
- FAIL: 0
- PENDING: 0

The supplied harness is useful as a smoke test, but two checks have weak assertions, so it was not treated as sufficient proof by itself.

## Independent validation

An independent exact oracle compared expected and actual frequent itemsets/rules on generated datasets.

- generated datasets: 320
- randomized comparison cases: 640
- mismatches: 0
- negative controls rejected: 16/16

A synthetic 8-item completeness case produced:

- 255 frequent itemsets
- 6,050 directed rules
- 254 directed splits of the full 8-item set

## Real-data threshold runs

| Run | Support | Confidence | Itemsets | Rules |
|---|---:|---:|---:|---:|
| A | 3% | 30% | 115 | 12 |
| FINAL | 1% | 30% | 1,219 | 950 |
| B | 1% | 60% | 1,219 | 238 |

All 950 FINAL rules have lift greater than 1.

With the most frequent item appearing in 1,959 of 17,080 baskets, any non-empty consequent has baseline support at most 1,959/17,080 ≈ 11.47%. Therefore:

- FINAL lower bound: lift ≥ 0.30 / (1959/17080) ≈ 2.62
- W1 lower bound: lift ≥ 0.60 / (1959/17080) ≈ 5.23

These are theoretical lower bounds, not observed minimum lifts.

## Manual verification

Manual checks were kept separate from automated validation. They included:

- baseline behavior;
- tiny-example arithmetic;
- real U1 support/confidence/lift calculation;
- forward/reverse confidence check;
- A / FINAL / B reruns;
- U1 / W1 / W2 inspection;
- final UI test rerun;
- independent validation rerun.

The full randomized suite and all 950 rules were checked programmatically rather than recomputed manually one by one.
