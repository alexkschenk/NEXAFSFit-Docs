# Review staged series results

GUI **0.2.30**. First [stage a series fit](series-fitting.md). This page describes its review tabs; staging does not change working scans.

(inspect-residuals-and-compare-parameter-trends)=

## Inspect residuals and compare parameter trends

In GUI **0.2.30**, the **Spectrum / residual** tab shows the selected staged target
and its data-minus-model residual. Its label identifies the scan and reviewed pass.
Failed or unstaged rows clear the previous curves and show their status, so a
neighbouring successful fit cannot be mistaken for that target. The preview covers
the loaded grid; optimisation still uses each target's chosen fitting domain.

Open **Parameter trends / compare passes**, then choose a parameter. Blue is pass
one and orange is pass two; both remain visible when available. Red crosses mark
reported bound hits. Missing/failed targets leave gaps rather than zero values.
The adjacent table reports values, available standard errors, bounds flags and
per-pass status. Fixed parameters are labelled and have no error bars; copied
models are identified as copies. Available bars are ±1 local standard error, not
total experimental uncertainties; [statistics limitations](../reference/fit-statistics.md#read-the-residual-before-trusting-a-number)
still apply, including unavailable covariance for strict fits.

The review-axis dropdown offers selected scan order, the configured numeric
coordinate, and incidence angle when recognised degree-valued scan metadata is
available. Angle values follow the scan-table source/convention, including the
identified R1 correction; missing angles remain gaps. These display choices do
not reorder targets, change trend constraints or invalidate a fit. Native model
parameter values match the parameter table: EMG μ is not its maximum, and μ is
not shifted into displayed-energy coordinates. Shared parameters can legitimately
form a constant series. Click a plotted point or review-table row to select that
scan in the target table; use the spectrum tab to inspect its curve.

:::{figure} ../media/images/series-review-preview.png
:label: figure-series-parameter-review
:alt: Parameter review showing independently fitted Gaussian widths in blue and a fixed mean width in orange, with numeric coordinates and per-pass values and uncertainty status.

Synthetic comparison: independent widths in pass one, fixed mean width in pass
two. This illustrates the effect of an imposed assumption, not evidence that the
second pass is physically preferable.
:::

Use [two-pass review](two-pass-fitting.md#stage-select-and-apply-the-intended-pass)
to choose which pass **Apply** commits. Changing the plotted parameter or review
axis does not change that choice.

## Review staged results before applying

Stage the selected targets, inspect successes, failures and fitted curves, then
choose **Apply successful results**. Failed/unselected scans retain their work.
Cancelling discards staged changes; cancelling a running job requests a safe
stop and waits for its worker.

For averaging, sharing promotion and tighter second-pass windows, follow
[two-pass fitting](two-pass-fitting.md). Merely running either pass does not
replace working scans. Choose the reviewed successful fits in the
[CSV export selector](exporting-fits.md) after applying them.

:::{note} Scope of this introduction
The GUI implements the controls above. The
[synthetic series walkthrough](../examples/series-walkthrough.md) demonstrates a
small reproducible case. A complete experimental two-pass analysis remains future
documentation work; no scientific interpretation is inferred from group names.
:::

## Related topics

- [Set up series targets and strategy](series-fitting.md).
- [Choose the pass to apply](two-pass-fitting.md#stage-select-and-apply-the-intended-pass).
- [Export applied successful results](exporting-fits.md).
