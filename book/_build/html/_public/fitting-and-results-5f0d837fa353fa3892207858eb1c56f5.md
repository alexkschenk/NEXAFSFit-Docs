# Running a fit and reviewing the result

These instructions describe GUI **0.2.27**. Begin with a model that roughly matches
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

Peak μ bounds are a third quantity: they restrict a component's allowed position,
not the spectrum's fitted energy domain. Use the parameter table, bound handles
or the plot's **μ bounds from zoom** context action to change those explicitly.
The [drag-control guide](drag-controls.md#set-position-bounds-by-dragging-lines)
shows which lines to grab and how to expose off-screen or infinite bounds.

The GUI evaluates the full loaded grid before selecting residual points. This
keeps the linear background's mean-energy reference stable for a restricted fit.
Do not crop arrays in a Python comparison and assume the background is unchanged.

## Set the evaluation budget and tolerances

Open **Fit settings…** in the Fitting box (under **More…** in Compact).
The default maximum is **50,000 residual
evaluations**, including numerical-derivative calls; it is not an iteration count.
The three default tolerances are **1e-8**.

| Control | Ordinary least-squares stopping test |
| --- | --- |
| ftol | Small relative improvement in cost |
| xtol | Small parameter-step change |
| gtol | Small bounds-scaled gradient |

Keep one common tolerance or uncheck **Use one convergence tolerance for all three
tests** to edit them separately. Smaller values demand tighter convergence;
larger values permit earlier stopping. Settings persist in sessions and are used
for explicit collection workflows too. Strict series trends use a different solver:
only their budget and ftol apply; xtol/gtol are inactive.
Use **Restore Defaults** to reset the dialog's controls. Click **OK** to
use the edited values; cancelling leaves the existing settings intact. Successful
fit records keep the settings used for that fit, independently of later edits.

:::{figure} ../media/images/fit-settings-preview.png
:label: figure-fit-settings
:alt: Fit settings dialog with maximum evaluations and common or separate ftol, xtol and gtol controls.

Evaluation budget and convergence settings; tighter tolerances do not establish
that a model is physically appropriate.
:::

## Fit the active spectrum

Click the blue **Fit spectrum** button or press **F5**. Only the active row's displayed channel
is fitted; overlay curves remain comparisons. The worker uses a snapshot while
editing is disabled. A progress window shows residual evaluations and elapsed time.
Its bar represents the fraction of the evaluation **budget**, not a prediction of
completion time: convergence can occur before that bar fills.

Choose **Stop fit**, press Escape or close the progress window to request a safe
stop. The worker checks at evaluation boundaries; an evaluation already running
must return first. The window stays open with a stopping message until the worker
exits normally. A stopped fit discards trial parameters and retains the working
model and previous successful result. Wait for completion before closing the main
window. [Collection stopping](series-fitting.md) retains its separate behaviour.
The standalone application also requests a [recovery checkpoint](sessions-and-models.md#recover-interrupted-work)
before launching the fit.

A successful fit updates the working model and retains a fitted-model snapshot
and statistics. A failed fit retains the prior successful result; it does not
promote the current trial to a successful result. Later parameter edits change
the working plot while the retained successful snapshot remains separate.

## Read more than the convergence flag

Open **Last fit results** and inspect the Messages panel. Review:

- Residual structure: data minus model should not show unresolved systematic features.
- Parameters at/near bounds, implausible widths, or large compensating areas/gain.
- Whether fixed states, links, shared parameters and fit domain match your intent.
- Sensitivity to reasonable starting values, windows and component choices.
- Uncertainties, remembering that missing stderr is unavailable rather than zero.

Chi-square depends on signal units and preparation. A smaller value after adding
parameters is not by itself evidence for a better physical interpretation.
Joint series statistics describe all fitted scans and may include soft penalties;
per-scan misfit is a separate report. Strict trends currently have no covariance
uncertainties. Definitions and limitations are in the
[shared statistics reference](../reference/constraints.md#read-the-residual-before-trusting-a-number).

## When the budget is exhausted

Check the active channel/stage, fit range, model shape, parameter bounds and
links first. A missing edge or near-degenerate set of peaks can consume a much
larger budget without becoming identifiable. Use graphical positioning or a
simpler model to improve the starting point. Then increase maximum evaluations
if more work is justified, or adjust tolerances deliberately.

The Messages panel's **History…** button contains the full recent diagnostic
text. It is separate from the menu's model Undo/Redo history. Save a session
to retain successful work before major preparation/model changes, and use
[CSV exports](exporting-fits.md) for the retained numerical reports.

## Related topics

- [Check the active detector and preparation stage](scans-and-channels.md#select-each-scans-detector-independently).
- [Adjust component starting values and position bounds](drag-controls.md).
- [Propagate a reviewed model across scans](series-fitting.md).
- [Export the retained successful fit](exporting-fits.md).
- [Check fit/series/export problems by symptom](troubleshooting.md#fits-series-stages-and-exports).
