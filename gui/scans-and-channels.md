# Managing scans, channels and comparisons

These instructions describe GUI **0.2.30**. The browser manages independent
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

(import-files-or-a-folder)=

## Read the scan table

First [import files or a folder](importing-data.md). This page describes the
resulting browser table and independent channel workspaces.

### Arrange the scan-table columns

File and comment columns start compact. Drag any column-header boundary to resize them;
widths remain as adjusted while inspecting scans. Scan number is omitted from
this table, but remains available in retained metadata. Right-click column headings
to show/hide columns; **Notes** starts hidden.

### Read the set-energy range

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

### Return to a channel's working model

Every visited scan/channel retains its own model, fitting range, prepared signal
and last successful fit. Returning restores that channel's work. A newly visited
channel starts with a copy of the current model. Switching scans preserves working
Undo/Redo histories; those histories are not persisted across application restarts.
**Save session** retains the collection, channel work, results and active selection.

After repreparation, cached channels that could not be replaced can be labelled
**previous preparation**. This is distinct from an apply-ready preparation preview:
inspect its status and prepare/refit the intended channel before using old results.

## Compare scans without changing fit targets

See [Compare scans without changing fit targets](comparing-scans.md#compare-scans-without-changing-fit-targets) for this procedure.

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

See [Attach notes to a scan](scan-notes.md#attach-notes-to-a-scan) for this procedure.

## Related procedures

### Review energy order

If import opens an ordering panel, follow [Review and sort measured energies](energy-order-review.md).

## Related topics

- [Prepare one scan or an acquisition series](preparing-data.md).
- [Diagnose import failures and review energy order](import-diagnostics.md).
- [Inspect metadata and measurement angles](metadata.md).
- [Choose colours, edge subtraction and component offsets](component-display.md).
- [Fit the active displayed channel](fitting-and-results.md).
- [Select targets for propagation or simultaneous fitting](series-fitting.md#set-up-a-first-series-fit).
- [Save the collection and its channel work](sessions-and-models.md).
- [Use these selections in a complete acquisition workflow](../examples/beamline-workflow.md).
