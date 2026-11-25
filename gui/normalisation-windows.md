# Set baseline and scale windows

GUI **0.2.30**. Open [Prepare scans](preparing-data.md) on imported native acquisitions. Enable intensity preparation and Preview before placing the plot’s window handles.

(choose-normalisation-windows-by-inspection)=

## Choose normalisation windows by inspection

After Preview, drag the **blue Baseline** and **orange Scale** boundaries on the
lower prepared-energy plot. Drag either boundary to change a limit, or drag the
shaded interval to move a whole window. The numeric fields update when the drag
is released; numeric edits also move the markers. The markers are hidden when
baseline/scale intensity preparation is unchecked.

If a default window lies outside your scan, enter suitable limits numerically
before Preview. The defaults are suggestions, not windows inferred from the data.

The lower plot uses the prepared energy axis, including the chosen alignment.
The raw-count plot above remains a comparison with the original acquisition.
Use the preview-stage dropdown to inspect corrected or I0-normalised signals as
well as the fit-ready signal, and enable overlays to compare selected scans.

Changing a window or recipe invalidates Apply. The previous curves remain visible
as an explicitly labelled **previous preview**, so features do not disappear
while placing the windows. No processing runs during a drag. Click **Preview**
again and review the result before **Apply to selected scans** becomes available.

:::{figure} ../media/images/preparation-windows-preview.png
:label: figure-preparation-windows
:alt: Unified preparation dialog with scan-selection checkboxes, a shared photodiode reference, and blue baseline and orange scale window boundaries on the lower prepared-energy plot.

The same dialog prepares one scan or an explicitly selected series. These
illustrative scans are synthetic; matching colours identify corresponding traces.
:::

### Choose mean or area scaling

After optional I0/PD correction, intensity preparation subtracts the mean in the
baseline window. **Scale method** then selects the denominator:

- **Mean to one** (default): mean of the baseline-subtracted signal in the scale window.
- **Area to one (trapezoidal)**: signed integral of that signal against measured
  energy, using consecutive points inside the scale window. It requires at least
  two points and a nonzero finite area. Endpoints are not interpolated or extrapolated.

Area scaling accounts for unequal energy spacing; it makes the measured window's
integral one, rather than its average height. It also changes the prepared signal's
units relative to mean scaling. Negative signed areas are allowed, as negative
means are in mean mode. Choose suitable windows and inspect the preview. These
preparation areas are distinct from individual fitted peak areas $A$.

The native engine supplies baseline/mean preparation; optional area scaling is
implemented in the GUI's Qt-independent preparation wrapper. Existing sessions
without a scale-method setting keep mean scaling. The choice and denominator
are retained with the processed channels and session. See also
[scientific conventions](../reference/preparation.md#preparation-scaling).

## Related topics

- [Preview and apply preparation](preparing-data.md).
- [Choose a separate photodiode reference](photodiode-correction.md).
- [Check scaling definitions and units](../reference/preparation.md#preparation-scaling).
