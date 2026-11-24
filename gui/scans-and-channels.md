# Managing scans, channels and comparisons

These instructions describe GUI **0.2.27**. The browser manages independent
acquisitions; selecting a trace is separate from selecting preparation or fit targets.

The standard large-window split reserves six scan rows. Compact initially uses
two; particularly short windows hide the browser until requested. See
[workspace layouts](layout-and-small-screens.md). Drag the dock
divider to give the table or plot more space; the initial sizing is applied once,
so later resizing is retained during that window's use.

:::{figure} ../media/images/six-scan-workspace-preview.png
:label: figure-six-scan-layout
:alt: Main window showing six complete scan rows above the resizable data plot.

The default split shown with six synthetic acquisitions. The dock divider remains
adjustable when inspecting larger collections or giving the plot more space.
:::

## Import files or a folder

**Data import → Import** adds one file without discarding the scans already loaded.
Native SXR `.asc` files use NEXAFSFit's channel/header importer. The original
source bytes, header, native dictionary and monitor channels remain retained.
Channeltron is the usual initial detector; choose another mapped signal afterward.

For ordinary text/CSV, select delimiter, header rows and distinct energy/intensity
columns explicitly. This imports one intensity signal; unrelated CSV columns do
not automatically become native detector or reference channels.

**Load folder** reads native `.asc` files in the chosen directory, without recursing
into subdirectories. Import the PD acquisition with the sample scans if needed.
The table starts with filename, comment, **Overlay**, **Set energy (eV)** and
**Incidence angle** (fifth). Detector and Preparation are adjacent, followed by
raw sample position, Points and Acquired. Invalid files are reported by filename while
valid files remain loaded. Cancelling stops between files and retains those already
read. Exact repeats of a retained native filename/content are skipped.

File and comment columns start compact. Drag any column-header boundary to resize them;
widths remain as adjusted while inspecting scans. Scan number is omitted from
this table, but remains available in retained metadata. Right-click column headings
to show/hide columns; **Notes** starts hidden.

