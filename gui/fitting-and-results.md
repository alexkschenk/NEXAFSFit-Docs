# Fit the active spectrum

These instructions describe GUI **0.2.30**. Begin with a model that roughly matches
the active scan/channel; see [model construction](model-editor.md).
Check its [Detector/stage selection](scans-and-channels.md#select-each-scans-detector-independently)
before fitting, particularly after inspecting raw monitors or prepared channels.

## Separate the viewing range from the fitting range

Expand **Fit energy range** in the Fitting box to expose its limits and lock.
The collapsed header still reports both. For panel placement and Compact mode,
see [workspace layouts](layout-and-small-screens.md).

Zooming controls the view. The **Fitting** box's minimum/maximum energies and
shaded fit region control which residual points enter optimisation. Unlock the
fit range to drag its boundaries, then lock it before arranging peaks. **Show fit
window** hides its graphic while retaining the chosen limits.

:::{figure} ../media/images/fitting-controls.png
:label: figure-fitting-controls
:alt: Fitting box with expanded energy limits locked at 280 to 305 eV, prominent Fit spectrum button, propagation button and settings/results actions.

The Fitting box with its energy-range controls expanded. The example domain is
locked; unlock it to edit the limits. Compact layouts place settings and results
under More instead of the separate buttons shown here.
:::

Peak μ bounds are a third quantity: they restrict a component's allowed position,
not the spectrum's fitted energy domain. Use the parameter table, bound handles
or the plot's **μ bounds from zoom** context action to change those explicitly.
The [drag-control guide](drag-controls.md#set-position-bounds-by-dragging-lines)
shows which lines to grab and how to expose off-screen or infinite bounds.

The GUI evaluates the full loaded grid before selecting residual points. This
keeps the linear background's mean-energy reference stable for a restricted fit.
Do not crop arrays in a Python comparison and assume the background is unchanged.

## Fit the active spectrum

Click the blue **Fit spectrum** button or press **F5**. Only the active row's displayed channel
is fitted; overlay curves remain comparisons. The worker uses a snapshot while
editing is disabled. A progress window shows residual evaluations and elapsed time.
Its bar represents the fraction of the evaluation **budget**, not a prediction of
completion time: convergence can occur before that bar fills.

### Stop a running fit safely

Choose **Stop fit**, press Escape or close the progress window to request a safe
stop. The worker checks at evaluation boundaries; an evaluation already running
must return first. The window stays open with a stopping message until the worker
exits normally. A stopped fit discards trial parameters and retains the working
model and previous successful result. Wait for completion before closing the main
window. [Collection stopping](series-fitting.md) retains its separate behaviour.
The standalone application also requests a [recovery checkpoint](recovery.md#recover-interrupted-work)
before launching the fit.

### Keep successful results distinct from current edits

A successful fit updates the working model and retains a fitted-model snapshot
and statistics. A failed fit retains the prior successful result; it does not
promote the current trial to a successful result. Later parameter edits change
the working plot while the retained successful snapshot remains separate.

## Related procedures

### Set the evaluation budget and tolerances

See [Set the evaluation budget and tolerances](fit-settings.md#set-the-evaluation-budget-and-tolerances) for this procedure.

### Read more than the convergence flag

See [Read more than the convergence flag](fit-results.md#read-more-than-the-convergence-flag) for this procedure.

### When the budget is exhausted

See [When the budget is exhausted](fit-settings.md#when-the-budget-is-exhausted) for this procedure.

## Related topics

- [Check the active detector and preparation stage](scans-and-channels.md#select-each-scans-detector-independently).
- [Adjust component starting values and position bounds](drag-controls.md).
- [Propagate a reviewed model across scans](series-fitting.md).
- [Export the retained successful fit](exporting-fits.md).
- [Check fit/series/export problems by symptom](troubleshooting.md#fits-series-stages-and-exports).
