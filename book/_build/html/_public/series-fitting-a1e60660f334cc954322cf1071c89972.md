# Fit a series of spectra

:::{note} Current workflow — GUI 0.2.27
Preparation and fitting are separate collection operations. This chapter covers
the implemented fitting controls and staged review. Practise with the
[synthetic series exercise](../examples/series-walkthrough.md), or follow the
[acquisition workflow](../examples/beamline-workflow.md) with your own data.
:::

Start by fitting a representative spectrum and inspecting its residuals,
components, bounds and constraints. This reference model is the starting point
for the collection workflow. The current working model is used, including edits
since its last successful fit. It does not establish that every later spectrum
has the same physical interpretation.

## Set up a first series fit

1. Import the scans and explicitly prepare them if required; choose the source's
   intended fitting channel/stage.
2. Fit a representative scan, then check its components, bounds, fixed states and
   links. Deliberate edits afterward also become part of the reference model.
3. Click **Propagate model / fit…** beside Fit spectrum and check the desired targets.
4. Expand **Series setup** if collapsed. Choose a strategy and whether **Use the source fitting domain for every target**
   should be checked. Review the target channel availability.
5. If needed, set **Series constraints…** and **Trends / pass-two setup…** before
   staging. A pass with coupled trends needs simultaneous mode.
6. Click **Stage pass one / single pass**, select result rows and inspect curves,
   status and statistics. Working scans remain unchanged while staged.
7. Apply the reviewed successful results, or configure [pass two](two-pass-fitting.md)
   first. Save a session after application and export the selected retained fits.

