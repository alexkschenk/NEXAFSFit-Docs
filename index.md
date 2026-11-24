# NEXAFSFit

**NEXAFS modelling, preparation and fitting with the graphical interface.**

Use the graphical interface to inspect scans, build models and fit spectra.
This manual introduces the current workflow using synthetic exercises and a
folder-to-results guide, with reference material
for interpreting fitted parameters.

::::{grid} 1 1 2 2
:::{card} Graphical interface
:link: gui/getting-started.md

Learn to import spectra, construct models interactively, refine fits and inspect results.
:::
:::{card} Scientific reference
:link: reference/lineshapes.md

Explore the meaning of model parameters, lineshapes, constraints and fitting choices.
:::
:::{card} Worked examples
:link: examples/index.md

Follow complete, reproducible analyses with associated datasets.
:::
::::

:::{note} Version and scope
The manual includes GUI **0.2.28** and the supplied numerical engine.
Minor interface refinements may follow. Advanced experimental examples and videos
are planned; they are not prerequisites for the basic workflows documented here.
Windows executable distribution is also future work.
:::

## Choose a reading path

| Starting point | Suggested route |
| --- | --- |
| New to the GUI | [First synthetic fit](gui/getting-started.md) → [fit controls/results](gui/fitting-and-results.md) → [save or export](gui/exporting-fits.md) |
| Working on a small or scaled display | [Arrange panels and choose a compact layout](gui/layout-and-small-screens.md) |
| Working through an acquisition folder | [Folder-to-results workflow](examples/beamline-workflow.md), with checkpoints and links to each stage |
| Starting with beamline acquisitions | [Import and select channels](gui/scans-and-channels.md) → [prepare scans](gui/preparing-data.md) → [construct a model](gui/model-editor.md) |
| Building or refining a model | [Parameters and components](gui/model-editor.md) → [drag controls/bounds](gui/drag-controls.md) → [display tools](gui/component-display.md) → [peak/edge links](gui/peak-constraints.md) |
| Fitting a collection | [Reviewed single fit](gui/fitting-and-results.md) → [series setup](gui/series-fitting.md) → [optional second pass](gui/two-pass-fitting.md); practise with the [series exercise](examples/series-walkthrough.md) |

Use the scientific reference alongside the GUI: [profiles, areas and
energy conventions](reference/lineshapes.md), or [bounds, sharing, trends and
statistics](reference/constraints.md). The [worked examples](examples/index.md)
provide known data and complete GUI exercises. Preparation uses copies
and does not rewrite raw acquisition files.

## Find a workflow

For a quick route to a particular task:

| Task | Instructions |
| --- | --- |
| Find the cause of an unexpected result or disabled control | [Troubleshooting and workflow checks](gui/troubleshooting.md) |
| Resolve import errors or review measured-energy reversals | [Import diagnostics](gui/import-diagnostics.md) |
| Inspect metadata or choose an incidence-angle source | [Metadata and measurement angles](gui/metadata.md) |
| Record fitting observations with a scan | [Scan notes](gui/scans-and-channels.md#attach-notes-to-a-scan) |
| Check foil alignment and its measured feature energy | [Reference inspection](gui/preparing-data.md#inspect-the-reference-calibration) |
| Decide between average-height and integrated-area scaling | [Normalisation windows](gui/preparing-data.md#choose-normalisation-windows-by-inspection) |
| Inspect several scans or channels together | [Scan browser and overlays](gui/scans-and-channels.md#compare-scans-without-changing-fit-targets) |
| Place many peaks, choose colours or assign custom groups | [Model construction](gui/model-editor.md) |
| Keep one peak within a separation of another | [Peak and edge links](gui/peak-constraints.md) |
| Link one or several edge positions to peaks | [Edge-to-peak linking](gui/peak-constraints.md#link-the-first-ionisation-edge-to-a-peak) |
| Change component visibility, focus or colours | [Component display settings](gui/component-display.md) |
| Inspect data with the current edges subtracted | [Subtract current edges](gui/component-display.md#subtract-current-edges) |
| Shift component curves down for inspection | [Offset components](gui/component-display.md#offset-components) |
| Move components, change widths/tails or drag their allowed position bounds | [Drag controls](gui/drag-controls.md) |
| Change the fit budget or convergence tolerances | [Fitting settings](gui/fitting-and-results.md#set-the-evaluation-budget-and-tolerances) |
| Share parameters, constrain trends and reseed a second pass | [Series fitting](gui/series-fitting.md) and [two-pass fitting](gui/two-pass-fitting.md) |
| Try an entire series workflow on known data | [Synthetic series walkthrough](examples/series-walkthrough.md) |
| Save editable work or reuse a model with new data | [Sessions and models](gui/sessions-and-models.md) |
| Export numerical fit reports | [CSV exports](gui/exporting-fits.md) |
