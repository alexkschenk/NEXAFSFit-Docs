# Two-pass fitting and averaged reseeding

This guide describes GUI **0.2.24**. Use a broad first pass to inspect a series,
then deliberately select seeds, narrower bounds and parameter scopes for pass two.
The [synthetic walkthrough](../examples/series-walkthrough.md) supplies runnable data.
Begin with [series setup](series-fitting.md#set-up-a-first-series-fit) and a reviewed
[single-spectrum fit](fitting-and-results.md). Pass-two options build on that staged workflow.

## Stage and review pass one

1. Fit a representative spectrum and inspect its model, bounds and links.
2. Open **Propagate model / fit…**, select targets, strategy, fit ranges and order.
   Set any reference-relative **Series constraints…** deliberately: they already
   restrict the first pass, so do not make them narrower than intended.
3. Use **Trends / pass-two setup…** to assign trend rules to the intended pass(es).
   Coupled rules require simultaneous fitting for the pass that uses them.
4. Click **Stage pass one / single pass**. Select rows and inspect fits/errors.

Every selected target must have a successful **fitted** pass-one result before
pass two can be seeded. Copy-without-fitting results are not sufficient. Staging
does not change working scans; the first pass remains available for review.

## Choose seeds per parameter

Open **Trends / pass-two setup… → Pass-two reseeding / bounds**. Check Use only for
the parameters you intend to configure. Unchecked parameters keep their own
pass-one values, bounds, fixed state and sharing.

| Seed choice | Value used for a configured parameter |
| --- | --- |
| Own pass-one | That target's fitted first-pass value |
| Mean | Unweighted arithmetic mean over explicitly chosen successful first-pass target scans |
| Reference | Original working reference model captured when the workflow opened |

The **Average scans** button sets the pool for one parameter. Select several rows
and use **Set averaging scans for selected parameter rows…** to give them a common
pool. Non-target or failed scans cannot provide means. A mean is a model seed,
not a data average, intensity normalisation or uncertainty estimate.

For a named group, choose it and click **Select group parameters**. Set the batch
seed/bounds/Sharing choices, then **Enable / set selected rows**. This shortcut
selects related parameter rows; it does not deactivate other peaks. Review the
individual rows afterward, especially when area and width need different tolerances.

## Narrow bounds and optionally release sharing

| Bounds choice | Pass-two behaviour |
| --- | --- |
| Keep pass-one bounds | Change the selected seed without narrowing its allowed interval |
| Absolute tolerance | Intersect seed-minus-Below to seed-plus-Above with existing bounds |
| Percentage tolerance | Intersect a window based on the absolute seed magnitude with existing bounds |
| Fix to chosen seed | Hold that value fixed with valid ordered bounds |

For a mean μ seed of 285.00 eV, an absolute 0.03 eV window allows 284.97–285.03 eV
before intersection. A 5% window around sigma = 0.32 gives 0.304–0.336 eV.
Percentage windows around zero are not meaningful: use an absolute window or Fix.
A selected seed must be legal for each target; a mean outside an individual
pass-one bound is reported, not silently clamped. Fixed parameters stay fixed.

Sharing can remain unchanged, become **Per spectrum**, or become **Global**.
Changing a pass-one shared position to per_spectrum allows small scan-specific
adjustments inside the tighter interval. If independent pass one produced different
values for a parameter that will be global in pass two, choose a common Mean/
Reference seed or change Sharing explicitly. No automatic averaging is inferred.

Click **Review pass-two starting values and effective bounds…** before fitting.
Inspect each target's resolved values and intervals, then **Apply setup** to return
to the collection dialog. This accepts the recipe, not any fitted scan.

:::{figure} ../media/images/second-pass-preview.png
:label: figure-second-pass
:alt: Pass-two setup table with per-parameter seed methods, tolerance windows, Sharing and averaging-scan controls.

Review seeds, pools and bounds before staging the second optimisation. The example is synthetic.
:::

## Stage, select and apply the intended pass

Choose independent-per-seed or simultaneous **Pass two**, then **Stage pass two**.
Pass-two trends require simultaneous mode. Use **Review staged results → Review /
use selected pass** to switch between passes and inspect their curves/statistics.
**Apply successful results** commits the currently reviewed pass only.

Use the [parameter-comparison tab](series-fitting.md#inspect-residuals-and-compare-parameter-trends)
to plot both passes against scan order, coordinates or available incidence angles.
Look for changed values, bound hits and failed-target gaps, then inspect the
selected scan's residual. Plot selection does not switch the pass to be applied.

| Change or outcome | What remains available |
| --- | --- |
| Edit only pass-two setup/strategy | Pass one is retained; pass two is invalidated |
| Change targets/order/range/first-pass strategy or first-pass rules | Both stages are invalidated |
| Second pass fails | First pass remains available for explicit review/application |
| Cancel / discard | Working scans stay unchanged; an active worker is stopped safely before closing |
| Save session after applying | Accepted source-channel recipe and applied results/provenance are retained |

An uncommitted dialog stage is not saved by saving the main workspace. Save a
session before a major workflow if you need a persistent checkpoint. Applied
results include the resolved seeds/windows and pass-one provenance, but are not
evidence that tighter bounds improved the scientific model. Compare residuals
and bound hits between passes, then export the reviewed retained fits.

## Related topics

- [Select series targets, strategies and trend rules](series-fitting.md).
- [Keep parameter sharing distinct from reseeding](../reference/constraints.md#sharing-across-spectra).
- [Check uncertainty and joint-statistics limitations](../reference/constraints.md#read-the-residual-before-trusting-a-number).
- [Work through a synthetic mean-seeded second pass](../examples/series-walkthrough.md).
- [Save applied provenance and export the reviewed pass](exporting-fits.md).
