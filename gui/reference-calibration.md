# Inspect reference energy calibration

GUI **0.2.30**. After [importing native scans](importing-data.md), enable alignment in [Prepare scans](preparing-data.md) and Preview. This page describes the optional reference-inspection panel.

## Configure energy alignment

1. In **Prepare scans**, check **Align all channels using one reference / I0**.
2. Choose the **Energy-reference channel** containing the feature to align.
3. Set **Reference feature window (eV)** on the original measured-energy axis and
   enter the **Reference target energy (eV)**.
4. Choose **max** or **min** in **Reference feature**, then **parab3** or
   **argextreme** in **Feature localisation**.
5. Click **Preview** for the checked acquisitions, then inspect the panel below.

The reference/I0 calculation needs the I0 monitor even when detector I0
normalisation is unchecked. One derived offset aligns all parallel acquisition
channels; the feature search precedes baseline subtraction and intensity scaling.

(inspect-the-reference-calibration)=

## Inspect the reference calibration

With energy alignment checked, click **Preview**, then **Show reference calibration**
to expand an optional panel on the right. If that would exceed the available
screen, it opens as a **Reference calibration** preview tab instead; switch back
to **Raw / prepared preview** for the signals. **Hide reference calibration** collapses
it again. The panel is hidden initially and does not change preparation targets.

Choose an acquisition in its dropdown to see the actual reference-channel/I0
signal used by that acquisition's preview. A separate **PD reference** entry
shows the photodiode acquisition's own calibration when PD correction is enabled.
The chosen energy-reference channel is respected, including alternatives to the foil.

For **parab3**, orange points identify the three native input samples and the red
curve shows their **local interpolating parabola**. The vertical marker and readout
give the measured feature energy; the summary also reports the aligned target
and applied offset. This is feature localisation, not a full-spectrum Gaussian
or component fit. **argextreme** and boundary fallbacks show the measured point
and explain why no parabola is drawn. See [the calibration convention](../reference/preparation.md#reference-energy-calibration).

The horizontal axis is **measured energy before alignment**. It initially spans
the recorded reference search window; wheel/drag zoom can inspect the wider
retained trace. **Show search window** restores the recorded limits. The fitted
curve is drawn only across its three input samples, rather than extrapolated
over the entire reference spectrum.

Opening the panel, changing its scan or zooming never runs calibration. It reads
results retained by Preview. After settings change, the panel labels those results
as **previous preparation / preview**; Preview again to update. Unpreviewed scans,
older sessions without an inspection record and alignment-disabled preparations
show an explicit unavailable message. A failed preview does not replace an older
display with an apparently successful result. Apply still requires a successful
complete preparation preview.

:::{figure} ../media/images/reference-calibration-preview.png
:label: figure-reference-calibration
:alt: Expanded preparation dialog with a reference acquisition selector, measured reference over I0 signal, local red parabola, orange input samples and purple feature-energy marker.

Optional reference inspection in GUI 0.2.30. These scans are synthetic; measured
feature energy and the target/offset readout describe the native calibration.
:::

## Use the correct energy axis for each window

The acquisition's **reference search window** uses original measured energy.
The **baseline and scale windows** use the prepared energy axis, including the
calibration offset when alignment is enabled. The raw/prepared axes line up
visually, but their signals can have different energy offsets. Reference inspection
uses measured energy so its marker identifies the feature before alignment.

## Related topics

- [Preview and apply a preparation recipe](preparing-data.md).
- [Set baseline and scale windows](normalisation-windows.md).
- [Understand the calibration convention](../reference/preparation.md#reference-energy-calibration).
