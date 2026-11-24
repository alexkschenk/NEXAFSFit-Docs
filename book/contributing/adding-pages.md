# Adding documentation pages and media

Maintainer note: the current reader build is GUI-only. This authoring source and
`python/` sources remain excluded by `project.exclude`; do not restore those reader
routes merely while updating GUI chapters. Restoration requires a deliberate
scope change and notebook validation, documented in `docs/BOOK_DEFERRED_PYTHON.md`.
The README and local USER_GUIDE are entry/quick-reference documents; detailed reader
procedures belong in one canonical Book chapter.

All book source files live under `docs/book`. Edit them in VS Code or JupyterLab;
run `jupyter book start` inside `docs/book` to preview, using the same `nexafs-fit`
environment as the GUI. From the repository root, `run_docs.ps1` also launches
the preview and handles Windows UTF-8 output. The developer setup guide is
`docs/BOOK-START-HERE.md` (outside the public book).

## Scope and status

Use `gui/` for GUI tutorials, `python/` for independent numerical workflows,
`reference/` for shared science, and `examples/` for complete analyses. Keep one
canonical explanation and link to it from both interfaces. Internal architecture,
source compatibility records and development logs stay outside this book.

For a finished tutorial, state the software version and validation performed.
Use a short opening callout to mark unfinished content, for example:

```text
:::{note} Status: draft
This chapter is a template; its workflow has not yet been validated.
:::
```

Use **partial** for a workflow with explicitly identified unimplemented steps,
and **planned** for future material. A draft chapter does not imply that the
underlying software feature is unavailable. Do not present a planned Windows
executable or future fitting example as something readers can already download.

## Add a new page

1. Create a `.md` file in the appropriate folder, with one top-level `# Heading`.
2. Add the path to `project.toc` in `myst.yml`.
3. Preview the page, check its links, and include it in a normal Git commit with the matching code change.

## Page structure and related navigation

Place a chapter where readers need it: data/preparation before modelling, parameter
basics before advanced editing, single fits/results before series/two-pass workflows.
Keep native scientific notebooks in Python and Jupyter; rendering-only examples
belong with authoring material. Use the TOC hierarchy to organise pages while
preserving their existing filenames and URLs.

Give each topic one main explanatory home. Other pages should link to that section
and retain only the context needed for their own task. At the start, link any
prerequisite a reader may have skipped by entering through search or a direct link.
Beside a control or scientific distinction, link the procedure/reference that
answers the likely follow-up. End each non-home chapter with **Related topics**:
choose a few useful companion tasks rather than copying the whole sidebar.

Use descriptive link text such as “set independent position bounds” or “interpret
joint fit statistics.” Preserve existing section anchors when changing headings.
An explicit MyST label before a heading can keep an established target:

```text
(existing-topic-anchor)=
## A clearer section title
```

Notebook navigation is editable Markdown too. Change its text cells without
rewriting scientific code/outputs unnecessarily, and use project-unique cell IDs.

After building, run the navigation audit from the repository root in nexafs-fit:

```bat
python -m tools.validate_book_navigation
```

It checks active local links/media, rendered section anchors, unique TOC membership,
home-page reachability and companion-page routes on every chapter. Its minimum
of two distinct related pages prevents dead ends; it is a floor, not a reason
to add unrelated links. Scientific correctness and whether a link answers the
reader's question still need editorial review.

## Reusable visual components

Use cards for chapter entry points, short callouts for cautions, and tabbed blocks when showing equivalent GUI/Python approaches.

The landing page provides a reusable card/grid pattern. The tabs below demonstrate
the layout only; they do not describe a validated fitting workflow.

::::{tab-set}
:::{tab-item} GUI
Document click-by-click instructions, annotated screenshots, and short videos.
:::
:::{tab-item} Python
Document the validated function calls, executable cells, and expected results.
:::
::::

Use one `#` title, descriptive `##` sections, relative page links and stable,
unique figure/equation labels. Keep layouts in standard MyST rather than custom
JavaScript or CSS. For example, a **layout-only** parameter table:

| Parameter | Meaning | Units / convention |
| --- | --- | --- |
| _Exact API name_ | _Verified scientific definition_ | _Units, scaling and bounds_ |

For equations, use inline `$...$` or a labelled block:

```text
:::{math}
:label: equation-example
y = a x + b
:::
```

Replace example mathematics with verified implementation-specific definitions;
do not infer a lineshape convention from a generic textbook formula.

## Screenshots

Put image files under `media/images/`. Reference them relatively, for example:

```text
![Peak editor with an asymmetric component](../media/images/peak-editor.png)
```

