# Restrict series parameters around a reference

GUI **0.2.30**. Open **Series constraints…** from [series setup](series-fitting.md). Start with a reviewed reference model and choose physically justified tolerances.

(restrict-propagation-around-the-reference)=

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
[edge panel](edge-links.md#edge-links-position) before staging.

:::{figure} ../media/images/series-constraints-controls.png
:label: figure-series-constraints-controls
:alt: Series constraints table with absolute tolerance for a Gaussian position, percentage tolerance for an EMG width, a fixed EMG tail and effective bounds alongside reference bounds.

Illustrative absolute, percentage and fixed-reference rules. Compare **Reference
bounds** with **Effective series bounds** before Apply. These synthetic choices
are not estimates of experimental error.
:::

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

:::{important} Reference limits stay anchored
These limits stay centred on the **original reference**, even in previous-success
seeding mode. Seeding changes the start, not the allowed reference interval. These
hard bounds are chosen restrictions, not automatically inferred uncertainties.
:::

Accepted recipes are
saved with the source-channel session view; applied targets retain ordinary
bounds/fixed states and provenance. Saving only the source model does not activate
its separate collection recipe.

## Related topics

- [Select targets and a fitting strategy](series-fitting.md).
- [Specify overall trends](series-trends.md).
- [Choose pass-two seed windows](two-pass-fitting.md).
