# Fit a series of spectra

:::{note} Current workflow — GUI 0.2.30
Preparation and fitting are separate collection operations. This chapter covers
the implemented fitting controls and staged review. Practise with the
[synthetic series exercise](../examples/series-walkthrough.md), or follow the
[acquisition workflow](../examples/beamline-workflow.md) with your own data.
:::

:::{tip} Establish a representative model first
Start by fitting a representative spectrum and inspecting its residuals,
components, bounds and constraints. This reference model is the starting point
for the collection workflow. The current working model is used, including edits
since its last successful fit. It does not establish that every later spectrum
has the same physical interpretation.
:::

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

:::{figure} ../media/images/series-setup-controls.png
:label: figure-series-setup-controls
:alt: Series fitting dialog before staging, with expanded setup, simultaneous strategy, three checked targets, constraint and trend buttons, review tabs and staging footer.

Series setup with three synthetic targets checked and simultaneous fitting
selected. No results are staged yet, so the review area has no fitted curves
and Apply is unavailable. Check the strategy and target channel before staging.
:::

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

### Decide which parameters are shared

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

## Review staged results before applying

Follow [series result review](series-review.md), then choose the successful targets
and intended pass to apply. Working scans remain unchanged until that action.

## Related procedures

### Inspect residuals and compare parameter trends

See [Inspect residuals and compare parameter trends](series-review.md#inspect-residuals-and-compare-parameter-trends) for this procedure.

### Restrict propagation around the reference

See [Restrict propagation around the reference](series-constraints.md#restrict-propagation-around-the-reference) for this procedure.

### Trends and ordering

See [Trends and ordering](series-trends.md#trends-and-ordering) for this procedure.

## Related topics

- [Prepare acquisition channels before fitting](preparing-data.md).
- [Distinguish within-model links from series rules](peak-constraints.md).
- [Choose mean seeds and narrower second-pass windows](two-pass-fitting.md).
- [Practise the complete series workflow on known data](../examples/series-walkthrough.md).
- [Export applied series fits](exporting-fits.md#export-the-active-fit-or-selected-series-scans).
- [Resolve staging, bounds and target-selection problems](troubleshooting.md#fits-series-stages-and-exports).
