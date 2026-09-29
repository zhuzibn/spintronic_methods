# Spintronic Methods

This repository contains two LaTeX documents: `spintronic_methods.tex` is a
spin-dynamics modeling reference covering notation, material parameters,
Hamiltonians, magnetization dynamics, and thermal stability;
`spintronics_background.tex` covers spintronics concepts, symmetry, and
unconventional spin--orbit torque. The
`.tex` files and editable figure SVGs are the source of truth. Both document
PDFs are rebuilt, verified, and committed with each repository commit so the
current rendered references are available on GitHub. Editable figure SVGs and
their LaTeX-ready PDF companions are also committed. Other generated PDF and
SVG files are reproducible build artifacts and should not be committed.

## Repository layout

- `spintronic_methods.tex` — spin-dynamics modeling reference, sections 1–5 of
  the former combined document.
- `spintronic_methods.pdf` — tracked rendering of the modeling reference,
  rebuilt for each commit.
- `spintronics_background.tex` — spintronics concepts, symmetry, and unconventional
  spin--orbit torque document.
- `spintronics_background.pdf` — tracked rendering of the concepts and
  symmetry document, rebuilt for each commit.
- `sections/spintronics_concepts.tex` — illustrated spintronics concepts,
  including A-type antiferromagnetic order.
- `images/` — editable SVG figure sources and same-named PDF companions included
  by the LaTeX document.
- `macros.tex` — shared vector, derivative, and spintronics commands.
- `references.tex` — shared numbered references cited by both documents.
- `symbols.tex` — shared parameter definitions, physical constants, units, and
  field conventions, included in both documents.
- `sections/symmetries.tex` — a symmetry-for-spintronics tutorial covering
  time reversal, inversion, parity--time symmetry, mirror and spin-rotation
  rules, plus symmetry selection rules for Rashba SOC, DMI, spin and anomalous
  Hall effects, Edelstein response, SOT, STT, and altermagnetic spin splitting.
- `sections/unconventional_sot.tex` — routes to out-of-plane spin polarization
  through crystal symmetry, magnetic order, and electrical asymmetry.
- `interactive/z-axis-rotation.html` — a dependency-free interactive companion
  for the active $z$-axis rotation derivation, with animated basis vectors,
  Cartesian projections, and a live rotation matrix.
- `sections/material_parameters.tex` — shared heavy-metal spin-Hall angles and
  literature- and implementation-sourced parameter sets for CoFeB, GdFeCo,
  Mn$_3$Sn, and layered van der Waals antiferromagnets, with model and unit
  conventions.
- `sections/hamiltonian.tex` — atomistic spin Hamiltonian.
- `sections/atomistic_boundaries.tex` — staged periodic-to-open boundary
  workflow for bulk verification, finite-device studies, DMI edges, and
  finite-size checks.
- `sections/macrospin_effective_field.tex` — one- and two-sublattice macrospin
  energies and effective fields, including anisotropy, demagnetization,
  dipolar coupling, thermal fluctuations, and inter-sublattice exchange.
- `sections/llg_equation.tex` — LLGS and equivalent LL equations with field-like
  torque, including the torque amplitudes and sign convention.
- `sections/spin_torque.tex` — reusable damping-like and field-like spin--orbit
  torque expressions not included in either document.
- `sections/thermal_stability.tex` — thermal stability factor.
- `sections/thermal_stability_delta.tex` — reusable equation fragment for the
  thermal stability factor.
- `sections/build-equation.sh` — renders one equation fragment as an SVG.
- `.gitignore` — excludes LaTeX intermediates and reproducible rendered outputs.

## Requirements

See `ENVIRONMENT.md` for the authoritative list of required and optional tools,
LaTeX packages, setup commands, and read-only verification commands. On Windows,
run the SVG build script from WSL or another Bash environment.

## Build the two documents

Run the build from the repository root:

```bash
latexmk -pdf spintronic_methods.tex
latexmk -pdf spintronics_background.tex
```

If `latexmk` is unavailable, run `pdflatex` twice so that the table of contents and
cross-references are resolved:

