# Python and Jupyter basics

NEXAFSFit's numerical package can be imported without launching the GUI. Start
with the [native single-spectrum fitting notebook](single-spectrum-fit.ipynb),
which uses the same synthetic CSV and starting model as the GUI introduction.

## Environment and source

Use the existing `nexafs-fit` Conda environment for this checkout. From its root:

```bat
conda activate nexafs-fit
jupyter lab
```

Select the environment's Python kernel. The notebook locates its accompanying
data relative to the checkout and imports the supplied `NEXAFSFit/` source. It
does not import Qt or the GUI. Jupyter Book is a development tool, not a runtime
requirement of the numerical engine or a prerequisite for reading HTML.

The source checkout's README gives development installation instructions. No
independently published numerical-package distribution or Windows executable is
assumed by this guide.

## Native model vocabulary

A basic native model uses:

| Input | Structure |
| --- | --- |
| `edge_cfg` | Global/background/edge parameter configurations keyed by engine name |
| `peak_specs` | List of component specifications with name, shape, optional group/scale and parameter dictionaries |
| A parameter configuration | `value`, `min`, `max`, `vary`; optional sharing information for series workflows |

The notebook deliberately uses a simple model with no GUI-specific constraint
or multiple-edge extensions. Its JSON is a versioned GUI model file, but the
notebook reads only the native `edge_cfg` and `peak_specs` dictionaries.

## Small API map

These names and signatures are checked against the supplied source:

### Native fitting

| Function | Purpose |
| --- | --- |
| `build_params_from_specs(edge_cfg, peak_specs)` | Construct lmfit parameters from native specifications |
| `model_components_from_specs(params, E, edge_cfg=..., peak_specs=...)` | Evaluate total, background, edge, peaks and group sums |
| `fit_spectrum_generic(E, I, edge_cfg, peak_specs, **lsq_kws)` | Return an optimisation result and calculated components |
| `residual_from_specs(params, E, I, edge_cfg, peak_specs)` | Return the full data residual vector |
| `unit_profile(shape, E, mu, sigma=0.25, gamma=0.20, tau=0.5)` | Evaluate an engine unit-area profile |

Complete ordinary fits are in the [single-spectrum notebook](single-spectrum-fit.ipynb)
and [shared-series notebook](shared-series-fit.ipynb). The latter introduces the
joint-parameter builder, multispectrum residual and per-scan extractor together.

### Native import and preparation

| Function | Purpose |
| --- | --- |
| `Import_NEXAFS_SXR(directory, filelist, feedback=False)` | Load native acquisitions into retained scan dictionaries |
| `normalise_scan_by_I0(scan, detectors=..., out_group=...)` | Store detector/I0 arrays in a processed group, preserving raw arrays |
| `calibrate_energy_with_reference_foil(scan, window, target_energy, method=...)` | Locate a reference/I0 feature and store aligned energy plus feature/offset metadata |
| `prepare_fitting_signal(scan, group=..., detector=..., baseline_window=..., scale_window=...)` | Subtract a baseline-window mean and divide by a scale-window mean |

The fitting notebooks show complete fitting calls, parameter states and recovery
checks. See [lineshapes](../reference/lineshapes.md) before interpreting area,
width, EMG position or energy-shift values.

The native calibration API and its measured-versus-aligned energy convention have
a [worked call in the scientific reference](../reference/lineshapes.md#reference-energy-calibration).
Its default method is **argextreme**; pass **parab3** explicitly to reproduce the
GUI's initial localisation choice. Native `prepare_fitting_signal` remains
**mean-only**: the GUI's optional trapezoidal area scaling is a preparation-wrapper
extension, not an extra native-function argument. See
[preparation scaling](../reference/lineshapes.md#preparation-scaling) for both definitions.

These functions populate the supplied scan dictionary's processed groups and
metadata. Use a deep copy if you want to keep that entire dictionary unchanged;
preserving raw arrays is different from making no changes to the dictionary.

## Restricted fitting domains

The GUI evaluates full-grid spectra and then masks residuals. Cropping E and I
before calling a native fitter changes the linear background's mean-energy
reference. The notebook includes a small masked-residual example using lmfit
Minimizer, preserving the full-grid convention.

For GUI-exported Python models, keep the generated setup/helpers together.
Basic dictionaries alone do not reconstruct every GUI extension: bounded peak
links need their generated relationship setup, and multiple edges need the
complete exported evaluator. **Export → Python export** supplies this code using
the independent numerical package; it does not require Qt.

## Next steps

The [native shared-series notebook](shared-series-fit.ipynb) fits three synthetic
CSVs with global positions/widths and per-scan areas, then extracts complete
single-scan components. The same files have a
[GUI walkthrough](../examples/series-walkthrough.md), including hard trends and
two-pass reseeding as separate GUI workflows.

Executable raw-SXR preparation and advanced native two-pass notebooks remain
future additions. The current GUI workflows are documented in
[preparing data](../gui/preparing-data.md), [series fitting](../gui/series-fitting.md)
and [two-pass fitting](../gui/two-pass-fitting.md). GUI strict-trend and structured
link extensions are not presented as arbitrary native-function options.

## Related topics

- [Run a native single-spectrum fit](single-spectrum-fit.ipynb).
- [Run a native shared-parameter fit](shared-series-fit.ipynb).
- [Compare both interfaces and download their example files](../examples/index.md).
- [Keep links and multiple edges in exported Python](../gui/exporting-fits.md#choose-what-to-save).
