# Acquisition alignment and normalisation conventions

GUI **0.2.30** uses the following acquisition-processing definitions.
For the recipe controls, use [Prepare scans](../gui/preparing-data.md).

(reference-energy-calibration)=

## Reference energy calibration

Native `calibrate_energy_with_reference_foil` locates a chosen maximum/minimum
of reference/I0 within a search window on the **original measured energy axis**.
`argextreme` selects the measured extreme. `parab3` refines it with the vertex of
the quadratic through that sample and its two immediate in-window neighbours
in acquisition order. At a window boundary it falls back to the measured point;
degenerate or linear three-point inputs also use the centre point. This is local
interpolation, not an independently optimised peak-shape fit.

The data offset is `target_energy - feature_energy`, applied equally to all
parallel acquisition channels. A separate photodiode acquisition is calibrated
independently. No baseline/scale windows are involved in finding this reference
feature. The later baseline and scale windows use aligned energies. This data
alignment offset is distinct from the model's optional `dE`.

Choose reference windows and targets appropriate to the acquisition. The native
function can construct reference/I0 when absent; its method default is argextreme,
whereas the GUI initially selects parab3. The GUI's optional inspection panel
retains the native input trace and output feature/offset. Its local curve uses
the same three-point formula without rerunning calibration or modifying the result.
An out-of-window native vertex is reported without silently moving or clipping it.

(preparation-scaling)=

## Preparation scaling

For a baseline window $W_b$, preparation first subtracts
$b=\operatorname{mean}_{E_i\in W_b} y_i$. Mean scaling uses
$D=\operatorname{mean}_{E_i\in W_s}(y_i-b)$ and returns $(y_i-b)/D$.
This is the unchanged native `prepare_fitting_signal` behaviour and the GUI default.
The native function has no selectable area-method argument.

### Optional area denominator

The GUI's optional area scaling instead uses
$D=\sum_i (E_{i+1}-E_i)[(y_i-b)+(y_{i+1}-b)]/2$, over consecutive
measured points inside $W_s$. At least two distinct points and a finite nonzero
denominator are required. The integration is signed, accounts for unequal energy
spacing and adds no interpolated endpoints. It uses increasing measured energy;
an acquisition with reversals first requires the user's explicit sorting review.

This area denominator has signal-times-energy units. Setting the prepared window
integral to one differs from setting its average height to one; neither quantity
is an individual model peak area $A$. Baseline subtraction still uses a mean in
both modes. Settings, native intermediates and actual scale denominators are
retained without changing original data. See the [GUI preparation procedure](../gui/preparing-data.md).

## Related topics

- [Inspect reference calibration](../gui/reference-calibration.md).
- [Choose normalisation windows](../gui/normalisation-windows.md).
- [Distinguish processing from the spectral model](lineshapes.md).