**Set energy** is the minimum–maximum of the original **Mono Setpoint**, column 2
in native SXR files. It uses recorded setpoints for acquired points, independently
of measured-energy jitter, foil alignment or fit/view limits. Missing/nonfinite
setpoints show `—`; text/CSV import does not substitute its energy column.
See [raw sample position and units](metadata.md#raw-sample-position).

:::{figure} ../media/images/scan-details-preview.png
:label: figure-scan-details
:alt: Detached scan panel with compact file/comment columns, Overlay third, set-energy fourth, incidence fifth, adjacent Detector/Preparation and Points before Acquired.

Synthetic scans in the detached browser. Notes are hidden initially; right-click
a row to edit notes or a heading to choose visible columns.
:::

## Review energy order

A fit needs finite, strictly increasing measured energies. An SXR scan with
backward steps opens **Review measured-energy order**. Compare acquisition-order
and sorted previews, then choose **Import sorted copy** or skip that scan.

Sorting keeps every measurement paired with its detector values; it does not
average, smooth or discard points, and the original acquisition order/source stays
retained. Exact duplicate energies and nonfinite data are not repaired by sorting.
No sorting runs merely because a scan is opened or selected.

See [import diagnostics](import-diagnostics.md) for the review dialog, unresolved
errors, text-column import checks and partial folder-import failures.

## Select each scan's detector independently

Click a filename/comment to make that row active. Its **Detector** dropdown selects
its displayed channel. The active-channel shortcut changes only that row.
Changing an inactive row's Detector does not activate it or change other rows.

| Choice | What is displayed |
| --- | --- |
| Raw channel | Original counts on measured energy |
| Aligned counts | Original counts on the acquisition's calibrated energy axis |
| I0-normalised | Retained detector/I0 signal |
| I0/PD-corrected | Retained detector/I0 divided by the separate PD response |
| Prepared | The selected final baseline/scale result, or corrected/raw signal if intensity preparation was disabled |

Later stages appear only after explicit [preparation](preparing-data.md). Switching
to a stage reads stored arrays; it does not rerun processing. I0 and beam-current
monitors remain available as raw/aligned counts rather than being normalised by
themselves. An unavailable stage/channel is reported instead of substituted.

Every visited scan/channel retains its own model, fitting range, prepared signal
and last successful fit. Returning restores that channel's work. A newly visited
channel starts with a copy of the current model. Switching scans preserves working
Undo/Redo histories; those histories are not persisted across application restarts.
**Save session** retains the collection, channel work, results and active selection.

After repreparation, cached channels that could not be replaced can be labelled
**previous preparation**. This is distinct from an apply-ready preparation preview:
inspect its status and prepare/refit the intended channel before using old results.

## Compare scans without changing fit targets

Check row **Overlay** boxes or use **Overlay all scans**. Each overlay uses that
row's Detector, so a PD acquisition can show Photodiode Direct while a sample
shows prepared Channeltron. The active curve is slightly thicker; all overlaid
traces are lines labelled with filename, channel and stage. **Clear overlays**
returns to the active spectrum's point display.

Overlays do not resample or comparison-normalise signals. A raw monitor can have
a very different intensity scale from a prepared detector: choose compatible stages
when comparing trends. [Comparison colours](component-display.md#change-component-and-comparison-colours)
are editable through **Line colours…**. For model-based comparison views, see
[edge subtraction](component-display.md#subtract-current-edges).

Default comparison colours follow a sequential Viridis gradient across the loaded
scan-table order. Early scans are purple, then blue/teal, then green. Active and
inactive traces use the same mapping; toggling overlays does not redistribute it.
Adding scans can redistribute default colours across the larger collection.
Recorded angles are not used to silently sort scans. Individually assigned colours
override the gradient and remain saved with the session.

| Operation | Which data it uses |
| --- | --- |
| Main plot overlays | Each checked row's own Detector selection |
| Fit spectrum | Active row's displayed channel only |
| Prepare scans | Explicitly selected native acquisitions; recipe prepares all mapped detectors |
| Propagate model / fit | Explicit targets on the source channel/stage, including available cached channels |
| CSV export | Explicit rows' currently selected channels with retained successful fits |

## Inspect metadata and incidence angle

The **Metadata** tab searches retained fields and shows their source/PV identifiers.
For the identified R1 readback, incidence is R1 − 45° from the sample surface.
The [metadata guide](metadata.md) explains the retained fields, angle-source
mapping, conventions and troubleshooting missing or conflicting values.

## Detach or hide the browser

**Detach** opens Scans/Metadata as a separate window; **Attach** restores it.
Dragging/double-clicking its title bar also changes docking. Closing it hides it;
**Scans / metadata** reopens it. None of these actions unloads data or changes a
model. Right-click table headers to choose visible columns during inspection.
Browser mapping/tab/docking choices are retained in complete sessions.
The **View** menu also reopens hidden panels. Components and Parameters have their
own docking/tab controls; [layout instructions](layout-and-small-screens.md#show-hide-detach-or-tab-panels)
explain their placement and reset commands.

## Attach notes to a scan

Right-click a scan row and choose **Edit scan notes…**. Write observations or fitting
reminders, then **Save** to attach the text to that scan; **Cancel** discards the
dialog edits. Notes belong to the acquisition across all Detector channels and
remain attached when preparation or reviewed series fits are applied.

To inspect them in the table, right-click a column heading and enable **Notes**.
Hover for full text or double-click its cell to edit. Notes are plain text and do
not replace the recorded comment/label or modify the acquisition file.

Use **Session → Save session** to persist notes with the collection, then **Open
session** to restore them. Model-only JSON and fitted-result CSV commands do not
carry scan notes. Notes are not model edits in the Undo history. See
[session checkpoints](sessions-and-models.md).

## Related topics

- [Prepare one scan or an acquisition series](preparing-data.md).
- [Diagnose import failures and review energy order](import-diagnostics.md).
- [Inspect metadata and measurement angles](metadata.md).
- [Choose colours, edge subtraction and component offsets](component-display.md).
- [Fit the active displayed channel](fitting-and-results.md).
- [Select targets for propagation or simultaneous fitting](series-fitting.md#set-up-a-first-series-fit).
- [Save the collection and its channel work](sessions-and-models.md).
- [Use these selections in a complete acquisition workflow](../examples/beamline-workflow.md).