For an exercise with files and expected values, follow the
[synthetic series walkthrough](../examples/series-walkthrough.md).
On small screens, [layout controls](layout-and-small-screens.md#preparation-and-series-dialogs)
keep setup scrollable while staging/Apply/Cancel remain available.

## Choose targets and a strategy

If the acquisitions need preparation, use the shared
[Prepare scans workflow](preparing-data.md#preparation-is-explicit) first. Inspect
normalisation overlays and, where needed, each acquisition's reference calibration
before applying the preparation. Then select the intended fitting channel/stage.

Click **Propagate model / fit…** beside Fit spectrum. Select target scans
explicitly. The active channel identifies the target channel; this is different
from overlaying each row's arbitrary detector selection. A result for a cached
channel can be applied without changing that scan's displayed channel.

| Strategy | Starting values / parameter sets |
| --- | --- |
| Fit each target from the reference | Independent fit starting from the same reference model |
| Fit in order from the previous success | Independent fits, seeded successively in the chosen order |
| Fit simultaneously | One optimisation with global and per-spectrum parameters |
| Copy without fitting | Stage the model for targets; does not create a successful fit result |

Sharing dropdowns in the main parameter table matter for simultaneous fitting.
`global` means one value across targets; `per_spectrum` gives each target an
independent value. A label such as “bulk” does not create sharing automatically.

For example, share peak μ and sigma globally while letting A vary per spectrum
to compare intensities with a common line position/width. The scope is an explicit
model assumption, not a claim that every series should have constant widths.

Choose whether to use the source fitting range or each target's retained range.
Inspect availability before starting: a channel/stage that has not been prepared
or visited may be unavailable. The source is changed only if explicitly included
as a target. The current source model, including edits since its last successful
fit, is frozen when the workflow opens; an older fitted snapshot is not substituted.

## Inspect residuals and compare parameter trends

In GUI **0.2.25**, the **Spectrum / residual** tab shows the selected staged target
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
total experimental uncertainties; [statistics limitations](../reference/constraints.md#read-the-residual-before-trusting-a-number)
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

## Restrict propagation around the reference

**Series constraints…** can narrow selected parameters relative to the original
reference using absolute or percentage windows, or fix them to reference values.
These limits intersect the source model's bounds. They do not drift with each
previous-fit seed. Use a physically motivated tolerance rather than making all
parameters nearly immovable.

Linked targets remain derived. Restrict their reference or separation parameter
instead of imposing an independent bound that the target cannot honour.
This includes every linked ionisation-edge position: each follows its controlling
peak's effective scope. Configure those links in the
[edge panel](peak-constraints.md#link-the-first-ionisation-edge-to-a-peak) before staging.

| Series-constraint choice | Example / meaning |
| --- | --- |
| Keep reference bounds | Keep the existing allowed interval |
| Absolute tolerance | μ = 285.20, Below/Above = 0.05 eV gives 285.15–285.25 eV before intersection |
| Percentage tolerance | sigma = 0.30, Below/Above = 5% gives 0.285–0.315 eV before intersection |
| Fix to reference | Hold the reference value; Free becomes false without setting equal bounds |

Bounds are intersected with the reference's existing interval, so a tolerance
cannot widen it. Percentages use `abs(reference value)`; a zero reference requires
an absolute tolerance or Fix. Below/Above can differ, must be nonnegative and
cannot both be zero for a tolerance rule. Fixed, linked and static rows explain
their roles instead of being silently freed. Review **Effective series bounds**
before staging; Ctrl/Shift and the selected-row controls can apply a rule to many rows.

These limits stay centred on the **original reference**, even in previous-success
seeding mode. Seeding changes the start, not the allowed reference interval. They
are hard bounds, not automatically inferred uncertainties. Accepted recipes are
saved with the source-channel session view; applied targets retain ordinary
bounds/fixed states and provenance. Saving only the source model does not activate
its separate collection recipe.

## Trends and ordering

**Trends / pass-two setup…** supports soft penalties or strict constraints for
monotonic evolution, a straight-line trend, smoothness, endpoints, maximum
step/slope or curvature, and an identified scan as a maximum/minimum. A chosen
extremum can also have monotonic sides. These rules couple scans and require a
simultaneous fit.

Choose equal scan order or explicitly enter increasing numeric coordinates such
as time or dose. Coordinates define spacing for derivative limits; their units
therefore affect the meaning of a maximum slope/curvature. Scan order is an
explicit input, not an inferred acquisition chronology.

A soft penalty encourages a trend. It does not guarantee it. Strict constraints
use a separate constrained solver; see [constraints and statistics](../reference/constraints.md)
for the distinction and its uncertainty limitations.

### Choose what a trend means

Open **Trends / pass-two setup… → Overall trends**, add a rule and select an active
independent **per_spectrum** parameter. A global parameter already has one value
across the series. For a linked target, select its controlling source/separation.
Choose whether the rules apply to pass one, pass two or both.

| Rule | Soft strength | Strict strength |
| --- | --- | --- |
| Increasing / decreasing | Penalise adjacent violations beyond fractional tolerance | Non-decreasing / non-increasing |
| Maximum / minimum at selected scan | Encourage the named acquisition to dominate the others | Named acquisition at least / at most every other target |
| Smooth curvature | Penalise curvature, optionally in log space | Exact straight line |
| Small steps | Penalise adjacent changes, optionally in log space | Constant value |
| Final / initial ratio | Encourage the chosen ratio | Exact ratio |
| Final value | Encourage the chosen endpoint | Exact endpoint |
| Maximum absolute slope | Penalise excess above the chosen maximum | Bound absolute slope |
| Maximum absolute curvature | Penalise excess above the chosen maximum | Bound absolute curvature |

In particular, strict “small steps” means **zero** step. Use maximum absolute
slope for a nonzero allowed step limit. Equal scan order gives slope units per
scan; numeric time/dose gives units per time/dose. A maximum curvature is a
second-derivative restriction, not an energy-width bound.

Soft **Weight** is lambda: penalty residuals scale by its square root. **Floor**
protects relative/log calculations. **Fraction tolerance** is a fraction (0.02
means 2%), not an entered percentage; it is distinct from the percentage-tolerance
columns in Series constraints. Target is the endpoint, ratio or nonnegative maximum
for rules that use it. Inactive cells are retained but ignored.

### Pin a maximum or minimum to an acquisition

Choose the named scan in **Extremum scan**; it must also be checked as a target.
**Extremum only** compares it with all other selected targets. **Monotonic sides**
additionally imposes rise/fall for a maximum or fall/rise for a minimum in the chosen
order. Ties/plateaus are allowed, and an endpoint can be the extremum. The anchor
follows the acquisition when rows move; it is not silently changed to the largest
fitted value or implicitly checked as a target.

For a middle scan expected to have the largest P2 area, choose P2_A, Maximum at
selected scan, that middle acquisition and the appropriate strength. Choose
Monotonic sides only if intermediate scans should also follow that shape. This
does not constrain any later coverage or kinetic calculation.

### Order, coordinates and incompatible constraints

Use **Move scan up/down** to set the order explicitly. Numeric coordinates must
increase strictly for selected targets; equal/reversed values are rejected instead
of sorted. Coordinate units are chosen by the user. A limit of 0.1 area per minute
is different from 0.1 area per second. Soft nonuniform smoothness normalises mean
spacing; maximum derivative limits always use the actual coordinate units.

Infeasible strict rules/bounds or a nonconverged joint fit produce no successful
joint stage. Strict constraints are not huge penalties and currently have no
covariance/stderr estimate; only budget/ftol apply. Soft-only fits use least squares
with all three tolerances. Joint statistics can include penalty residuals; inspect
data agreement separately. See [fitting and results](fitting-and-results.md).

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

- [Prepare acquisition channels before fitting](preparing-data.md).
- [Distinguish within-model links from series rules](peak-constraints.md).
- [Choose mean seeds and narrower second-pass windows](two-pass-fitting.md).
- [Practise the complete series workflow on known data](../examples/series-walkthrough.md).
- [Export applied series fits](exporting-fits.md#export-the-active-fit-or-selected-series-scans).
- [Resolve staging, bounds and target-selection problems](troubleshooting.md#fits-series-stages-and-exports).
