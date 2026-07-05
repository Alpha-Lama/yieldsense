# YieldSense — Brief Description

**What I built:**
YieldSense is a live yield intelligence dashboard for small and mid-size discrete manufacturing plants. It gives a factory owner one number — plant yield right now — and their line supervisor the exact machine causing a drop, with a corrective action, before the next part comes off the line.

**The problem it solves:**
50,000+ small manufacturers find out their yield was 71% the next morning. By then 200 bad parts are already made, material is wasted, and delivery is at risk. YieldSense makes yield a live number, not a lagging report.

**How it works:**
Sensor streams (spindle load, cycle time, reject counts, temperature) from 6 production lines land in Cloud Storage. NVIDIA RAPIDS cuDF processes rolling aggregations on GKE GPU nodes — delivering a per-line yield score every 4 seconds (vs 4+ minutes on CPU, a 38x speed-up). BigQuery stores 90 days of yield history as the drift detection baseline. When a line crosses the critical threshold, Gemini Enterprise Agent automatically generates a plain-language root cause diagnosis and corrective action. Looker serves the plant manager weekly view.

**Stack used:**
- NVIDIA RAPIDS + cuDF (GKE GPU nodes) — real-time sensor aggregation
- Google Cloud Storage — streaming buffer + 90-day archive
- Google BigQuery — yield baseline lake
- Gemini Enterprise Agent Platform — automated root cause diagnosis
- Google Kubernetes Engine — GPU node hosting
- Looker — management dashboard

**Why acceleration is load-bearing:**
At CPU speed (4m 12s per cycle), the score arrives after the damage is done. At RAPIDS speed (9.1s), it arrives before the next part is made. That is not a benchmark. That is a different product.

**User:**
Factory owners and line supervisors at small/mid-size discrete manufacturers — the 80% that SAP was never built for, with no data team and no real-time visibility.

**Economic impact:**
1% yield improvement in a 400-unit/hr plant = Rs 1.5 lakh/day saved.
