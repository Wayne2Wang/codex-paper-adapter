---
name: codex-paper-adapter
description: Migrate an existing Overleaf or LaTeX paper to an official conference template while preserving all author-written manuscript and bibliography content. Use when changing a paper's venue or template version, correcting its conference-project structure, or auditing whether an existing migration conforms to the target venue. Do not use to draft, rewrite, or complete the paper.
---

# Codex Paper Adapter

Convert the project structurally and losslessly: the result should use the verified official template for the requested venue, year, and submission stage, while the authors' title, prose, equations, figures, tables, labels, citations, bibliography records, comments, and supplementary material remain unchanged.

## Non-negotiable boundaries

- Do not write, paraphrase, shorten, expand, or "improve" manuscript content.
- Do not invent authors, affiliations, disclosures, checklist answers, acknowledgements, results, citations, or metadata. If the venue requires information that is absent, leave it for the authors and report the exact unresolved requirement.
- Treat user-authored `.tex` and `.bib` material as content even when incomplete. Move it or update references to it, but do not silently delete or rewrite it.
- Distinguish old template assets and untouched sample boilerplate from author content. Remove old venue assets only after proving they are unused and contain no author additions. Preserve any ambiguous file and report it.
- Do not commit, push, change Overleaf settings, or enable camera-ready mode unless the user asks.

## Audit before changing anything

Inspect the repository, its instructions, Git status, and ignore rules. Do not assume `main.tex` is the entry point.

Build a dependency inventory covering:

- root entry-point `.tex` files and any configured Overleaf main document;
- `\input`, `\include`, `\usepackage`, `\documentclass`, bibliography, and graphics paths;
- section files, bibliography databases, figures, supplementary material, custom macros, and build configuration;
- existing venue-specific `.sty`, `.cls`, `.bst`, sample `.tex`, sample `.bib`, and documentation files;
- tracked and untracked user files, including local changes that must be preserved.

Classify each file as author content, project support, target-template material, old-template material, generated build output, or ambiguous. Record a content baseline using Git diff/status and file hashes or copies as appropriate. Compile the original project when practical so migration-only failures can be distinguished from existing failures.

## Normalize the project structure first

If the project already has an equivalent clean structure, preserve it rather than performing a cosmetic reorganization. Otherwise organize it before changing templates:

```text
project/
|-- main.tex                 primary entry point
|-- *.bib                    author bibliography database(s)
|-- sections/                author manuscript and supplement `.tex` files
`-- style_<venue><year>/     files distributed with the official template
```

Keep non-template assets such as figures and data in their existing sensible directories. Do not force a single bibliography file or merge section files. Move files with history-preserving operations where possible, then change only the path references needed to keep the same document connected.

The `style_<venue><year>/` directory should contain the complete official template bundle needed for provenance and reproducibility, including shipped style/class, bibliography-style, helper-package, example, sample-bibliography, and documentation files. Keep downloaded template files byte-for-byte intact. Adapt the project entry point to reference their subdirectory paths instead of editing the official files merely to accommodate the folder layout.

## Identify the correct official template

Determine the exact conference, year, and submission stage from the request and repository. Ask the user only when one of these changes the template or anonymity mode and cannot be determined safely.

Templates and author rules are time-sensitive. Check the live official conference author/submission page and follow its link to the official archive or official Overleaf template. Prefer the conference-controlled source over third-party mirrors or remembered prior-year files. Verify that the downloaded archive and its internal version labels agree with the requested venue/year.

If the requested year's official template has not been released, or official sources conflict, stop before substituting another year. Present the evidence and ask the user which source or fallback to use. Keep a concise record of the source URL and retrieval date for the handoff, but do not add provenance prose to the manuscript.

## Perform a content-preserving migration

Use the official example only to learn required preamble, package, title/author, bibliography, appendix, and submission-mode wiring. Do not copy its example paper text into the manuscript.

Make the smallest structural edits needed to the entry point and project support files:

- replace the old venue's class/style invocation with the target invocation;
- preserve the existing title, author fields, abstract, section inputs, bibliography data, figures, tables, and supplement;
- switch citation or bibliography-style wiring without modifying bibliography records or citation intent;
- retain custom packages and macros unless they conflict with an explicit target rule; resolve conflicts structurally when possible and otherwise report them;
- use the correct review/anonymity mode by default for a submission-template request; enable final mode only when explicitly requested;
- place target-template assets in `style_<venue><year>/` and remove references to the old template;
- add only official, content-free structural hooks that are mandatory to compile or expose a required author-supplied section. Never fill the hook. Prefer reporting a missing substantive requirement to fabricating compliance.

Do not delete the old template bundle until the migrated project builds and a reference search confirms nothing uses it. Delete only files confidently classified as old template assets or generated outputs, never author content. If deletion was not clearly authorized or classification is uncertain, retain the files and tell the user what remains.

## Verify the result

Validate against both the official template and the current author guidelines, not merely successful compilation.

1. Compile from a clean build with the engine and bibliography workflow appropriate to the project.
2. Treat missing files, undefined control sequences, citation-system incompatibilities, accidental final mode, and wrong paper size as migration defects.
3. Inspect the rendered PDF for required headers, line numbers, anonymity, margins, fonts, title layout, references, and appendix ordering. Do not repair content-originated overfull boxes by rewriting prose; report them.
4. Search for stale old-venue names, style paths, checklists, final-copy flags, and bibliography styles. Some occurrences may be legitimate citations or comments; inspect before changing them.
5. Compare the post-migration content inventory with the baseline. Every author content file and bibliography record must remain, with diffs limited to path/wiring changes explicitly required by the migration.
6. Review Git status and diff so generated build artifacts and accidental unrelated changes are absent.

Report the official source used, structural changes, validation performed, old template files removed or retained, and any requirements that still need author input. Describe the project as fully compliant only when every checkable requirement passes; otherwise distinguish template/format compliance from unresolved substantive author obligations.
