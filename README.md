# Nudge or Nag

**When should a job page proactively wave an "Optimize" badge at its owner?** A Bayesian Beta-Binomial Monte Carlo decision prototype.

- **Live demo:** https://shriyashekatkar25-byte.github.io/nudge-or-nag/
- **Gallery:** https://shriyashekatkar25-byte.github.io/apm-lab/

Part of **APM Lab** by Shriya Shekatkar (UCLA, Stats & Data Science minor). Stats 102C Monte Carlo Methods applied to real product decisions.

## The launch it mirrors
On Oct 26, 2026, Databricks listed **Lakeflow Performance Observability** as generally available: job-level query performance metrics, plus an Optimize button that opens Genie Code. Source: [Databricks platform release notes, October 2026](https://docs.databricks.com/aws/en/release-notes/product/2026/october).

## The PM decision
The pull-only button already exists. The harder question is whether, and when, the product should *push*: badge a job on the job list (and in a weekly digest) when it has probably regressed.

- **Customer problem:** owners of hundreds of jobs can't tell noise (a cold cluster, a skewed input day) from a real slowdown.
- **Proposed change:** badge a job when P(slow-run rate > 20% | last n runs) ≥ τ, where a slow run takes more than 1.5× the job's trailing median.
- **Success metric:** badge precision (share of badged jobs that truly regressed); secondary: Optimize click-through from badges.
- **Guardrails:** precision ≥ 70%, recall ≥ 50%, false badges ≤ 3 per 100 jobs, a decision within ≤ 12 runs.
- **Stakeholders:** job owners, Lakeflow Jobs and observability engineering, the Genie Code team, platform admins, Support.

## What the prototype shows
- **Customer problem framing:** the badge is judged by trust (precision) first, not by how many jobs it flags.
- **Prototyping:** 240 seeded synthetic jobs; sliders for run history, confidence bar τ and prior strength; a canvas fleet map, a per-job posterior histogram, and a precision/recall sweep across τ.
- **Data-driven decisions:** it compares the Bayesian rule with a naive "raw slow fraction > 20%" rule. At the default settings the naive rule raises roughly four times as many false badges (about 10 vs 2.5 per 100 jobs).
- **SQL/Python:** an illustrative pipeline that counts slow runs per job in SQL, then draws Beta posteriors in NumPy and writes a badge table.
- **Clear writing:** a director memo rewrites itself from the simulation results, ending with a one-sentence VP summary.

## Methods
| Course chapter | Method | Where it's used |
|---|---|---|
| Ch2 Bayesian statistics | Beta prior + Binomial data → Beta posterior (conjugate update) | Each job's slow-run rate |
| Ch2 | Posterior mean as a weighted average of prior and data; credible intervals | Prior-strength slider, spotlight 90% interval |
| Ch1 Monte Carlo basics | Tail probability estimated as the share of 400 posterior draws above 20% | The badge rule |
| Ch1 | Seeded pseudorandom numbers; Bernoulli runs from uniforms | Reproducible fleet |
| Ch5 (support) | Box-Muller normals inside a gamma sampler to draw Beta variates | Posterior sampling |

## Design baseline
Each pin was used as design input only. Everything is redrawn in CSS, SVG and canvas, and no pin images are used.
- **Hero**: [Affirming (Lumi)](https://www.pinterest.com/pin/902268106616155137/). Pale grey ground, airy oversized pink sans type in a staggered, ragged stack.
- **Cards and buttons**: [diy cake corset (Lulu)](https://www.pinterest.com/pin/897834875763833149/). Satin pink gradients, ruffled scalloped edges, rainbow sprinkle strips, glossy cherry-red buttons.
- **KPI badges**: [Cutie Pony Room Shoes [Pink] (W Concept Global)](https://www.pinterest.com/pin/744360644705697676/). Pink, lilac and cream candy stripes around a yellow terry "sole" with a stitched sun motif.
- **Simulation room**: [Fantasy Art pin (LaKase Perry)](https://www.pinterest.com/pin/96264510780156865/). Black ground, chrome armor-plate panels and rivets, poppy-red flowers and green vines, a gold accent.

---
Unofficial concept prototype by Shriya Shekatkar. All data is simulated. Not affiliated with any company.
