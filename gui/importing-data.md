# Import data

GUI **0.2.30**. Use **Data import** to add acquisitions to the collection. Import does not normalise, calibrate or rewrite source files.

(check-the-import-reader-and-preview)=

## Check the import reader and preview

Open **Data import → Import** and choose a file. For a recognised native SXR `.asc` acquisition,
the Reader is **Native SXR .asc (automatic header parsing)**. The panel shows the
filename/comment, point count, raw-detector selector and a short numerical preview.
It retains all mapped channels, not only the detector currently previewed.

**Show metadata / column details** expands the original header when needed.
Use it to check file identity and column descriptions; see the
[metadata guide](metadata.md) for searching retained fields after import. The
numerical preview shows only the first eight native measurements, not the whole scan.

:::{figure} ../media/images/import-native-controls.png
:label: figure-native-import-controls
:alt: Native SXR import dialog with a numeric preview, collapsed metadata checkbox, automatic native reader and Channeltron detector selector.

Native import of a synthetic acquisition. The detector selects the preview;
the acquisition's mapped channels are retained together.
:::

### Text/CSV column settings

For ordinary text/CSV, select **Text columns**, delimiter, header rows to skip and
distinct **1-based** energy/intensity columns. The file excerpt helps identify
these settings. A `.asc` extension alone does not guarantee the native structure:
its column-description and scan-values blocks must be recognised. Changing readers
is not a way to repair invalid data. Text-column import retains the chosen arrays,
not a complete native acquisition for later monitor/reference processing.

:::{figure} ../media/images/import-text-controls.png
:label: figure-text-import-controls
:alt: Text import dialog showing a CSV excerpt, comma delimiter, one skipped header row, energy column one and intensity column two.

Settings for the supplied two-peak CSV exercise: comma delimiter, one header row,
and columns 1/2. Choose settings appropriate to your own file's structure.
:::

(load-a-folder-and-review-its-diagnostics)=

## Load a folder and review its diagnostics

1. Choose **Data import → Load folder**, then select the directory containing your
   native SXR `.asc` acquisitions. Only files directly in that directory are read;
   subdirectories and other file extensions are ignored.
2. Watch the progress dialog for the filename being read. Files are processed in
   filename order. Unlike **Import**, this route reads native headers automatically
   without opening the single-file reader/detector preview for each file. It does
   not offer text-column settings for files that are not recognised native scans.
3. After the reading stage, review any **Review measured-energy order** dialogs.
   Each identifies the affected file. Follow the
   [sorting review steps](energy-order-review.md#review-before-importing-a-sorted-copy), then choose
   **Import sorted copy** or **Skip scan** for that acquisition. An ordering dialog
   does not mean the whole folder has failed.
4. Check the loaded scans in the scan panel. Each retains its mapped channels;
   use that row's **Detector** dropdown to inspect another signal. Channeltron is
   preferred initially when present. See
   [independent detector selection](scans-and-channels.md#select-each-scans-detector-independently).
5. Read the final result counts in **Messages**. If **Some scans could not be
   imported** appears, record the listed filenames and their individual errors.
   Successfully imported scans remain available.

| Folder result | Meaning and next action |
| --- | --- |
| `loaded` | Scans added to the browser, including any explicitly accepted sorted copies |
| `already loaded` | The same retained native filename and exact source contents were found; no second copy is added |
| `failed` | A file could not be read or validated; inspect its reported error and retry that file with **Import** if needed |
| `skipped` | An energy-order review was declined, or a scan awaiting review was left out after cancelling folder loading |

**Cancel** stops the reading stage between files. Valid scans already read are
still added; pending energy-order reviews are skipped. Files not yet read are
left out of this attempt. Closing an individual review is equivalent to
**Skip scan** for that file, so review the final counts before continuing.

:::{tip} Isolate a problem file
Use **Data import → Import** on a failed file to inspect its reader, header and
numerical preview. For text or CSV data, use this single-file route to choose
columns explicitly. After resolving the problem, you can retry the folder:
unchanged scans already retained under the same filename are skipped.
:::

## Continue after import

Select the intended [scan and detector](scans-and-channels.md), inspect
[metadata](metadata.md), then [prepare native scans](preparing-data.md) if needed.
For a failure, use [import diagnostics](import-diagnostics.md).

## Related topics

- [Review measured-energy ordering](energy-order-review.md).
- [Diagnose a failed import](import-diagnostics.md).
- [Manage loaded scans](scans-and-channels.md).
