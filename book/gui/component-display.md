# Component display, edge subtraction and offsets

These instructions describe GUI **0.2.27**. Use the component list, its **Actions…**
menu and the expandable **Display options** below the main plot to change the view. These
controls help inspect a model while its numerical parameters remain separate.
If a panel is hidden, use [View and workspace layouts](layout-and-small-screens.md).
If you have not added components yet, start with [model construction](model-editor.md).

## Show, hide and focus components

The checkbox beside a component hides or shows its curve. It does **not** remove
that component from the model: it still contributes to Total, residuals, fitting
and fitted-curve exports. Use **Remove** when you mean to remove the numerical term.
Selecting a hidden peak will not reveal its drag handles until its curve is shown.

**Focus Mode**, initially on, fades unselected peaks, edges and background. Click
a curve, component entry or parameter-table header to select it. Turn Focus Mode
off to restore full opacity to the visible components. Hidden checkboxes remain
hidden; Focus Mode does not override them. Data, Total and residual are not faded.

| Display control | Use |
| --- | --- |
| Component-list checkbox | Hide/show that curve without deactivating the term |
| Focus Mode | Fade unselected visible components |
| Show fit window | Hide/show the fitting-domain graphic, preserving its limits |
| Show μ bounds | Hide/show finite position-bound handles for the selected independent peak |
| Subtract current edges | Inspect data/envelope after visual removal of current model edges |
| Offset components and factor | Move individual components/handles down for comparison |

See [drag controls](drag-controls.md) for the distinction between handles, viewing
range and fitting/parameter limits.

## Change component and comparison colours

1. Select a component and open **Actions…** beside Components, or right-click the
   component list.
2. Use **Colour selected component…** for that component, or **Line colours…**
   for the envelope, peaks, edges, background and loaded comparison scans.
3. In Line colours, select multiple entries with Ctrl/Shift to assign a common
   colour for visual grouping, then Apply.

Total defaults to red. The parameter table's component headers match the component
colours. A common colour does not create a peak group, link parameters or assign
Sharing; use the [model/group controls](model-editor.md#organise-groups-and-line-colours)
for those choices. Model colour edits support model Undo and are retained with
models/sessions. Comparison colours are per-scan view choices saved in sessions.
Their default is a Viridis gradient in loaded scan order, distinct from the
component palette. Unchanged default colours stay automatic when applying Line
colours; individual edits override them. Preparation comparison overlays use the
same sequential map across their listed scans, with matching raw/prepared colours.

:::{figure} ../media/images/overlay-series-preview.png
:label: figure-sequential-overlays
:alt: Eight synthetic spectra progressing from purple through blue and teal to green in loaded scan order.

Sequential colours distinguish positions in a synthetic scan series. The order
comes from the loaded table, without inferring a scientific trend or sorting by angle.
:::

## Subtract current edges

Use **Subtract current edges**, a checkbox below the plot beside Focus Mode, to
see where peaks sit above the model continuum.

1. [Add an ionisation edge](model-editor.md#add-edges-and-global-corrections-explicitly) if needed and give it sensible position/width/height/slope
   values. It can be a working estimate; no successful fit is required.
2. Check **Subtract current edges**.
3. Inspect the data and total above the current edges; adjust components as needed.
   Updating edge parameters updates the display immediately.
4. Uncheck it to restore the ordinary data/envelope view.

The display subtracts the **sum of every current model edge**, including additional
edges and terms whose display checkbox is hidden. The same edge sum is subtracted
from data and Total, and the edge curves themselves are hidden in this view.
Peak and linear-background curves are unchanged by edge subtraction. It does not
subtract the linear background or the other peaks. With no edges, or a zero edge
contribution, there may be no visible change.

Edge drag controls are hidden along with edge curves. Turn subtraction off to use
the [edge handles](drag-controls.md#drag-ionisation-edge-parameters).

The residual and optimiser continue using the original active working spectrum
and complete model. Raw arrays and prepared arrays are unchanged. This is a
**display of current model subtraction**, not a new prepared detector channel or
a permanently edge-corrected dataset. Fitted-curve CSVs retain their numerical
data/envelope/edge columns independently of this toggle.

For overlays, each scan uses the edges from its own displayed channel's working
model. Overlaying scans does not apply the active model's edge to every scan.
To make an edge follow a peak, use [edge-to-peak linking](peak-constraints.md#link-the-first-ionisation-edge-to-a-peak).
For operations that change the actual working signal, see [scan preparation](preparing-data.md).

## Offset components

Check **Offset components** on the same display row, then adjust its numeric factor.
This shifts the individual peak, edge and background curves downward, together
with their peak handles. A dotted **Component zero (offset)** line marks their
shifted zero; a curve below ordinary zero does not imply that its fitted area
became negative.

The shift is

```{math}
:label: equation-component-display-offset
\Delta I = f\,\max\!\left(\max(y)-\min(y),\;\max|y|\right),
```

where $f$ is the factor (initially **0.1**) and $y$ is the active working signal.
Previously saved explicit factors are restored. For a signal ranging from −0.2
to 1.8, the scale is 2.0; factor 0.1 shifts components down by 0.2 intensity unit.
Increasing the factor moves them farther
down. Factor zero gives no shift, and unchecking the toggle restores normal positions.
When no data are loaded, the model preview supplies the scale.
For an all-zero signal, the renderer uses a scale of 1.

Data, comparison scans, Total and residual do not move. Parameters and numerical
fits/exports are unchanged. The vertical view expands to include the shifted
curves while horizontal framing is retained. Drag the shifted handles normally:
the GUI removes the visual shift before converting their height back into A.
The [drag-control screenshot](drag-controls.md#select-a-component-and-expose-its-controls)
shows this arrangement.

You can combine offset components with edge subtraction. The total/data then
show the edge-subtracted view while individual component curves are displaced
below them. Focus Mode can still fade unselected components in either view.

## Save display choices and check the numerical result

Complete sessions retain per-scan/channel focus, subtraction, offset/factor and
fit-window/bound visibility choices. Component colours/curve visibility are model
display metadata; comparison colours and view preferences belong to the session.
Use [Save session](exporting-fits.md#choose-what-to-save) when you want to reopen
the complete visual working state.

Before interpreting an unusual component, inspect its table parameters and the
unchanged residual. These display tools aid comparison; they do not impose a
constraint, normalise data or establish an experimental component assignment.

## Related topics

- [Use shifted peak handles and set bounds](drag-controls.md).
- [Add numerical edges, background and global corrections](model-editor.md#add-edges-and-global-corrections-explicitly).
- [Prepare or normalise the working signal](preparing-data.md).
- [Interpret the unchanged residual](fitting-and-results.md#read-more-than-the-convergence-flag).
- [Save visual work or export numerical curves](exporting-fits.md).
- [Diagnose missing or immovable components](troubleshooting.md#component-controls-and-fit-limits).