```bash
pdflatex spintronic_methods.tex
pdflatex spintronic_methods.tex
pdflatex spintronics_background.tex
pdflatex spintronics_background.tex
```

The outputs are `spintronic_methods.pdf` and `spintronics_background.pdf`.
Commit both PDFs; auxiliary LaTeX files remain ignored by Git. The shared
notation and reference list appear in both documents. References to material
in the other document use its title rather than a LaTeX cross-reference.

## Refresh a figure PDF

Each figure keeps an editable SVG source and a same-named PDF companion that
`pdflatex` can include without conversion. After editing an SVG, export its PDF
companion with Inkscape, for example:

```bash
inkscape images/a-type-afm-stacking.svg \
  --export-filename=images/a-type-afm-stacking.pdf
```

Commit the SVG and PDF together. The PDF companion is a required document input,
not a disposable rendered output. Inkscape is needed only when refreshing a
figure; building either document still requires only the LaTeX toolchain.

## Build one equation as an SVG

An equation fragment contains only the mathematical expression that will appear
inside inline math delimiters. For example,
`sections/thermal_stability_delta.tex` can be rendered with:

```bash
cd sections
./build-equation.sh thermal_stability_delta
```

The first argument is the fragment filename without `.tex`. An optional second
argument changes the output path:

```bash
./build-equation.sh thermal_stability_delta custom-name.svg
```

The script must be run from `sections/`. It writes temporary files under
`sections/.equation-build/` and writes the SVG relative to the current directory.
Both the temporary directory and SVG files under `sections/` are ignored by Git.

## Add an equation

1. Choose an existing topic file or create `sections/topic_name.tex`.
2. If the equation also needs a standalone SVG, put the expression in a small
   fragment such as `sections/equation_name.tex` and include that fragment from the
   topic file.
3. Add a new topic file to the appropriate document entry point with
   `\input{sections/topic_name}`.
4. Give equations that will be cross-referenced a stable, unique label such as
   `eq:llg-sot`.
5. Record definitions, assumptions, units, sources, sign conventions, and
   implementation notes where they matter.
6. Put repeated notation in `macros.tex`, not in an individual topic file.
7. Build the affected document and, when applicable, render the standalone SVG.

Suggested entry structure:

```latex
\subsection{Equation Name}

\paragraph{Equation.}
\begin{equation}
    % Equation goes here.
    \label{eq:unique-name}
\end{equation}

\paragraph{Definitions.}
% Define every symbol that is not already in symbols.tex.

\paragraph{Assumptions.}
% State the model assumptions and validity range.

\paragraph{Units and sign convention.}
% State the unit system and any convention-sensitive signs.

\paragraph{Source.}
% Add a paper, textbook, DOI, or derivation reference.

\paragraph{Implementation notes.}
% Record normalization and differences from simulation code.
```

## Version-control policy

Commit the source and workflow files:

- `.gitignore` and `README.md`;
- both document `.tex` entry points and their PDFs, plus `macros.tex` and
  `symbols.tex`;
- source `.tex` files under `sections/`;
- editable SVG figures and their same-named PDF companions under `images/`; and
- `sections/build-equation.sh`.

Before every commit, rebuild both document PDFs, verify that both builds
succeed, and include the PDFs in the commit. Do not commit other reproducible
outputs or intermediates, including LaTeX auxiliary and log files,
`sections/.equation-build/`, or generated SVG files under `sections/`. The PDF
figure companions under `images/` are required inputs and are the exception to
this generated-output rule.

## Maintenance rules

- Change shared notation once in `macros.tex`.
- Do not reuse an equation label.
- Preserve the original source convention and document any conversion explicitly.
- Replace placeholder source paragraphs with real citations before relying on an
  equation.
- Keep a visible `Reference needed` warning beside provisionally unreferenced
  user-supplied content and report each warning during handoff.
- When requested knowledge has no approved source, include it provisionally with
  a visible `Reference needed` warning instead of omitting it.
- Regenerate and commit a figure's PDF companion whenever its SVG source changes.
- Check dimensions and limiting cases before using an equation in code.
- Keep generated outputs reproducible from the committed source and scripts.
