# Troubleshooting and workflow checks

This guide covers GUI **0.2.24**. Start with the scan, channel and stage you intend
to analyse, then locate the step that produced the unexpected result. Preserve a
session checkpoint before replacing preparation or applying a large series fit.

## Identify what is being changed

If a panel or button seems absent, check **View** and expandable section headers
before changing the model. Compact layouts tab Components/Parameters, may hide the
scan browser, and put settings/results under **More…**. [Layout recovery](layout-and-small-screens.md)
explains reset/show/detach controls and scrolling in preparation/series dialogs.

| Action | Scope |
| --- | --- |
| Choose a row's Detector | That scan's displayed signal and independent channel workspace |
| Check Overlay | A comparison line using that row's Detector; it does not add a fit target |
| Preview preparation | Staged processing of explicitly checked acquisitions, across mapped detectors |
| Fit spectrum | One active displayed channel |
| Stage a series fit | Explicit target scans on the source channel/stage; working scans are retained |
| Apply successful series results | Reviewed successful targets; other scans/channels retain their work |
| Export fitted CSV | Selected rows' displayed channels and retained successful fit snapshots |

The [folder-to-results guide](../examples/beamline-workflow.md) connects these
steps. [Channel and overlay management](scans-and-channels.md) explains their
independent selections.

## Import, metadata and channel selection

| Symptom | Check and next action |
| --- | --- |
| Only some folder files loaded | Read the failed filenames; folder loading handles native `.asc` files, not arbitrary CSV files or subdirectories. See [import diagnostics](import-diagnostics.md). |
| Review measured-energy order opens | Inspect the acquisition/sorted previews before accepting a sorted copy. Duplicates and nonfinite values cannot be repaired by sorting. |
| Incidence is unavailable or unexpected | Inspect Source/PV on the [Metadata tab](metadata.md). The identified R1 conversion is R1 − 45°; the raw position tuple retains R1 itself. |
| Set energy differs from the plotted endpoints | Set energy reports original mono setpoints. The plot can use measured or calibrated energies, and preparation may restrict PD overlap. |
| A prepared detector/stage is absent | Preparation may not have been applied, or that optional channel failed. Read Channel issues and the row's preparation status; switching channels does not run processing. |
| A scan says Previous preparation | It retains older work that could not be replaced by the latest recipe. Review or prepare/refit that channel before using it as current output. |

