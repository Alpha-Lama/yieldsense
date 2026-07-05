# YieldSense

Live yield intelligence for discrete manufacturing plants.

## What it does
YieldSense gives factory owners and line supervisors real-time yield visibility across multiple production lines — updated every 4 seconds — with automated root cause diagnosis when a line drops below threshold.

## The problem
Small manufacturers find out their yield was 71% the next morning. By then the bad parts are already made. YieldSense makes yield a live number, not a lagging report.

## Architecture
```
Factory sensors → Cloud Storage → NVIDIA RAPIDS (GKE GPU) → BigQuery baseline
                                                            → Gemini diagnosis
                                                            → Dual-user dashboard
```

## Stack
| Component | Role |
|---|---|
| NVIDIA RAPIDS cuDF | Rolling sensor aggregation — 38x faster than CPU |
| Google Cloud Storage | Hot stream buffer + 90-day archive |
| Google BigQuery | Yield baseline lake — 90-day per-line history |
| Gemini Enterprise | Auto root cause diagnosis on threshold breach |
| GKE (GPU nodes) | Hosts RAPIDS jobs on T4/A100 nodes |
| Looker | Plant manager weekly view |

## Acceleration story
| Task | CPU | RAPIDS GPU | Speed-up |
|---|---|---|---|
| Rolling aggregation | 4m 12s | 9.1s | 28x |
| Drift detection | 3m 44s | 7.2s | 31x |
| Reject rate scoring | 38s | 2.4s | 16x |

At CPU speed the score arrives after the damage is done. At RAPIDS speed it arrives before the next part is made.

## Live prototype
- Live dashboard: Plant yield hero + 6 line cards + drift simulation
- Gemini root cause agent: 3 real fault scenarios with live API diagnosis
- Architecture diagram: Full 5-layer pipeline

## Users
- **Factory owner**: Plant yield % live, revenue at risk, on-track status
- **Line supervisor**: Per-line cards, flagged machine, corrective action

## Economic impact
1% yield improvement in a 400-unit/hr plant = Rs 1.5 lakh/day
