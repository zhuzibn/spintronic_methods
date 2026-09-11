# Environment Requirements

This file is the authoritative environment specification for building the
Spintronic Methods library. `README.md` documents the build workflows and
repository layout.

## Required capabilities

Building `spintronic_methods.pdf` requires:

- `pdflatex`;
- `kpsewhich` for deterministic package discovery; and
- the LaTeX files `amsmath.sty`, `amssymb.sty`, `bm.sty`, `geometry.sty`,
  `graphicx.sty`, and `hyperref.sty`.

Rendering standalone equation SVGs additionally requires:

- Bash;
- GNU `realpath`;
- `latex`;
- `dvisvgm`; and
- the LaTeX file `standalone.cls`.

No minimum tool versions are currently specified.

## Optional capabilities

`latexmk` is recommended for complete-document builds. If it is unavailable,
running `pdflatex` twice is the supported fallback.

Inkscape is needed only to regenerate a PDF figure companion after editing its
SVG source. It is not needed to build `spintronic_methods.pdf` from committed
sources.

## Ubuntu and WSL setup

Install the required toolchain with:

```bash
sudo apt-get update
sudo apt-get install texlive-latex-base texlive-latex-recommended texlive-latex-extra dvisvgm
```

Install the optional build helper with:

```bash
sudo apt-get install latexmk
```

To refresh PDF figure companions from their editable SVG sources, install the
optional figure-export tool with:

```bash
sudo apt-get install inkscape
```

## Read-only verification

Check the required commands without producing build artifacts:

```bash
command -v bash
command -v realpath
command -v pdflatex
command -v latex
command -v dvisvgm
command -v kpsewhich
```

Check the required LaTeX files:

```bash
kpsewhich amsmath.sty
kpsewhich amssymb.sty
kpsewhich bm.sty
kpsewhich geometry.sty
kpsewhich graphicx.sty
kpsewhich hyperref.sty
kpsewhich standalone.cls
```

Run the canonical read-only environment profile from the dotfiles repository:

```bash
bash scripts/check-environments.sh equations
```

The actual PDF and SVG build commands write generated files. Run them only when
build validation is intended; they remain documented in `README.md`.
