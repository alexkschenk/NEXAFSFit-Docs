# Saving work and exporting fits

:::{note} Implemented and tested — GUI 0.2.24
CSV export has been checked against the numerical evaluator, including all four
peak shapes, parameter links, multiple edges, gain/scaling and restricted fit
ranges. The screenshot below uses synthetic data. See the
[first-fit tutorial](getting-started.md) for a complete introductory workflow.
:::

## Choose what to save

| Command | Saved content | Use it for |
| --- | --- | --- |
| Session → Save session | Loaded scans, retained raw/prepared channels, working models and successful fit results | Resuming editable work |
| Model → Save model | Current model values, bounds, states, components and constraints | Reusing a model with different data |
| Export → Fitted curves CSV… | Selected channels' last successful fitted curves and measured data | Comparing or plotting numerical results |
| Export → Fit parameters CSV… | Selected channels' last successful fit parameters, states and statistics | Numerical analysis in other tools |

**Open model** [reuses a saved model](model-editor.md#reuse-a-saved-model-on-current-data)
on the loaded spectrum without replacing its acquired data.
[Open session](sessions-and-models.md) restores a saved workspace. Save a session
before changing preparation if you need a persistent checkpoint of earlier work.
The [control map](getting-started.md#find-the-controls) and
[graphical editing guide](model-editor.md#graphical-editing) describe the interface;
this chapter explains saved reports and their interpretation.

## Export the active fit or selected series scans

1. Fit a spectrum, or Apply successful results from the series workflow.
2. Open the **Export** menu and choose **Fitted curves CSV…** or **Fit parameters CSV…**.
3. The active scan is initially checked if it has a successful fit. Check any
   other desired rows, or choose **Select successful fits**. Uncheck rows to omit
   them. Scans without a saved fit are unavailable.
4. Check the **Detector / stage** column. Each scan exports its own currently
   selected detector channel. To export another channel, select it in that
   scan's Detector dropdown before opening export. Cached fits for other channels
   are retained but are not silently included.
5. Click **Export CSV…**, choose a filename and save. Each menu command writes its
   own CSV; choose both export commands if you need both tables.

:::{figure} ../media/images/csv-export-preview.png
:label: figure-csv-scan-selection
:alt: CSV export dialog with an unavailable unfitted scan and a checked successful fit, detector and scope columns, and Export CSV and Cancel buttons.

Choose which retained fits to include. Exporting leaves working scans and models intact.
:::

The export uses the **last successful fitted model**, separately from subsequent
parameter edits. Last fit results shows the corresponding statistics. A failed
refit retains that earlier successful result. Changing preparation invalidates
affected results; refit those channels before exporting. An older fit record
without a saved model snapshot needs a new fit before it can be exported.
Opening a different model onto the data clears the active fit statistics; fit that
model again before exporting it. Undoing model import restores the model, not a
claim that the imported model was already fitted.

CSV files use UTF-8 with a byte-order mark for Windows spreadsheet applications.
Comments containing commas, Unicode or newlines are quoted. A cancelled or failed
write preserves an existing output file. Complete editable work remains in
**Save session**; CSV files are numerical reports rather than restorable sessions.

Reference-calibration traces and their inspection records are preparation data,
retained in complete sessions after a successful Preview is applied. The fitted-
curve and parameter CSV commands report the selected spectra's successful model
fits; they do not export a reference-calibration report.

## Fitted curves CSV

Each row contains one measured energy point. Selected scans are appended in scan
table order, with identifiers repeated so the rows can be separated later.

| Columns | Meaning |
| --- | --- |
| `scan_id`, `scan_label`, `comment`, `source_file` | Scan-table row number and source identity |
| `detector`, `channel_stage` | The selected signal and its retained preparation stage |
| `energy_eV`, `data` | Full loaded/prepared fitting arrays |
| `fit_total` | Envelope from the saved fit, including all corrections/components |
| `residual` | `data - fit_total` |
| `in_fit_range` | `true` for points within the saved fit's domain, otherwise `false` |
| `baseline`, `edge_total` | Full-grid linear background and summed ionisation edges |
| `peak:<name>` | Individual peak contribution, including gain and component scaling |
| `edge:<name>` | Each edge contribution; the first edge is `edge:primary` |
| `group:<name>` | Sum of peak contributions in the named group |

The full grid is exported even for a restricted fit. This preserves the engine's
background reference; use `in_fit_range` to select the points actually fitted.
Zoom, Focus Mode, component visibility, visual component offsets and Subtract
current edges do not alter exported arrays. `energy_eV` is the data energy axis;
any fitted `dE` remains part of the evaluated model.

If selected scans have different components, the CSV contains the union of their
component columns. A blank component cell means that component is absent in that
scan's model. Group sums overlap with individual peak columns; do not add group
sums to the peaks a second time when reconstructing the envelope.

## Fit parameters CSV

This is a long table with one parameter per row, carrying the same scan/channel
identifiers. It includes component name, lineshape and peak group, fitted value,
standard error, bounds, varying/fixed/linked state, generated relationship text,
bound indication and sharing scope. Generated expressions are reported as text;
loading a CSV does not run them.
For the meaning of these states and joint versus per-scan statistics, see
[bounds, sharing and fit statistics](../reference/constraints.md).

`parameter_kind` distinguishes **varying**, **fixed**, **linked** and **static**.
Lorentzian/Voigt `gamma` is included as a static specification, not as an
optimised width. Unused EMG gamma is excluded. A missing standard error is blank,
not zero. Infinite bounds are written as `-inf` and `inf`.

Peak `A` is intrinsic **integrated area**, not height. Exported peak curves also
include gain and optional component scaling. EMG `mu` is not necessarily the
position of its maximum. Preserve these distinctions when analysing the CSV.

The table includes the saved fitting range, evaluation count and fit statistics.
For simultaneous fits, `statistics_scope` is **simultaneous series**: `chisqr`,
`redchi` and `ndata` describe the joint optimisation. `spectrum_chisqr` and
`spectrum_ndata` describe the individual scan. Strict trend fits have no covariance
estimate, so their standard errors remain blank.

## Related topics

- [Select which detector/stage a scan exports](scans-and-channels.md#select-each-scans-detector-independently).
- [Read the retained fit and its diagnostics](fitting-and-results.md).
- [Review and apply staged series results](series-fitting.md#review-staged-results-before-applying).
- [Interpret parameter states and statistic scopes](../reference/constraints.md).
- [Save and reopen complete sessions or reusable models](sessions-and-models.md).
- [Check why an expected fit is unavailable or differs from the working plot](troubleshooting.md#fits-series-stages-and-exports).
