# Compare scans with overlays

GUI **0.2.30**. Start with [imported scans](importing-data.md) and choose each row’s [Detector](scans-and-channels.md#select-each-scans-detector-independently). Overlays are an inspection workflow.

(compare-scans-without-changing-fit-targets)=

## Compare scans without changing fit targets

Check row **Overlay** boxes or use **Overlay all scans**. Each overlay uses that
row's Detector, so a PD acquisition can show Photodiode Direct while a sample
shows prepared Channeltron. The active curve is slightly thicker; all overlaid
traces are lines labelled with filename, channel and stage. **Clear overlays**
returns to the active spectrum's point display.

:::{figure} ../media/images/scan-overlays-controls.png
:label: figure-scan-overlays-controls
:alt: Main plot with three synthetic scans displayed as sequentially coloured lines and labelled by filename and channel, alongside the active model and residual.

Three synthetic scans overlaid for comparison. The active model and residual
still refer to the active scan; the extra traces do not become fitting targets.
:::

:::{warning} Compare compatible signals and scales
Overlays do not resample or comparison-normalise signals. A raw monitor can have
a very different intensity scale from a prepared detector: choose compatible stages
when comparing trends.
:::

### Follow the comparison colours

[Comparison colours](component-display.md#change-component-and-comparison-colours)
are editable through **Line colours…**. For model-based comparison views, see
[edge subtraction](edge-subtraction.md#subtract-current-edges).

Default comparison colours follow a sequential Viridis gradient across the loaded
scan-table order. Early scans are purple, then blue/teal, then green. Active and
inactive traces use the same mapping; toggling overlays does not redistribute it.
Adding scans can redistribute default colours across the larger collection.
Recorded angles are not used to silently sort scans. Individually assigned colours
override the gradient and remain saved with the session.

### Distinguish comparison and processing selections

:::{important} Overlay checkboxes do not select fit targets
Main-plot overlays use each row's Detector selection. Preparation and series
fitting have their own explicit target lists; **Fit spectrum** uses only the
active displayed channel.
:::

| Operation | Which data it uses |
| --- | --- |
| Main plot overlays | Each checked row's own Detector selection |
| Fit spectrum | Active row's displayed channel only |
| Prepare scans | Explicitly selected native acquisitions; recipe prepares all mapped detectors |
| Propagate model / fit | Explicit targets on the source channel/stage, including available cached channels |
| CSV export | Explicit rows' currently selected channels with retained successful fits |

## Related topics

- [Choose independent detector channels](scans-and-channels.md).
- [Change comparison colours](component-display.md#change-component-and-comparison-colours).
- [Compare preparation previews](preparing-data.md).
