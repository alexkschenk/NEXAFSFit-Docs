# Edit parameter values, bounds and states

GUI **0.2.30**. First [add or load components](model-editor.md). Select a component to inspect its coloured group in the parameter table.

(values-bounds-free-and-sharing)=

## Values, bounds, Free and Sharing

The table groups rows under coloured component headers.

:::{figure} ../media/images/parameter-table-controls.png
:label: figure-parameter-table-controls
:width: 560px
:alt: Grouped parameter table with global corrections, background, edge and selected Gaussian rows, showing Value, Min, Max, Free and Sharing columns.

Parameters of an existing synthetic model. Coloured headers identify components;
Free and Sharing are separate controls. The displayed bounds belong to that model,
not necessarily the new-peak defaults listed below. Scroll to reach later rows
or columns.
:::

New peaks start with the following bounds (energies in eV):

| Parameter | Default Min | Default Max |
| --- | --- | --- |
| μ | Initial μ − 1 | Initial μ + 1 |
| σ (Gaussian, Voigt and EMG) | 0.1 | 5 |
| τ (EMG) | 0 | 2 |

These are starting bounds, editable in the table or with the applicable drag
controls. Dragging an independent peak position translates its Min/Max interval
with it, retaining the original distances from μ. Numerical value edits do not
translate bounds. Existing models and duplicated peaks retain their configured
bounds until explicitly edited. In **Add multiple peaks…**, an
explicit starting σ or τ outside the default interval extends the necessary
endpoint to include that value; ordinary starting values keep the defaults.

| Field | Meaning |
| --- | --- |
| Value | Current model value and initial value for the next fit |
| Min / Max | Hard limits on an independent fitted parameter |
| Free checked | Allow the optimiser to vary that parameter |
| Free unchecked | Hold the parameter fixed |
| Sharing | `global` or `per_spectrum` for simultaneous series fits |
| Relations / notes | Generated links, area conventions or static specifications |

### Fixed, linked and static values

The [series scope guide](series-fitting.md#choose-targets-and-a-strategy) explains
when Sharing affects fitting. [Parameter-state definitions](../reference/constraints.md#four-different-parameter-states)
distinguish bounds, fixed values, linked targets and static specifications.

:::{important} Fix a value with Free, not equal bounds
A fixed parameter still needs a valid ordered interval; do not make Min equal
Max to fix it. Clear **Free** instead.
:::

A linked target is calculated from another
parameter and cannot be edited independently. Its previously stored independent
bounds/settings remain inactive until the link is removed.

Lorentzian/Voigt `gamma` is a static specification: edit its value, but it is not
optimised through a Free checkbox. See [lineshapes and parameters](../reference/lineshapes.md).

## Related topics

- [Arrange parameters with plot handles](drag-controls.md).
- [Link peak parameters](peak-constraints.md).
- [Choose sharing for a series](series-fitting.md).
