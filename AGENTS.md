# Project Agent Instructions

## LaTeX workflow

- After modifying LaTeX source, do not compile the document, render generated
  output, or perform output-validation checks by default. The user will compile
  and inspect the document manually.
- Run compilation or generated-output validation only when the user explicitly
  requests it.
- Before every commit, rebuild `equations.pdf` from the current LaTeX source,
  verify that the build succeeds, stage the PDF with the other changes, and
  include it in the commit. When pushing the commit, push the PDF to GitHub as
  part of that commit so the current rendered document is publicly available.
  This commit-time requirement is an explicit exception to the default
  no-compilation rule above.

## Reference style

- Every equation must have a nearby citation or explicit source reference to at
  least one of the following: the EE237 lecture notes, a paper authored or
  coauthored by Zhifeng Zhu, or any repository, implementation, or source file
  hosted under Zhifeng Zhu's GitHub namespace at `https://github.com/zhuzibn/`,
  including shared utilities such as `constantfile.m`.
- Store bibliography entries in `references.tex` and cite them from the document
  with `\cite{...}`.
- Ensure every citation used in the document appears in the reference section.
- Format journal references as: `author list, paper title, journal name, volume,
  pages or article number (year)`.
- Format each author as `<first initial(s)>. <family name>` and separate authors
  with commas.
- Use the standard abbreviated journal name without italics and set only the
  journal volume in bold.
- Omit the issue and DOI unless the user requests them; do not bold the page or
  article number or year.
- Follow this LaTeX pattern:
  `A. Author, B. Author, Paper title, J. Abbrev., \textbf{12}, 345678 (2026).`
