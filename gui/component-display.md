# Component visibility, focus and colours

These instructions describe GUI **0.2.30**. Use the component list, its **Actions…**
menu and the expandable **Display options** below the main plot to change the view. These
controls help inspect a model while its numerical parameters remain separate.
If a panel is hidden, use [View and workspace layouts](layout-and-small-screens.md).
If you have not added components yet, start with [model construction](model-editor.md).

## Show, hide and focus components

:::{important} Hiding a curve leaves its model term active
The checkbox beside a component hides or shows its curve. It does **not** remove
that component from the model: it still contributes to Total, residuals, fitting
and fitted-curve exports. Use **Remove** when you mean to remove the numerical term.
:::

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

## Save display choices and check the numerical result

Complete sessions retain per-scan/channel focus, subtraction, offset/factor and
fit-window/bound visibility choices. Component colours/curve visibility are model
display metadata; comparison colours and view preferences belong to the session.
Use [Save session](exporting-fits.md#choose-what-to-save) when you want to reopen
the complete visual working state.

Before interpreting an unusual component, inspect its table parameters and the
unchanged residual. These display tools aid comparison; they do not impose a
constraint, normalise data or establish an experimental component assignment.

## Related procedures

### Subtract current edges

See [Subtract current edges](edge-subtraction.md#subtract-current-edges) for this procedure.

### Offset components

See [Offset components](component-offsets.md#offset-components) for this procedure.

## Related topics

- [Use shifted peak handles and set bounds](drag-controls.md).
- [Add numerical edges, background and global corrections](model-editor.md#add-edges-and-global-corrections-explicitly).
- [Prepare or normalise the working signal](preparing-data.md).
- [Interpret the unchanged residual](fit-results.md#read-more-than-the-convergence-flag).
- [Save visual work or export numerical curves](exporting-fits.md).
- [Diagnose missing or immovable components](troubleshooting.md#component-controls-and-fit-limits).
