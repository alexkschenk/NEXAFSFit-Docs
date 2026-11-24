# Inspecting metadata and measurement angles

These instructions describe GUI **0.2.24**. Metadata identifies an acquisition and
its recorded settings. The Metadata view is read-only; selecting a display source
does not edit the raw file, normalise a signal or constrain a fit.

## Open the Metadata tab for the active scan

1. Select the intended scan in **Scans / metadata**. Metadata follows the active
   acquisition, not whichever overlay happens to be under the pointer.
2. Open the **Metadata** tab. If the browser is hidden, reopen it with **Scans /
   metadata**; Detach/Attach can move it into a separate window.
3. Type in **Filter retained metadata…** to match text in field names, values,
   units or source/PV identifiers. Filtering hides rows in this view only.
4. Hover over a cell to read its full text, then compare any relevant original
   header/column details in the import or preparation panel.

| Column | Meaning |
| --- | --- |
| Source / PV | Recorded beamline PV, or Raw header for ordinary header fields; first column |
| Field | Recorded description or header key; starts at a resizable 420 px width |
| Recorded value | Value read from the original file |
| Unit | Recorded unit where available; a blank unit is not an inferred conversion |

:::{figure} ../media/images/metadata-browser-preview.png
:label: figure-metadata-browser
:alt: Raw metadata browser with Source/PV first, resizable Field column, recorded value and unit, and only the original R1 row.

The metadata browser shown using synthetic acquisitions. Field availability depends
on each file; this screenshot is not an experimental metadata inventory.
:::

## What is retained and what is displayed

Native SXR import keeps the original source bytes/header and the engine's complete
scan dictionary. The Metadata tab reads the original file's `# key = value` and
Extra PV records **in the order they appear in the header**. It does not append
engine-dictionary entries, processing settings or calculated incidence rows.
Repeated records remain in their original positions. Filtering only hides rows;
it does not reorder them.

Missing units remain blank; quoted commas and wrapped PV records stay in their
own fields. An unrecognised PV record is shown as an unparsed raw record for
inspection. Numeric column descriptions remain available in the original
header/column view. Original dictionaries and source bytes are unchanged.
Source/PV and Field start at compact resizable widths; drag their header boundaries
for longer text and hover for complete cell contents. Widths stay as adjusted
while switching scans.

The scan table shows summaries including filename/comment, set-energy range,
acquisition time, raw position and incidence. Scan number remains in metadata
rather than a redundant table column. The Metadata tab provides the broader searchable
record. The original header/column view is useful for checking a field that the
current parser does not expose. Retaining source bytes does not mean every original
numeric column has become a mapped detector channel.

Ordinary text/CSV import retains selected columns and basic import settings, but
does not retain a native raw-header record for this table; its Metadata table is
empty. Acquisition metadata is also distinct from
[preparation diagnostics](preparing-data.md#inspect-the-reference-calibration)
or [fit statistics](fitting-and-results.md): use those panels for reference offsets,
processing results and fitted parameters.

## Raw sample position

The scan table's **Sample position (X, Y, Z, R1, R2)** shows the original manipulator
readbacks in that order. The identified PVs are `SR14ID01MCS02FAM:X.RBV`, `Y.RBV`,
`Z.RBV`, `R1.RBV` and `R2.RBV` with the same prefix. For example,
`(1.5 mm, 10.7 mm, 4 mm, 80°, 0°)` displays raw coordinates; hover over the cell for each
axis's recorded unit and exact PV. The supplied format uses mm for translations
and degrees for rotations. Values are displayed without unit conversion.

**Raw R1 = 80° stays 80° in this tuple**, while the separate calibrated incidence
column shows 35° from the surface. Missing, conflicting or nonfinite readbacks
appear as `—` in their tuple position. Unrelated motors and guessed coordinates
are not substituted. Preparation, detector switching and model changes do not
alter these readbacks. See [scan-table set energy and layout](scans-and-channels.md#import-files-or-a-folder).

Recognised degree-valued incidence readouts use **°**, for example **35°**.
The Metadata tab retains the originally recorded unit strings such as `deg`;
display symbols do not convert raw values or change the angle convention.

User observations are separate from recorded metadata: use
[scan notes](scans-and-channels.md#attach-notes-to-a-scan), saved with complete sessions.

## Select the incidence-angle source

The scan table's incidence field uses automatic identification initially.
It recognises explicit incidence fields and the identified R1 calibration
below; it does not choose an unrelated undulator, grating or mirror angle.

There is no incidence-source dropdown in the Metadata panel. If an explicit
mapping is needed, right-click a raw metadata row and use **Use as incidence-angle
source**. The mapping is applied
across the scan browser using that source identifier, rather than one row's current
numeric value. Other acquisitions can therefore show different values—or unavailable
if that field is absent. Selecting a source does not replace original metadata.
Use **Use automatic incidence-angle source** in the same context menu to return
to automatic identification without adding any metadata rows.

For a directly recorded angle, choose the interpretation label **From surface
(90° = normal)**, **From normal (0° = normal)** or **As recorded**. This selector
labels the convention; it does not calculate 90 minus the value or convert units.
Use a source whose recorded meaning and units you understand.

## The identified R1 conversion

For degree-valued `SR14ID01MCS02FAM:R1.RBV` (**R1 Position**), the GUI derives
incidence from the sample surface as **R1 − 45°**:

| Recorded R1 | Derived incidence |
| ---: | ---: |
| 80° | 35° from the surface |
| 135° | 90° from the surface: normal incidence |

The Metadata tab shows only recorded **R1 Position**, in its original file position.
The calculated incidence is shown separately in the scan table and its tooltip;
no corrected row is added to raw metadata. Mapping the identified raw R1 uses this
calibration for the scan table. Its convention remains fixed to the sample surface;
the convention selector is read-only for this derived scan-table value.

The exact PV identifier matters, not its Extra PV sequence number in the header.
Other R1-like names and already-recorded incidence angles do not receive this offset.
This display calculation does not alter energy calibration, intensity preparation,
spectral arrays or model parameters.

## Missing, ambiguous and unexpected fields

| What you see | Interpretation / next step |
| --- | --- |
| `—` for incidence | No valid identified source, a missing mapped field or conflicting automatic candidates; inspect raw fields and the scan-table tooltip, then use the row context menu if needed |
| R1 conversion unavailable | Check the exact PV, a finite unambiguous value and degree units |
| Direct incidence disagrees with derived R1 | Automatic reports the conflict; choose the intended source explicitly |
| Mixed convention in the column heading | Inspect each row's tooltip and source meaning rather than treating all numeric values as equivalent |
| A field disappears while filtering | Clear the filter before deciding it was not retained |
| A field exists in the raw header but not the table | Preserve the file/header and report the missing label/PV; no silent value is fabricated |
| Expected detector is absent | Check original column descriptions and [import diagnostics](import-diagnostics.md); metadata presence alone does not map a numeric signal |

## Keep mapping and inspection work

**Save session** retains original acquisition records and browser source/convention,
tab and docking/visibility choices. A model-only file does not carry the collection's
metadata. Use [sessions and reusable models](sessions-and-models.md) to choose the
appropriate saved artifact. Header right-click lets you adjust scan-table columns
during inspection; that view action does not edit recorded fields.

## Related topics

- [Resolve header, data-row and energy-order import problems](import-diagnostics.md).
- [Choose independent detectors and compare scans](scans-and-channels.md).
- [Inspect reference calibration and processing results](preparing-data.md).
- [Save and reopen the metadata-bearing collection](sessions-and-models.md).
