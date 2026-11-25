# Review and sort measured energies

GUI **0.2.30**. This panel can appear during [single-file or folder import](importing-data.md). Review each affected acquisition before deciding whether to import a sorted copy.

(why-an-out-of-order-scan-opens-another-panel)=

## Why an out-of-order scan opens another panel

Fitting requires finite, strictly increasing measured energies. Even when the
requested scan steps increase, monochromator jitter or backlash can produce
out-of-order measurements. Both **Import** and **Load folder** detect this
ordering problem for native SXR scans and offer a review. The panel reports the
recorded energies; it does not diagnose the instrument cause or silently reorder
the data.

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

(review-before-importing-a-sorted-copy)=

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

(when-sorting-is-insufficient)=

## When sorting is insufficient

:::{warning} Sorting does not repair every acquisition
**Import sorted copy** is disabled if the proposed view still fails validation.
Inspect the explanation: exact duplicate energies, nonfinite selected values
and too few points require explicit resolution. The GUI does not average
duplicates or drop bad rows. **Skip scan** leaves that acquisition out while
allowing other folder results to be retained.
:::

The graphical ordering review is for retained native SXR scans. Ordinary text/CSV
columns with non-increasing energy are rejected with an import error; they do not
receive that native review dialog. If you create a corrected text-file copy outside
the GUI, preserve complete row/channel pairing and keep the original for provenance.

## Related topics

- [Import a file or folder](importing-data.md).
- [Diagnose unresolved import errors](import-diagnostics.md).
- [Prepare the retained acquisition](preparing-data.md).
