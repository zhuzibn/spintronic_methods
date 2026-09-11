# Project Agent Instructions

## LaTeX workflow

- After modifying LaTeX source, do not compile the document, render generated
  output, or perform output-validation checks by default. The user will compile
  and inspect the document manually.
- Run compilation or generated-output validation only when the user explicitly
  requests it.
- A background Windows MiKTeX integration may automatically rebuild
  `spintronic_methods.pdf` after LaTeX source changes. Keep that auto-built PDF
  in the working tree instead of restoring it. Treat the automatic rebuild as
  unverified until its build result or generated output has been checked.
- Before every commit, rebuild `spintronic_methods.pdf` from the current LaTeX
  source, verify that the build succeeds, stage the PDF with the other changes,
  and include it in the commit. When pushing the commit, push the PDF to GitHub
  as part of that commit so the current rendered document is publicly available.
  This commit-time requirement is an explicit exception to the default
  no-compilation rule above.

## Reference style

- When the user asks to refer to a paper, first search
  `C:\Users\zzf-m\OneDrive\papers\_knowledge` to determine whether that paper
  has already been processed.
- Every equation, quantitative value, nontrivial physical claim, and
  source-derived figure must have a nearby citation or explicit source reference
  to at least one of the following: the EE237 lecture notes, a paper authored or
  coauthored by Zhifeng Zhu, or any repository, implementation, or source file
  hosted under Zhifeng Zhu's GitHub namespace at `https://github.com/zhuzibn/`,
  including shared utilities such as `constantfile.m`.
- Content supplied directly by the user may be added provisionally without a
  reference. In that case, place a visible `Reference needed` warning near the
  unsupported content and report the warning in the final response so the user
  can provide a source later. Do not add an indirect or weak citation merely to
  remove the warning.
- When the user asks the agent to provide knowledge and no approved reference is
  available, add the requested content provisionally before asking the user for
  a source instead of omitting the content. Place a visible `Reference needed`
  warning near the unsupported content and report the warning in the final
  response so the user can provide a source later.
- For an original schematic, identify it as original and cite the source that
  governs its physical convention. If that source is not available, give the
  figure the same visible `Reference needed` warning.
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
