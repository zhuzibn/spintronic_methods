# Spin Dynamic Methods

This repository is a reusable, LaTeX-first collection of equations for spin
Hamiltonians, magnetization dynamics, spin--orbit torque, and thermal stability.
The `.tex` files are the source of truth. The complete `equations.pdf` is rebuilt,
verified, and committed with each repository commit so the current rendered
document is available on GitHub. Other generated PDF and SVG files are
reproducible build artifacts and should not be committed.

## Repository layout

- `equations.tex` — main document, package setup, and section order.
- `equations.pdf` — tracked rendering of the complete document, rebuilt for each
  commit.
- `macros.tex` — shared vector, derivative, and spintronics commands.
- `references.tex` — numbered references cited by the equation sections.
- `symbols.tex` — parameter definitions, physical constants, units, and field
  conventions.
- `sections/hamiltonian.tex` — atomistic spin Hamiltonian.
- `sections/macrospin_effective_field.tex` — one- and two-sublattice macrospin
  energies and effective fields, including anisotropy, demagnetization,
  dipolar coupling, and inter-sublattice exchange.
- `sections/llg_equation.tex` — LLGS and equivalent LL equations with field-like
  torque, including the torque amplitudes and sign convention.
- `sections/spin_torque.tex` — reusable damping-like and field-like spin--orbit
  torque expressions not included in the complete document.
- `sections/thermal_stability.tex` — thermal stability factor and macrospin thermal
  field correlation.
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
latexmk -pdf equations.tex
```

If `latexmk` is unavailable, run `pdflatex` twice so that the table of contents and
cross-references are resolved:

```bash
pdflatex equations.tex
pdflatex equations.tex
```

The output is `equations.pdf`. Commit this complete-document PDF; auxiliary LaTeX
files remain ignored by Git.

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
3. Add a new topic file to `equations.tex` with
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
- `equations.tex`, `equations.pdf`, `macros.tex`, and `symbols.tex`;
- source `.tex` files under `sections/`; and
- `sections/build-equation.sh`.

Before every commit, rebuild `equations.pdf`, verify that the build succeeds, and
include the PDF in the commit. Do not commit other reproducible outputs or
intermediates, including LaTeX auxiliary and log files,
`sections/.equation-build/`, or generated SVG files under `sections/`.

## Maintenance rules

- Change shared notation once in `macros.tex`.
- Do not reuse an equation label.
- Preserve the original source convention and document any conversion explicitly.
- Replace placeholder source paragraphs with real citations before relying on an
  equation.
- Check dimensions and limiting cases before using an equation in code.
- Keep generated outputs reproducible from the committed source and scripts.
