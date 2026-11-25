# NEXAFSFit

**NEXAFS modelling, preparation and fitting with the graphical interface.**

The manual describes GUI **0.2.30** and the supplied scientific engine. Choose a
task below or follow a worked exercise. Individual process pages are the main
instructions; troubleshooting pages link to the relevant controls.

::::{grid} 1 1 2 2
:::{card} First fit
:link: gui/getting-started.md

Fit a known synthetic spectrum and learn the main controls.
:::
:::{card} Acquisition workflow
:link: examples/beamline-workflow.md

Follow the route from a raw folder to reviewed reports, with session checkpoints.
:::
:::{card} Worked examples
:link: examples/index.md

Practise single and series fits with distributable synthetic data.
:::
::::

:::{note} Scope
Windows executable distribution, advanced experimental examples and videos are
planned. Python/notebook reader material is deferred. These are not prerequisites
for the GUI workflows documented here.
:::

## Choose a reading path

| Starting point | Route |
| --- | --- |
| New to the GUI | [First fit](gui/getting-started.md) → [review the result](gui/fit-results.md) → [save a session](gui/sessions-and-models.md) |
| Raw acquisitions | [Import](gui/importing-data.md) → [inspect scans](gui/scans-and-channels.md) → [prepare](gui/preparing-data.md) → [build a model](gui/model-editor.md) → [fit](gui/fitting-and-results.md) |
| A series of spectra | [Review a representative fit](gui/fit-results.md) → [series setup](gui/series-fitting.md) → [review staged results](gui/series-review.md) |
| Small or scaled display | [Arrange panels and recover hidden controls](gui/layout-and-small-screens.md) |

## Find a workflow

| Task | Main procedure |
| --- | --- |
| Import one file, text/CSV or a folder | [Import data](gui/importing-data.md) |
| Review backwards energy steps or duplicates | [Measured-energy sorting panel](gui/energy-order-review.md) |
| Find why a file failed | [Import diagnostics](gui/import-diagnostics.md) |
| Select each scan’s channel | [Scan browser and detector channels](gui/scans-and-channels.md) |
| Overlay spectra | [Compare scans](gui/comparing-scans.md) |
| Inspect source/PV fields or incidence | [Metadata and angles](gui/metadata.md) |
| Write observations | [Scan notes](gui/scan-notes.md) |
| Select preparation targets and apply a recipe | [Prepare scans](gui/preparing-data.md) |
| Inspect reference features and fitted energy | [Reference calibration panel](gui/reference-calibration.md) |
| Use a separate PD acquisition | [Photodiode correction](gui/photodiode-correction.md) |
| Drag baseline/scale windows and choose mean or area | [Normalisation windows](gui/normalisation-windows.md) |
| Add model terms and peak groups | [Model construction](gui/model-editor.md) |
| Place one or many peaks | [Adding peaks](gui/adding-peaks.md) |
| Set values, bounds, fixed states and sharing | [Parameter table](gui/parameter-table.md) |
| Drag component handles and μ bounds | [Drag controls](gui/drag-controls.md) |
| Relate two peak parameters | [Peak constraints](gui/peak-constraints.md) |
| Link one or several edges to peaks | [Edge-link panel](gui/edge-links.md) |
| Change visibility, focus and colours | [Component display](gui/component-display.md) |
| Inspect data above the current edges | [Edge-subtracted display](gui/edge-subtraction.md) |
| Shift component curves down | [Component offsets](gui/component-offsets.md) |
| Set the fitting domain and run or stop a fit | [Single-spectrum fitting](gui/fitting-and-results.md) |
| Change the evaluation budget/tolerances | [Fit settings](gui/fit-settings.md) |
| Assess residuals, bound hits and uncertainty | [Single-fit review](gui/fit-results.md) |
| Select series targets and a strategy | [Series setup](gui/series-fitting.md) |
| Restrict propagation around a reference | [Series constraints](gui/series-constraints.md) |
| Set soft/strict trends and scan coordinates | [Series trends](gui/series-trends.md) |
| Average seeds and tighten a second pass | [Two-pass fitting](gui/two-pass-fitting.md) |
| Compare passes and apply results | [Series review](gui/series-review.md) |
| Save or reopen the complete workspace | [Sessions](gui/sessions-and-models.md) |
| Reuse a model on new data | [Model templates](gui/model-files.md) |
| Restore an interrupted workspace | [Recovery](gui/recovery.md) |
| Write fitted curves or parameters to CSV | [Fit exports](gui/exporting-fits.md) |
| Find an unexpected outcome | [Troubleshooting](gui/troubleshooting.md) |

## Scientific definitions

- [Profiles, areas, widths and model energy](reference/lineshapes.md).
- [Acquisition energy alignment and normalisation](reference/preparation.md).
- [Bounds, links, sharing and trend units](reference/constraints.md).
- [Fit statistics and uncertainty](reference/fit-statistics.md).
