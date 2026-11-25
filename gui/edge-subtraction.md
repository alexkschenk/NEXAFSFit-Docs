# Inspect spectra with current edges subtracted

GUI **0.2.30**. Use this main-plot display mode after [adding edges](model-editor.md#add-edges-and-global-corrections-explicitly). It does not prepare or replace the working signal.

(subtract-current-edges)=

## Subtract current edges

Use **Subtract current edges**, a checkbox below the plot beside Focus Mode, to
see where peaks sit above the model continuum.

1. [Add an ionisation edge](model-editor.md#add-edges-and-global-corrections-explicitly) if needed and give it sensible position/width/height/slope
   values. It can be a working estimate; no successful fit is required.
2. Check **Subtract current edges**.
3. Inspect the data and total above the current edges; adjust components as needed.
   Updating edge parameters updates the display immediately.
4. Uncheck it to restore the ordinary data/envelope view.

:::::{tab-set}
::::{tab-item} Ordinary view

:::{figure} ../media/images/model-display-ordinary.png
:label: figure-edge-view-ordinary
:alt: Ordinary synthetic spectrum with data and Total above the current edge, separate peak and edge curves, and unchanged residual below.

The synthetic spectrum before display subtraction. The edge raises the
high-energy data and envelope above the component zero.
:::
::::
::::{tab-item} Edges subtracted

:::{figure} ../media/images/model-display-edge-subtracted.png
:label: figure-edge-view-subtracted
:alt: The same spectrum with Subtract current edges checked, data and Total lowered by the current edge contribution, hidden edge curve and unchanged residual.

The same model and arrays with **Subtract current edges** checked. The data and
Total lose the current edge contribution visually; the residual is unchanged.
:::
::::
:::::

The display subtracts the **sum of every current model edge**, including additional
edges and terms whose display checkbox is hidden. The same edge sum is subtracted
from data and Total, and the edge curves themselves are hidden in this view.
Peak and linear-background curves are unchanged by edge subtraction. It does not
subtract the linear background or the other peaks. With no edges, or a zero edge
contribution, there may be no visible change.

Edge drag controls are hidden along with edge curves. Turn subtraction off to use
the [edge handles](drag-controls.md#drag-ionisation-edge-parameters).

:::{important} Edge subtraction is a display setting
The residual and optimiser continue using the original active working spectrum
and complete model. Raw arrays and prepared arrays are unchanged. This is a
**display of current model subtraction**, not a new prepared detector channel or
a permanently edge-corrected dataset. Fitted-curve CSVs retain their numerical
data/envelope/edge columns independently of this toggle.
:::

For overlays, each scan uses the edges from its own displayed channel's working
model. Overlaying scans does not apply the active model's edge to every scan.
To make an edge follow a peak, use [edge-to-peak linking](edge-links.md#edge-links-position).
For operations that change the actual working signal, see [scan preparation](preparing-data.md).

## Related topics

- [Change component visibility and focus](component-display.md).
- [Offset components for inspection](component-offsets.md).
- [Fit and review the unchanged numerical model](fitting-and-results.md).
