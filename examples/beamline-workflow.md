# From a scan folder to reviewed fit reports

GUI **0.2.30**. Use this route through the task guides with your own supported
native SXR acquisitions. Each linked page owns the detailed procedure; this
checklist connects the stages without prescribing an experimental model.

## 1. Load and identify the acquisitions

[Import the folder](../gui/importing-data.md), including a separate PD acquisition
if needed. Resolve any [ordering reviews](../gui/energy-order-review.md) and
[import failures](../gui/import-diagnostics.md). Inspect [metadata and angles](../gui/metadata.md),
record [scan notes](../gui/scan-notes.md), and save an initial session.

## 2. Inspect channels before processing

[Choose each scan’s detector](../gui/scans-and-channels.md) and
[compare compatible signals with overlays](../gui/comparing-scans.md).

:::{important} Comparisons and analysis have separate selections
Overlay checkboxes do not select preparation or series-fit targets. Review the
target list in the workflow you are about to apply.
:::

## 3. Preview preparation for the intended sample scans

[Preview the preparation recipe](../gui/preparing-data.md). Use the dedicated
guides for [reference inspection](../gui/reference-calibration.md),
[photodiode correction](../gui/photodiode-correction.md) and
[normalisation windows](../gui/normalisation-windows.md). Review selected scans
before Apply, then save a prepared-session checkpoint.

## 4. Select the prepared fitting channel and build a model

Choose the prepared channel, then [build a model](../gui/model-editor.md) or
[open a model template](../gui/model-files.md). Use [peak placement](../gui/adding-peaks.md),
[parameter states](../gui/parameter-table.md), [drag controls](../gui/drag-controls.md),
[peak constraints](../gui/peak-constraints.md) and [edge links](../gui/edge-links.md)
as required by the model assumptions.

## 5. Fit and review one spectrum

[Set the domain and fit](../gui/fitting-and-results.md), changing
[numerical settings](../gui/fit-settings.md) deliberately. [Review the result](../gui/fit-results.md)
before propagation. Record observations and save the reviewed model/session.

## 6. Optionally propagate the model or fit the series

[Set targets and a strategy](../gui/series-fitting.md), then configure
[reference-relative bounds](../gui/series-constraints.md),
[trends/order](../gui/series-trends.md) or [pass-two seeds](../gui/two-pass-fitting.md)
when justified. [Review staged results](../gui/series-review.md) before applying.

## 7. Save editable work and export reports

[Save the complete session](../gui/sessions-and-models.md) after applying results.
[Export fitted curves and parameters](../gui/exporting-fits.md) for the intended
scan/channel fits. CSV reports and model templates do not replace a full session.

## Related topics

- [Start with a known synthetic spectrum](../gui/getting-started.md).
- [Practise a complete synthetic series](series-walkthrough.md).
- [Resolve symptoms at any workflow stage](../gui/troubleshooting.md).
- [Interpret areas and widths](../reference/lineshapes.md).
