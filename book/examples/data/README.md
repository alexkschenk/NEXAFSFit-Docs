# Distributable synthetic example data

These files are generated for the manual; no experimental acquisition is included.

- `two-peak-spectrum.csv`: 221 points, 281–292 eV, step 0.05 eV. Columns are
  `energy_eV` and `intensity`, with one header row and a comma delimiter.
- `two-peak-model.json`: schema-1 GUI starting model using only native
  `edge_cfg` / `peak_specs`, with two Gaussians and a linear background.
- `two-peak-truth.json`: generating parameter values, seed 20261123 and independent
  Gaussian noise standard deviation 0.006. Intensities are arbitrary units.

Regenerate these synthetic assets from the repository root in nexafs-fit:

```bat
python -m tools.create_book_examples
```

The generator also makes `media/images/unit-profiles.png` from the native unit
profiles. It does not overwrite authored Markdown or notebooks.

The series exercise adds `series-01.csv`, `series-02.csv`, `series-03.csv`,
`series-model.json` and `series-truth.json`. Each scan has 221 points on the same
281–292 eV grid. Two Gaussian shapes are shared across scans; P1 area is 1.6 and
P2 areas are 0.45, 0.95 and 0.60. The small baseline is fixed in this example.
Independent Gaussian noise has sigma 0.003 and seed 20261124; illustrative
coordinates are 0, 10, 20 in arbitrary units. Group labels have no experimental
assignment. Regenerate only those assets with:

```bat
python -m tools.create_series_book_example
```

Validate all three notebooks and GUI/native numerical agreement with:

```bat
python -m tools.validate_book_examples
```

Add `--write-outputs` to deliberately save freshly validated outputs in the native
tutorial notebooks. The validator also checks the synthetic series' hard maximum
and mean-based pass-two windows. The older plotting-only notebook is kept unexecuted in source.
Routine Jupyter Book builds do not regenerate data or run these tools.
