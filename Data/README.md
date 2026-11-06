# Warranty Analytics Dashboard: Find the Hidden Failure

A portfolio project that mirrors the core work of a warranty / reliability analytics role: join claims to sales, clean messy data, publish warranty metrics, find an emerging failure trend, and drive it to a corrective action.

**Important:** all data here is synthetic and says so. State this on your repo page and in interviews. Never present it as real company data.

## The scenario

You are the warranty analyst for a power-equipment maker with four product families (UPS 1kVA, UPS 10kVA, LV switchgear panel, surge protection device) built at two plants and sold in four regions. Leadership asks: "Where is our warranty cost coming from, and what should engineering fix first?"

## Files

| File | What it is |
|---|---|
| `data/sales.csv` | Units sold by product, plant, region and build month (clean) |
| `data/claims_raw.csv` | Warranty claims as exported: messy, with duplicates, inconsistent labels and bad values |
| Answer key | The planted problem and every injected data issue. Not included in your download; ask Claude for it once you have your own findings. |
| `ppit_car_tracker_template.csv` | Blank action tracker for the corrective-action step |

As-of date for the data is 31 Oct 2026. Builds run Jan 2025 to Jun 2026.

## Step 1: Clean the claims (SQL or Python)

Write down every issue you find and how you handled it. This log is part of your deliverable. Things to check:
- Exact duplicate rows
- Product names, region labels and date formats written several ways
- Missing build month (the serial number contains the build month, so it is recoverable)
- Missing plant
- Negative, zero or absurdly large claim costs
- Claims dated before the unit was built
- Vague failure descriptions

For each fix, decide: correct it, exclude it, or flag it. Justify the choice in one line.

## Step 2: Classify failure modes

Turn the free-text `failure_description` into a small, consistent set of failure modes (about 8 to 10, plus "Unclassified"). Start with keyword rules in Python or SQL. Report what share is unclassified and don't hide it.

## Step 3: Build the metrics

Define each one in a notes tab before building it.

| Metric | Definition |
|---|---|
| Claims | Count of cleaned unique claims |
| PPM | Claims divided by units sold, times 1,000,000, for a comparable cohort |
| CPU | Total claim cost divided by units sold |
| Top N | Failure modes (and product families) ranked by claim count and by cost |

**The trap to avoid:** units built recently have had less time to fail, so raw PPM makes new builds look better than old ones. Compare cohorts at the same age, for example claims within 12 months of build, using only builds that have had 12 months. Explain this in your notes.

Accruals are optional and need an assumption you must state clearly (for example, expected future claims from the cohort's early-life failure curve).

## Step 4: Dashboard (Power BI)

Suggested pages:
1. **Overview:** claims, cost, PPM and CPU by product family, with a Top N failure-mode view.
2. **Trend:** PPM by build month at equal age, filterable by product, plant and region.
3. **Deep dive:** the one anomaly you found, with the evidence that isolates it.
4. **Data quality:** what you cleaned and what remains unreliable.

Keep each page answering one question.

## Step 5: The deep dive

Your goal is to explain one spike, not report it. Slice the data by plant, build month, region and failure mode until you can say which cohort is affected and why it stands out. Then separate the main issue from smaller patterns: check whether a difference is large enough to matter or whether it could be explained by environment or normal variation.

## Step 6: One-page findings note

Write for an engineering manager, not an analyst. Include:
- The finding in one sentence with its number
- Evidence (one chart, one table)
- What is and isn't known (for example, you can see the cohort and failure mode, but not the root cause)
- A recommended next action and who should own it
- Data-quality caveats

## Step 7: Corrective-action tracker

Fill `ppit_car_tracker_template.csv` for your finding: owner, containment action, root-cause hypothesis, verification metric, due date. Add one line on how you would confirm the fix worked (for example, PPM of builds after the change).

## Resume bullet (fill in with your real results)

> Built a Power BI warranty dashboard on a synthetic claims and sales dataset (PPM, CPU, Top N); cleaned [N] raw records across [N] data issues and isolated [your finding with its number], recommending [your action].

Only use numbers you computed yourself.

## Interview talking points

- Why equal-age comparison matters
- How you decided what to exclude versus correct
- The difference between your finding and a root cause
- What you would do differently with real data
