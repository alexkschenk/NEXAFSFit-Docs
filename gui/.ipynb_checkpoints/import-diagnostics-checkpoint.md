# Import diagnostics and energy-order review

These instructions describe GUI **0.2.24**. Import reads data without normalising,
calibrating or changing the original file. This page explains the checks before
a scan becomes fit-ready; [preparation](preparing-data.md) is a later operation.

## Check the import reader and preview

Open **Data import → Import**. For a recognised native SXR `.asc` acquisition,
the Reader is **Native SXR .asc (automatic header parsing)**. The panel shows the
filename/comment, point count, raw-detector selector and a short numerical preview.
It retains all mapped channels, not only the detector currently previewed.

**Show metadata / column details** expands the original header when needed.
Use it to check file identity and column descriptions; see the
[metadata guide](metadata.md) for searching retained fields after import. The
numerical preview shows only the first eight native measurements, not the whole scan.

For ordinary text/CSV, select **Text columns**, delimiter, header rows to skip and
distinct **1-based** energy/intensity columns. The file excerpt helps identify
these settings. A `.asc` extension alone does not guarantee the native structure:
its column-description and scan-values blocks must be recognised. Changing readers
is not a way to repair invalid data. Text-column import retains the chosen arrays,
not a complete native acquisition for later monitor/reference processing.

## Why an out-of-order scan opens another panel

Fitting requires finite, strictly increasing measured energy. A native scan can
contain small backward steps, including early monochromator jitter or backlash.
The GUI detects the ordering problem; it does not diagnose the instrument cause
or silently reorder it.

**Review measured-energy order** reports the filename, point count, backward steps,
repeated energy points and the first non-increasing step. Its graph plots **measured
energy against acquired point number**, initially near the first problem. This
is an acquisition-order diagnostic, rather than the fitted intensity spectrum.

:::{figure} ../media/images/energy-order-review-preview.png
:label: figure-energy-order-review
:alt: Energy-order review dialog with measured energy against acquired point, backward-step counts, acquisition/sorted table tabs and Import sorted copy or Skip scan buttons.

An intentionally jittered synthetic scan. The original point numbers remain
available when inspecting the sorted preview.
:::

## Review before importing a sorted copy

1. Inspect the graph and the reported first non-increasing step. Zoom if the
   problem extends beyond the initially visible points.
2. In **Acquisition order**, compare original point numbers, measured energies
   and the selected detector values.
3. In **Sorted preview**, inspect the proposed increasing-energy order. Original
   point numbers identify where each measurement came from.
4. Choose **Import sorted copy** only if this is appropriate for that acquisition,
   or **Skip scan** to leave it unimported.

The reviewed choice sorts display/fitting arrays by **measured energy** for every
channel, maintaining paired detector values. It preserves all measurements and
does not average, interpolate, smooth or trim points. Native processing still uses
acquisition-order copies, preserving photodiode/mono-setpoint pairing. Original
order, the native dictionary and source bytes remain retained in complete sessions.

Simply viewing the preview does not accept sorting. A skipped scan does not remove
other scans already loaded. For single-file import, skipping returns to the import
panel; cancel that panel if you do not want to proceed.

## When sorting is insufficient

**Import sorted copy** is unavailable if the proposed view still fails validation.
Exact duplicate energies cannot form a strictly increasing axis. Nonfinite selected
energy/intensity values and too few points also need explicit resolution. The GUI
does not average duplicates or drop bad rows to make a file fit.

The graphical ordering review is for retained native SXR scans. Ordinary text/CSV
columns with non-increasing energy are rejected with an import error; they do not
receive that native review dialog. If you create a corrected text-file copy outside
the GUI, preserve complete row/channel pairing and keep the original for provenance.

## Diagnose common failures

| Message or symptom | Check |
| --- | --- |
| Cannot read selected columns | Text reader delimiter, skipped header rows, column numbers and numeric format |
| Native scan-values block missing | Reader selection, complete file/header and whether the acquisition is actually supported SXR format |
| Invalid numeric data on file line… | Inspect that original line; the importer has not discarded it |
| Inconsistent column count | Check for truncated/malformed rows or incomplete writes |
| Missing/non-finite data | Check measured energy and the selected detector; optional channels can remain retained but may fail when selected/prepared |
| Energy must be strictly increasing / duplicate energies | Use the native ordering review where offered; duplicates require a separate explicit decision |
| No detector channels / expected channel absent | Inspect original column details and mapped detector names; unmapped source columns remain in the retained bytes but are not automatically usable channels |
| File changed during import | Wait for acquisition/synchronisation to finish, then retry the stable file |
| No `.asc` scans found | Load folder is non-recursive and handles native `.asc` files; import text/CSV individually |

## Folder results and information to keep

Folder loading keeps valid scans while reporting failed filenames. Ordering review
is offered for affected native acquisitions; skipping one does not discard the rest.
The final message distinguishes loaded, already loaded, failed and skipped counts.
Cancelling loading retains scans already read. See [folder behaviour](scans-and-channels.md#import-files-or-a-folder).

For a reproducible issue report, keep the application version from **Help → About…**,
filename, reader/detector or text-column settings, exact diagnostic text and any
reported file line or acquisition point numbers. The main Messages panel's
**History…** holds recent full workspace messages; import-dialog warnings/details
may need to be recorded separately. Preserve an unedited source file.

## Related topics

- [Inspect field values, source identifiers and incidence mapping](metadata.md).
- [Manage successfully imported channels and overlays](scans-and-channels.md).
- [Prepare and calibrate retained native acquisitions](preparing-data.md).
- [Save the working collection and original acquisitions](sessions-and-models.md).
- [Continue through preparation and fitting](../examples/beamline-workflow.md).
- [Diagnose issues beyond import](troubleshooting.md).