Avoid screenshots containing unpublished data, private paths or credentials.

## Videos

Put short MP4 clips under `media/videos/` and embed them with MyST:

```text
:::{figure} ../media/videos/peak-editor-demo.mp4
:label: video-model-editor
Adjusting a model component in the editor.
:::
```

Longer hosted video tutorials can be embedded using MyST's `iframe` directive. Do not add references until the media exists.

## New notebook chapters

Write notebooks in JupyterLab or VS Code and add their `.ipynb` paths to `myst.yml`.
Use actual NEXAFSFit APIs verified against the numerical source and tested examples.
The existing synthetic-spectrum notebook is explicitly a **rendering demo** and
does not validate the engine. New scientific tutorials should keep that distinction.

Prefer small deterministic synthetic datasets. Keep associated data under
`examples/data/`, with units, columns, generation seeds and provenance. Document
working-directory assumptions and keep paths portable. Do not copy private
acquisitions, absolute workstation paths or the full research notebook into the
book. Keep Qt dependencies out of independent numerical examples.

Run JupyterLab from the activated `nexafs-fit` environment. Inspect
`jupyter kernelspec list --json` if a kernel is ambiguous, and check
`import sys; print(sys.executable)` interactively before running an example.
If the environment needs a named kernel, register it from the activated prompt:

```bat
python -m ipykernel install --sys-prefix --name nexafs-fit --display-name "Python (nexafs-fit)"
```

Then select it in the notebook and save the kernel metadata. Do not commit the
local executable path printed during inspection. The demo's `python3` kernel
must likewise resolve to this environment when executing it.

## Local validation

From the repository root, run:

```bat
powershell -NoProfile -ExecutionPolicy Bypass -File .\run_docs.ps1 -Build
```

Inspect the preview for navigation, readable equations, tabs, media captions and
mobile-width layout. The strict build checks configuration and internal references;
it does not prove scientific correctness or validate every external URL.

When changing runnable examples, deliberately validate execution:

```bat
powershell -NoProfile -ExecutionPolicy Bypass -File .\run_docs.ps1 -Build -Execute
```

Execution is opt-in and uses MyST's `_build/execute/` cache. After changing the
engine or environment, force a fresh execution from `docs/book` with
`jupyter book clean --execute` before rebuilding with `--execute`. Normal builds
do not run code and display outputs saved in the source notebook. Use `--execute`
to include outputs from MyST's execution cache; a new unexecuted source notebook
otherwise has no plots. Review outputs before publishing, and keep
expensive global fitting examples out of the routine execution path (MyST supports
notebook execution-skip metadata).

On Windows, the launcher gives each run its own Jupyter runtime folder under
`_build/`, keeps IPython files there,
and prefers IPv4 for Node.js localhost connections. These settings apply only
to its child process. If running MyST execution manually, use equivalent writable
runtime paths and localhost settings rather than changing the scientific code.
The launcher supplies a per-run temporary Jupyter configuration binding its
execution server to `127.0.0.1`; this avoids a Windows localhost connection mismatch.
It restores `JUPYTER_CONFIG_DIR` afterward and does not edit a user's permanent
configuration. The direct notebook validator is also available when diagnosing
execution independently of MyST's server connection.

See the official [Jupyter Book 2 guide](https://jupyterbook.org/stable/) and
[MyST notebook execution guide](https://mystmd.org/guide/execute-notebooks)
for additional authoring and execution options. Record validation and relevant
tool/software versions with documentation changes.

## Before publishing

The GUI/native examples are maintained with these repository tools:

```bat
python -m tools.create_book_examples
python -m tools.create_series_book_example
python -m tools.validate_book_examples
```

The first two regenerate the respective synthetic assets (and the first example's
native profile figure); they do not rewrite authored chapters or notebooks. The
validator executes both native fitting notebooks and the rendering demo in nexafs-fit,
compares native/GUI fitted values, bounds/states and complete spectra, and checks
the synthetic GUI series hard-maximum/two-pass recipe. Its small fits are explicit
example validation, not experimental collection runs. Add `--write-outputs` only
when deliberately refreshing the native tutorials' saved outputs. Routine builds
use those saved outputs without fitting;
the older rendering demo remains unexecuted in source.

Ensure statements reflect current functionality rather than planned features. Run a local build, test links, validate relevant notebooks and confirm that all included data and media can be publicly shared.

## Related topics

- [See a rendering-only notebook presentation example](../python/synthetic-spectrum.ipynb).
- [Use validated scientific examples as authoring references](../examples/index.md).
- [Keep scientific conventions consistent with the manual](../reference/lineshapes.md).
