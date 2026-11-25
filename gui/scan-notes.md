# Attach notes to a scan

GUI **0.2.30**. Notes record observations alongside an imported acquisition. They are separate from raw-file comments and metadata.

(attach-notes-to-a-scan)=

## Attach notes to a scan

Right-click a scan row and choose **Edit scan notes…**. Write observations or fitting
reminders, then **Save** to attach the text to that scan; **Cancel** discards the
dialog edits. Notes belong to the acquisition across all Detector channels and
remain attached when preparation or reviewed series fits are applied.

:::{figure} ../media/images/scan-notes-controls.png
:label: figure-scan-notes-controls
:alt: Scan notes editor identifying sample-A.asc, with example fitting observations and Save and Cancel buttons.

Example notes for a synthetic acquisition. Save commits the text to the working
scan; save a complete session to keep it across restarts.
:::

To inspect them in the table, right-click a column heading and enable **Notes**.
Hover for full text or double-click its cell to edit. Notes are plain text and do
not replace the recorded comment/label or modify the acquisition file.

Use **Session → Save session** to persist notes with the collection, then **Open
session** to restore them. Model-only JSON and fitted-result CSV commands do not
carry scan notes. Notes are not model edits in the Undo history. See
[session checkpoints](sessions-and-models.md).

## Related topics

- [Inspect recorded metadata](metadata.md).
- [Manage scan selection](scans-and-channels.md).
- [Save notes in a session](sessions-and-models.md).