Right-click a scan to record observations in [scan notes](scans-and-channels.md#attach-notes-to-a-scan).
Notes are separate from recorded metadata and persist with complete sessions.

## Preparation and calibration

| Symptom | Check and next action |
| --- | --- |
| Apply is disabled after moving a window | Settings changed. Run a new successful Preview; old curves are retained for inspection, not as fresh apply-ready output. |
| The reference panel asks for Preview | Preview performs calibration. Merely opening the panel or changing its inspected scan does not fit a reference. |
| Reference marker/curve looks wrong | Check the reference channel, feature, search window and target. Parab3 is local three-point interpolation, not a general peak fit. See [reference inspection](preparing-data.md#inspect-the-reference-calibration). |
| No reference fit curve appears | Argextreme and boundary/degenerate parab3 fallbacks can supply a feature energy without a parabola. Read the panel diagnostic. |
| Separate photodiode processing fails | Check reference selection, mapped channels, I0 values and energy/setpoint overlap. Use the recorded error rather than silently dropping or interpolating rows. |
| Scale window gives an error or unexpected amplitude | Verify the aligned window and selected signal. Mean and signed trapezoidal area use different denominators; zero/nonfinite denominators are invalid. |
| Normalisation looks reasonable for one scan but not the series | Enable comparison overlays and inspect the same prepared stage across representative targets before Apply. |

Follow the full [preparation procedure](preparing-data.md) for window dragging,
all-detector processing and photodiode options. All preparation operates on copies;
it does not overwrite acquisition files.

## Component controls and fit limits

| Symptom | Check and next action |
| --- | --- |
| Clicking a curve only selects it | Grab its orange target, blue μ line or width/tail arrow to drag. See [the handle key](drag-controls.md#handle-key). |
| Position, width or tail will not move | Check Free, links and bounds. A fixed/linked area also prevents height-preserving shape drags. Read Messages for rejected changes. |
| EMG maximum and μ marker disagree | μ is the underlying Gaussian mean. The maximum generally shifts with tau; see [EMG interpretation](../reference/lineshapes.md#emg-interpretation). |
| Position-bound lines are absent | Enable Show μ bounds, select a visible independent peak, and inspect whether its bounds are infinite/off-screen. Use [graphical-bound instructions](drag-controls.md#set-position-bounds-by-dragging-lines). |
| Zoom changes but the fitted interval does not | Viewing range, fitting domain and peak-position bounds are three separate settings. See [fit-domain controls](fitting-and-results.md#separate-the-viewing-range-from-the-fitting-range). |
| An edge position is not editable independently | It may follow a peak. Inspect the chosen edge in [Link/Unlink edge to μ](peak-constraints.md#link-the-first-ionisation-edge-to-a-peak). |
| Edge handles are missing or the slope will not drag | Select a visible edge, turn edge subtraction off, and enable Free for post_slope if needed. See [edge drag controls](drag-controls.md#drag-ionisation-edge-parameters). |
| A dragged peak moves past its former μ limits | Position drags translate its interval; release and review Min/Max. Fixed states and links remain enforced. |
| Components are faint, shifted or absent | Check Focus Mode, component visibility and Offset components. Those controls change the view, not fitted contributions. |
| No Total or residual is visible | Total appears for peaks/edges; residuals require data plus peaks, edges or linear background. Gain/dE alone do not create those components. |

[Display settings](component-display.md) also explain edge subtraction and
sequential comparison colours. A group name or common colour does not create
parameter sharing, scale factors or constraints.

## Fits, series stages and exports

| Symptom | Check and next action |
| --- | --- |
| Fit reaches its evaluation limit | Check the channel, domain, model, bounds and links before increasing the budget. Use [Fit settings](fitting-and-results.md#set-the-evaluation-budget-and-tolerances); the budget counts residual evaluations. |
| A fit converges but seems implausible | Inspect residual structure, bound hits, compensating parameters and sensitivity to sensible seeds. Convergence alone does not validate a scientific model. |
| Series trends cannot be enabled for a parameter | Trends require an independent per-spectrum parameter and simultaneous fitting. Shared parameters already have one value; linked targets use their controlling source/separation. |
| Strict trends report infeasibility | Check order/coordinates, target membership, fixed values and intersected bounds. A named extremum must be among the selected targets. |
| Pass two is unavailable | Every target needs a successful fitted pass one. Copy-only output, changed first-pass settings or incomplete targets do not supply those seeds. |
| Pass-two seed or bounds are rejected | Review the [averaging pool, seed and scope](two-pass-fitting.md). Means must be legal for each target; global parameters need a common seed. |
| Series output is missing from the main plot | Staging retains working scans. Review the intended pass and Apply successful results, then return to that target channel. |
| CSV differs from the edited working curve | CSV uses the last successful fitted snapshot, not later trial edits or display transforms. See [export scope](exporting-fits.md). |
| CSV selection omits a scan/channel | Select the row's intended Detector and ensure it has an applied successful fit with a saved fitted-model snapshot. |

Single fits now have a **Stop fit** control in their progress window. A stopping
request waits for the active evaluation to return; trial parameters are discarded.
Collection workflows request a safe stop and wait for their worker. Do not close
a running worker by force. [Series fitting](series-fitting.md) and [two-pass review](two-pass-fitting.md#stage-select-and-apply-the-intended-pass)
explain staging, invalidation and application.

If a staged scan failed, its spectrum/residual preview is cleared and labelled.
Use [parameter comparison](series-fitting.md#inspect-residuals-and-compare-parameter-trends)
to inspect gaps and bound hits across the set. After an application interruption,
use the [recovery choice](sessions-and-models.md#recover-interrupted-work); an old
checkpoint is a partial checkpoint in time, not confirmation that later work was saved.

## Keep a useful issue report

Record the application version from **Help → About…**, the operation, scan/channel/
stage, relevant settings and exact message. Messages → History retains recent full
workspace feedback; modal import warnings may need recording separately. It is
different from the model Undo/Redo history, and neither history persists on restart.

Keep an unedited source acquisition and a complete session if possible. State
whether the issue arose before Preview, after Preview, after Apply, during fitting
or when exporting. If you supply a screenshot, include the relevant selection and
diagnostic. Review files for research information before sharing them.

## Related topics

- [Work from a folder to reviewed fit reports](../examples/beamline-workflow.md).
- [Diagnose import-specific errors](import-diagnostics.md).
- [Distinguish data, view and fitting state](component-display.md).
- [Save a reproducible checkpoint](sessions-and-models.md).
- [Interpret bounds, sharing and fit statistics](../reference/constraints.md).
