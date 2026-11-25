# Diagnose import failures

These instructions describe GUI **0.2.30**. Use this page when an import fails or only part of a folder loads.
For the normal workflow, start with [Import data](importing-data.md). The
[measured-energy review](energy-order-review.md) has its own panel guide.

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

For folder results, use the
[Load folder checklist and count definitions](importing-data.md#load-a-folder-and-review-its-diagnostics). Continue with [scan management](scans-and-channels.md#import-files-or-a-folder)
to inspect the successfully loaded collection.

For a reproducible issue report, keep the application version from **Help → About…**,
filename, reader/detector or text-column settings, exact diagnostic text and any
reported file line or acquisition point numbers. The main Messages panel's
**History…** holds recent full workspace messages; import-dialog warnings/details
may need to be recorded separately. Preserve an unedited source file.

:::{important} Import keeps the original acquisition
Neither folder loading nor accepting a sorted copy rewrites the raw data file.
A [complete session](sessions-and-models.md) retains the original native source
and the explicit ordering choice alongside the working scan.
:::

## Import and review guides

### Check the import reader and preview

See [Check the import reader and preview](importing-data.md#check-the-import-reader-and-preview) for this procedure.

### Load a folder and review its diagnostics

See [Load a folder and review its diagnostics](importing-data.md#load-a-folder-and-review-its-diagnostics) for this procedure.

### Why an out-of-order scan opens another panel

See [Why an out-of-order scan opens another panel](energy-order-review.md#why-an-out-of-order-scan-opens-another-panel) for this procedure.

### Review before importing a sorted copy

See [Review before importing a sorted copy](energy-order-review.md#review-before-importing-a-sorted-copy) for this procedure.

### When sorting is insufficient

See [When sorting is insufficient](energy-order-review.md#when-sorting-is-insufficient) for this procedure.

## Related topics

- [Inspect field values, source identifiers and incidence mapping](metadata.md).
- [Manage successfully imported channels and overlays](scans-and-channels.md).
- [Prepare and calibrate retained native acquisitions](preparing-data.md).
- [Save the working collection and original acquisitions](sessions-and-models.md).
- [Continue through preparation and fitting](../examples/beamline-workflow.md).
- [Diagnose issues beyond import](troubleshooting.md).
