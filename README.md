# Spintronic Methods

This repository is a reusable, LaTeX-first collection of illustrated concepts
and equations for spin Hamiltonians, magnetization dynamics, spin--orbit torque,
and thermal stability. The `.tex` files and editable figure SVGs are the source
of truth. The complete
`spintronic_methods.pdf` is rebuilt, verified, and committed with each repository
commit so the current rendered document is available on GitHub. Editable figure
SVGs and their LaTeX-ready PDF companions are also committed. Other generated PDF
and SVG files are reproducible build artifacts and should not be committed.

## Repository layout

- `spintronic_methods.tex` — main document, package setup, and section order.
- `spintronic_methods.pdf` — tracked rendering of the complete document, rebuilt
  for each commit.
- `sections/spintronics_concepts.tex` — illustrated spintronics concepts,
  including A-type antiferromagnetic order.
- `images/` — editable SVG figure sources and same-named PDF companions included
  by the LaTeX document.
- `macros.tex` — shared vector, derivative, and spintronics commands.
- `references.tex` — numbered references cited by the document sections.
- `symbols.tex` — parameter definitions, physical constants, units, and field
  conventions.
- `sections/symmetries.tex` — time-reversal, inversion, mirror, spin-rotation,
  and combined parity--time symmetries for spin systems.
- `sections/material_parameters.tex` — shared heavy-metal spin-Hall angles and
  literature- and implementation-sourced parameter sets for CoFeB, GdFeCo, and
  Mn$_3$Sn, with model and unit conventions.
- `sections/hamiltonian.tex` — atomistic spin Hamiltonian.
- `sections/macrospin_effective_field.tex` — one- and two-sublattice macrospin
  energies and effective fields, including anisotropy, demagnetization,
  dipolar coupling, thermal fluctuations, and inter-sublattice exchange.
- `sections/llg_equation.tex` — LLGS and equivalent LL equations with field-like
  torque, including the torque amplitudes and sign convention.
- `sections/spin_torque.tex` — reusable damping-like and field-like spin--orbit
  torque expressions not included in the complete document.
- `sections/thermal_stability.tex` — thermal stability factor.
- `sections/thermal_stability_delta.tex` — reusable equation fragment for the
  thermal stability factor.
- `sections/build-equation.sh` — renders one equation fragment as an SVG.
- `.gitignore` — excludes LaTeX intermediates and reproducible rendered outputs.

## Requirements

See `ENVIRONMENT.md` for the authoritative list of required and optional tools,
LaTeX packages, setup commands, and read-only verification commands. On Windows,
run the SVG build script from WSL or another Bash environment.

## Build the complete document

Run the build from the repository root:

```bash
latexmk -pdf spintronic_methods.tex
```

If `latexmk` is unavailable, run `pdflatex` twice so that the table of contents and
cross-references are resolved:

```bash
pdflatex spintronic_methods.tex
pdflatex spintronic_methods.tex
```

The output is `spintronic_methods.pdf`. Commit this complete-document PDF;
auxiliary LaTeX files remain ignored by Git.

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
figure; building the complete document still requires only the LaTeX toolchain.

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
3. Add a new topic file to `spintronic_methods.tex` with
   `\input{sections/topic_name}`.
4. Give equations that will be cross-referenced a stable, unique label such as
   `eq:llg-sot`.
5. Record definitions, assumptions, units, sources, sign conventions, and
   implementation notes where they matter.
6. Put repeated notation in `macros.tex`, not in an individual topic file.
7. Build the complete document and, when applicable, render the standalone SVG.

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
- `spintronic_methods.tex`, `spintronic_methods.pdf`, `macros.tex`, and
  `symbols.tex`;
- source `.tex` files under `sections/`;
- editable SVG figures and their same-named PDF companions under `images/`; and
- `sections/build-equation.sh`.

Before every commit, rebuild `spintronic_methods.pdf`, verify that the build
succeeds, and include the PDF in the commit. Do not commit other reproducible
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
