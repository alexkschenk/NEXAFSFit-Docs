# Use a separate photodiode scan

GUI **0.2.30**. Use this optional correction inside [Prepare scans](preparing-data.md). [Import](importing-data.md) the sample and PD acquisitions first when using a shared folder.

## Select a reference

1. Check **Divide by separate I0-normalised photodiode scan** to reveal its controls.
2. Choose an acquisition in **Loaded photodiode scan**, or use **Select photodiode .asc…** for a separate native file.
3. Choose the appropriate **Photodiode channel** from the reference’s mapped channels.
4. Check sample targets explicitly. The chosen shared PD acquisition cannot also be a preparation target; **Select all data scans except the chosen photodiode reference** provides a shortcut.
5. Preview, compare the selected scans, then apply only after reviewing their statuses.

:::{figure} ../media/images/photodiode-selection-controls.png
:label: figure-photodiode-selection-controls
:width: 520px
:alt: Expanded photodiode options with a separate-file button, loaded scan dropdown, selected reference filename and Photodiode Direct channel.

Detail from Prepare scans after enabling PD correction. The loaded-reference
dropdown and channel selector identify which acquisition supplies the response.
The [full preparation window](preparing-data.md#figure-prepare-scans-overview)
shows these controls alongside targets and previews.
:::

## Check the separate photodiode overlap

The PD acquisition is I0-normalised and, when selected, independently energy
aligned. Its response is interpolated for division over the common energy
range. The overlap can shorten the prepared spectrum; inspect the preview and
retained point counts. Missing references, zero denominators and unsuitable
windows are errors to resolve, not reasons to silently invent a correction.

:::{important} One PD acquisition can serve several sample scans
The same selected reference is used for the checked preparation targets. It is
not selected automatically merely because a photodiode file was imported.
:::

## Inspect the effect across the set

Use preparation overlays and its stage dropdown to compare I0-normalised,
I0 / photodiode corrected and fit-ready signals. Settings changes require a new
Preview. If alignment is enabled, the optional
[reference panel](reference-calibration.md) also offers a PD reference entry.

## Related topics

- [Select and preview sample targets](preparing-data.md).
- [Place normalisation windows](normalisation-windows.md).
- [Import a shared reference with a folder](importing-data.md#load-a-folder-and-review-its-diagnostics).
