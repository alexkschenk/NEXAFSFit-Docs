# Add peaks individually or in a batch

GUI **0.2.30**. Choose a shape in **Component controls**. Start from [model construction](model-editor.md) if no model is present.

## Add one peak

Use **Add peak**, double-left-click in the main data axes, or right-click
and choose **Add component here**. Then review its [parameters](parameter-table.md).

### Place a peak with the mouse

Peak names must be unique Python identifiers. With loaded data, double-left-click
and right-click placement use the clicked energy, while the initial height is estimated from
the spectrum above the current continuum; the vertical click position is not a
separate height request. For EMG, native μ placement can differ from its maximum.
Each main-plot double-left-click adds a new peak and selects it, without moving
existing peaks. Undo removes that addition. Double-clicks on the axes or residual
panel do not add peaks.

(add-several-peaks-in-one-operation)=

## Add several peaks in one operation

Open **Add multiple peaks…**. Set Count, First/Last μ and Shape, then **Generate
rows** to create evenly spaced candidates. Alternatively, **Clear rows**, choose
a shape and double-left-click the data preview above the table to place peaks.
Click placement preserves existing rows and edits; Generate rows replaces them.
Single clicks and double-right-clicks do not append peaks. Up to 200 can be staged.

### Choose starting parameters and automatic areas

Edit names, μ, shapes and the relevant sigma/tau/static-gamma cells. Leave Area A
blank or `auto` to match the data above the **current edges and linear background**.
Existing peaks are not subtracted when choosing a new peak's height. A nonpositive
height above that continuum gives zero nonnegative area. Outside the loaded data,
enter an explicit area instead of using auto.

### Preview and commit the batch

**Preview initial areas** shows the calculated A values. Labels/markers follow
the pending rows. **Add peaks** validates the whole batch and adds it as one Undo
step; Cancel leaves the model unchanged. The preview retains the main plot's view
and edge-subtracted state. Its wheel follows the same energy limits/lower-edge
anchor; zooming does not edit candidate parameters. For EMG, placement specifies
native μ at the clicked displayed energy, which need not be its observed maximum.

:::{figure} ../media/images/batch-peaks-preview.png
:label: figure-batch-peaks
:alt: Add multiple peaks dialog with the current data above an editable candidate table and three labelled position markers.

Place candidates visually, then review their numerical starting parameters before adding them.
:::

## Related topics

- [Edit initial bounds and fixed states](parameter-table.md).
- [Drag an existing component](drag-controls.md).
- [Add optional edges and corrections](model-editor.md#add-edges-and-global-corrections-explicitly).
