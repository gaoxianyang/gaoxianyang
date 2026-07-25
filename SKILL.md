---
name: math-book-autoloop
description: Build and maintain a complete PhD-seminar-style augmentation of any mathematics book in a strict autonomous loop. Use when the user asks to continue, auto-continue, or systematically expand a math book into detailed lecture notes that cover definitions, theorems, propositions, lemmas, corollaries, examples, remarks, and important exercises with full proofs, hidden steps, source citations, chapter gap maps, gap schemas, scaffold/expansion layer separation, local and network knowledge-base retrieval, a mandatory knowledge-simplex sidecar, segmented delivery, iterative content review, iterative typography review, and final XeLaTeX PDF output.
---

# Math Book Autoloop

Use this skill for any mathematics-book seminar-note project.

## Minimal context

Resolve or create:

- `BOOK_TITLE`
- `SOURCE_PDF`
- `PROJECT_ROOT`
- `MAIN_TEX`
- `CHAPTER_FILES`
- `TRACKING_FILES`
- `KNOWLEDGE_JSON`
- `KNOWLEDGE_MARKDOWN`
- `PROJECT_PHASE` (`scaffold` or `expansion`)
- `RUN_MODE` (`scaffold`, `expansion`, `exercise-repair`, or `backscan-audit`)
- `CHAPTER_GAP_MAP`
- `GAP_SCHEMA`
- `OUTPUT_PDF`

Prefer project context files before inference. If `KNOWLEDGE_JSON` or `KNOWLEDGE_MARKDOWN` is missing, switch to `scaffold` first and initialize the sidecar layer before any new accepted source coverage.

## Non-negotiables

- Output is augmentation, not summary.
- Preserve original numbering and local order; keep labels unchanged and put anything outside the active slice in `defer_list`.
- Cover both numbered items and unnumbered ideas such as motivation, caveat, proof trick, notation nuance, dependency, and author strategy.
- Formal mathematical environments must be complete: definition/theorem/proposition/lemma/corollary/example/problem/exercise entries must include the full statement or prompt before any proof, verification, or solution.
- Examples are hard requirements: verify every included example step by step, checking each defining condition, map property, equality, containment, and claimed conclusion explicitly.
- Problems and exercises are hard requirements: classify each one as `solvable`, `partially solvable`, or `currently blocked`; fully solvable items must receive complete step-by-step solutions, while partial or blocked items must state the exact obstruction without faking progress.
- Do not replace a locally available full verification or solution with hints, sketches, role summaries, “similar argument”, “easy verification”, companion-only deferrals, or placeholder prose.
- Technical items must be expanded, not lightly glossed. At minimum they need proof strategy, hidden steps, hypothesis usage, omitted verifications/calculations, and later use when mathematically relevant.
- Keep workflow metadata (`Gate A/B/C`, self-checks, audit tables, revision notes, change logs) out of manuscript正文 and out of the final PDF.
- Never write `BOOK_TITLE` or the original book title into the manuscript body, chapter titles, section titles, running headers, title pages, PDF metadata, or generated article titles. Keep it only in project/tracking metadata when needed for source traceability; use the mathematical topic or source-slice label as the visible title instead.
- Maintain mathematical terminology consistently: first appearance is Chinese + English, then stable usage thereafter.
- Add diagrams when they materially reduce cognitive load. Prefer `tikz-cd` for commutative/categorical diagrams, `pgfplots` or plain `TikZ` for graphs/regions, and TikZ graph/positioning for graph-like figures.
- Treat the knowledge-simplex layer as mandatory: every accepted lecture-note slice must also update one AI-readable JSON fact source and one Obsidian-first Markdown rendering layer. The structure layer does not replace prose proofs, example verification, exercise solutions, or PDF output.

## Final-manuscript presentation boundary

The rendered manuscript/PDF must read as a self-contained standard mathematical lecture note, never as a source transcription, source comparison, production log, or revision plan.

- Use ordinary mathematical environments and direct mathematical prose for the visible manuscript. A definition, theorem, proposition, lemma, corollary, example, problem, or exercise must be titled only by its mathematical environment and number; do not prefix it with its source relationship.
- Do not render source-facing labels such as `原文陈述`, `Source statement`, `原书定义`, `原书定理`, `原书命题`, `原书引理`, `原书推论`, `原书例`, `原书题目`, `原书练习`, or equivalent Chinese/English wording. Do not use `sourceblock` in generated manuscript files.
- Do not render workflow-facing language such as `本轮`, `下一轮`, `后续习题修订`, `待修订`, `待补充`, `审核`, `验收`, `Gate`, `TODO`, `change log`, or equivalent planning/audit language.
- Put source identity, source-item mapping, revision history, future work, audit decisions, and all equivalent traceability information only in tracking files, the JSON/Obsidian sidecar, or TeX comments that do not render. Never use a visible box, heading, title, running header, footnote, caption, or PDF metadata for this purpose.
- When an existing project contains a visible source label or workflow label, preserve its mathematical statement/proof/solution content but rewrite it into the ordinary mathematical environment or prose that the reader needs; delete the visible label rather than merely hiding it with empty text or white styling.
- Before accepting a slice, inspect both the manuscript source and extracted final-PDF text for the prohibited source-facing and workflow-facing labels. A clean `.tex` file alone is insufficient because labels can come from macros, headers, bookmarks, or PDF metadata.

## Modes, evidence, and sidecar contract

- `RUN_MODE`:
  - `scaffold`: initialize or repair infrastructure only; no accepted source coverage advances.
  - `expansion`: normal source-ordered正文 expansion.
  - `exercise-repair`: focus on examples/problems/exercises in the target range.
  - `backscan-audit`: audit and repair an already-covered range without widening the frontier.
- Evidence order: source PDF -> local knowledge base -> network knowledge base -> conservative synthesis.
- Every loop that changes mathematical content must do both:
  - explicit local retrieval from source/project materials, and
  - explicit authoritative external cross-verification, unless a concrete exception is recorded.
- Record every external claim with URL, supported claim, and why it was relevant. If web verification fails or no suitable authority exists, record the exception and use conservative wording.
- Sidecar division of labor:
  - manuscript layer: formal statements, proofs, example verification, exercise solutions, diagrams, PDF output;
  - structure layer: vertices, edges, simplices, gluings, anti-simplices, do-not-glue constraints, terminology, proof/example/exercise structures, gaps/frontiers, traceability, and autoloop feedback.
- Sidecar scope firewall:
  - only the current source slice may be treated as covered;
  - future material may appear only as `future_hint`, `frontier`, or `gap`;
  - future-hint content cannot act as a current proof dependency.
- In new projects, initialize `MAIN_TEX`, `KNOWLEDGE_JSON`, and `KNOWLEDGE_MARKDOWN` together. Missing structure files are a scaffold defect, not something to silently skip.
- When the host permits subagents, reuse a small stable set for bounded sidecar work such as source extraction, terminology/web verification, or typography/render checks. Keep mathematical judgment and final acceptance local.

## Shared LaTeX template

- Use exactly one canonical shared lecture-note template.
- New projects start from this template core unless a clearly better house style already exists.
- Existing projects should absorb its environment/layout layer rather than spawning a parallel template.
- Keep the look quiet and mathematical: readable before pretty, stable before fancy.
- Treat the manuscript first as continuously readable mathematical Chinese. Use display math only when length, alignment, numbering, or emphasis genuinely improves clarity; otherwise keep formulas inline.

Use this built-in template skeleton by default:

```tex
\documentclass[11pt,a4paper,openany]{ctexbook}
\usepackage[a4paper,margin=28mm,headheight=15pt,footskip=12mm]{geometry}
\usepackage{setspace,microtype}
\setstretch{1.12}
\setlength{\parindent}{2em}
\setlength{\parskip}{0.25em}

\usepackage{amsmath,amssymb,amsthm,mathtools,bm,mathrsfs}
\numberwithin{equation}{chapter}
\allowdisplaybreaks[2]
\usepackage{enumitem,array,booktabs,graphicx,caption,xcolor}
\usepackage{tikz,tikz-cd,pgfplots}
\usetikzlibrary{arrows.meta,calc,matrix,positioning,graphs}
\pgfplotsset{compat=1.18}
\usepackage[most]{tcolorbox}
\usepackage{thmtools}
\usepackage{hyperref}
\usepackage[nameinlink,noabbrev]{cleveref}
\usepackage{fancyhdr}

\declaretheoremstyle[headfont=\bfseries\color{MidnightBlue},bodyfont=\itshape]{seminarplain}
\declaretheoremstyle[headfont=\bfseries\color{ForestGreen!60!black},bodyfont=\normalfont]{seminardef}
\declaretheoremstyle[headfont=\bfseries\color{BrickRed},bodyfont=\normalfont]{seminarexample}
\declaretheoremstyle[headfont=\bfseries\color{black!70},bodyfont=\normalfont]{seminarremark}

\declaretheorem[name=定理,numberwithin=chapter,style=seminarplain]{theorem}
\declaretheorem[name=命题,sibling=theorem,style=seminarplain]{proposition}
\declaretheorem[name=引理,sibling=theorem,style=seminarplain]{lemma}
\declaretheorem[name=推论,sibling=theorem,style=seminarplain]{corollary}
\declaretheorem[name=定义,sibling=theorem,style=seminardef]{definition}
\declaretheorem[name=构造,sibling=theorem,style=seminardef]{construction}
\declaretheorem[name=例,sibling=theorem,style=seminarexample]{example}
\declaretheorem[name=题目,sibling=theorem,style=seminarexample]{problem}
\declaretheorem[name=习题,sibling=theorem,style=seminarexample]{exercise}
\declaretheorem[name=注记,sibling=theorem,style=seminarremark]{remark}
\declaretheorem[name=记号,sibling=theorem,style=seminarremark]{notation}
\renewcommand{\proofname}{证明}

\tcbset{seminarblock/.style={enhanced,breakable,frame hidden,boxrule=0pt,sharp corners,left=2.2mm,right=1.2mm,top=1mm,bottom=1mm,before skip=0.75em,after skip=0.85em}}
\newtcolorbox{motivationblock}[1][]{seminarblock,title={动机与说明 / Motivation and commentary},colback=orange!2,colframe=orange!55!black,#1}
\newtcolorbox{strategyblock}[1][]{seminarblock,title={证明策略 / Proof strategy},colback=violet!2,colframe=violet!45!black,#1}
\newtcolorbox{hypothesisblock}[1][]{seminarblock,title={假设用途 / Use of hypotheses},colback=violet!2,colframe=violet!45!black,#1}
\newtcolorbox{hiddenstepsblock}[1][]{seminarblock,title={隐藏步骤 / Hidden steps},colback=violet!2,colframe=violet!45!black,#1}
\newtcolorbox{detailblock}[1][]{seminarblock,title={证明或检验 / Detailed proof or verification},colback=green!2,colframe=green!40!black,#1}
\newtcolorbox{lateruseblock}[1][]{seminarblock,title={后续用途 / Later use},colback=violet!2,colframe=violet!45!black,#1}
```

Default usage pattern:

- formal environment first,
- then `motivationblock` / `strategyblock` / `hypothesisblock` / `hiddenstepsblock` / `detailblock` / `lateruseblock` only when those layers materially help.

## Core gates

### Gate A: write gate

Create the checklist in tracking notes, not in manuscript正文. It must include:

- `items_to_cover`
- `idea_inventory`
- `idea_mapping`
- `proof_obligations`
- `technical_obligations`
- `depth_plan`
- `example_checks`
- `example_step_inventory`
- `exercise_solvability`
- `exercise_step_inventory`
- `local_knowledge_retrieval`
- `network_knowledge_retrieval`
- `term_decisions`
- `defer_list`

Sidecar mapping is mandatory:

- `items_to_cover` -> `source_scope`
- `idea_inventory` -> candidate vertices
- `idea_mapping` -> trace targets
- `proof_obligations` -> proof gaps / proof simplices
- `technical_obligations` -> technical simplices
- `example_checks` -> example-verification structures
- `exercise_solvability` and `exercise_step_inventory` -> exercise status + solution structures
- `term_decisions` -> terminology entries
- `defer_list` -> gaps/frontiers

Do not draft before Gate A is complete.

### Gate B: no-loss gate

- Map every nontrivial source idea atom to `covered`, `expanded`, `gap`, or `deferred`.
- Expand every technical item with proof strategy, hidden steps, hypothesis usage, omitted verifications/calculations, and later use when relevant.
- Reconstruct the full formal statement or prompt before any proof, verification, or solution.
- Verify every included example step by step from `example_step_inventory`.
- Solve every `solvable` exercise/problem step by step from `exercise_step_inventory`; for `partially solvable` or `currently blocked` items, write the exact obstruction and only the reachable part.
- Update the structure layer in lockstep:
  - definitions/notation -> vertices and terminology,
  - theorem-family statements -> theorem/proof simplices,
  - hidden steps -> support vertices / proof edges,
  - example checks -> example-verification structures,
  - exercise solutions -> exercise-solution structures,
  - misconceptions / false identifications -> anti-simplices or do-not-glue,
  - later use -> used-later / frontier relations.
- Replace lazy placeholders (`similar`, `analogous`, `easy to check`, `left to the reader`, `hint`, `sketch`, `同理`, `类似`, `易见`, `留作练习`, `略`, etc.) with full local steps unless the phrase is purely rhetorical after the step is already written out.
- Keep terminology, source scope, and future-hint boundaries consistent across manuscript and sidecar.

### Gate C: accept gate

Require all:

- numbering and local order match the source slice;
- no proof/verification/solution block lacks its full preceding formal statement/prompt;
- examples are fully verified step by step;
- each included problem/exercise has a solvability status, and every solvable one is fully solved step by step;
- no locally solvable example/exercise is left at hint/sketch level;
- no technical item is shallow enough that the reader still needs the source to reconstruct the argument;
- local retrieval and authoritative external cross-verification are complete, or a concrete exception is recorded;
- structure layer acceptance passes:
  - every nontrivial source idea is mapped,
  - every technical item has statement/proof or verification status,
  - every exercise has solvability status,
  - terminology first appearance is tracked,
  - traceability is complete for technical items,
  - JSON and Markdown are consistent,
  - no engineering/schema/update-log/Gate metadata leaks into manuscript正文 or PDF;
- display-math restraint holds: short routine formulas stay inline unless a display materially improves clarity;
- static checks are clean: environment pairing, duplicate labels, missing assets, statement-before-proof structure;
- PDF-clean check passes: no audit/change/self-check material is rendered;
- title-clean check passes: the original book title does not appear in any visible manuscript/PDF title, heading, running header, or PDF metadata;
- presentation-boundary check passes: no source-facing label, source-derived environment title, or workflow-facing label is visible in the manuscript/PDF; `sourceblock` is absent from generated manuscript files;
- compile check passes with the required XeLaTeX run count;
- attribution is present.

## Lean loop

1. Resolve mode, frontier, manuscript files, and mandatory sidecar files; if the sidecar layer is missing, run scaffold first.
2. Audit one contiguous source micro-slice.
3. Complete Gate A.
4. Run local retrieval and authoritative external cross-checking.
5. Run static checks before compile-sensitive edits.
6. Draft the manuscript slice and update the sidecar layer in lockstep.
7. Run a targeted example/exercise depth pass against `example_step_inventory` and `exercise_step_inventory`.
8. Run content, terminology, structure-health, and source-scope review; revise immediately.
9. Run typography review: compile, inspect affected pages, and collapse unnecessary short displays back into prose.
10. Apply Gate C; if it fails, stay on the same slice.
11. Update tracking, gap/frontier state, sidecar JSON, and Obsidian Markdown.
12. Compile XeLaTeX, verify the intended PDF output, and only then move to the next slice.

## Done criteria

A segment is complete only when:

- all targeted numbered items are present;
- all included formal environments have full statements/prompts;
- examples are verified step by step;
- exercises/problems are either fully solved step by step or explicitly blocked with the exact obstruction;
- technical items are fully expanded rather than lightly glossed;
- terminology, gap/frontier state, and source scope are consistent;
- local retrieval and authoritative external cross-verification are recorded, or a concrete exception is recorded;
- the knowledge-simplex sidecar exists in both required layers and passes structural acceptance;
- no workflow/audit/change-log content leaks into manuscript正文 or PDF;
- the original book title is absent from visible manuscript/PDF titles, headings, running headers, and PDF metadata;
- no visible source-facing labels (including `原文陈述` / `Source statement` or `原书定义` / `原书命题` / `原书练习`) or workflow-facing labels (including `本轮`, `下一轮`, or `后续习题修订`) remain;
- no unresolved lazy placeholder phrasing remains in example/problem/exercise blocks;
- display math is not being overused for short routine formulas;
- affected pages are visually checked and the PDF compiles cleanly.

## Report and editing rules

- Keep reports short and human-readable.
- In normal conversation, do not include file paths, file lists, or line references unless the user explicitly asks for them.
- Use the compact report shape:
  - Result
  - Gates
  - Review
  - Verification
  - Next
- Mention build artifacts only in `Verification`.
- Prefer `apply_patch` for manual edits.
- Do not revert unrelated changes.
- Keep mathematical language precise and readable; distinguish abstract categorical notions from concrete set-theoretic models.

## Resources

- [references/project-template.md](references/project-template.md)
- [references/seminar-latex-template.tex](references/seminar-latex-template.tex)
- [C:\Users\15816\.codex\skills\knowledge-simplex-organizer\references\math-book-autoloop-integration.md](C:\Users\15816\.codex\skills\knowledge-simplex-organizer\references\math-book-autoloop-integration.md)
- [C:\Users\15816\.codex\skills\knowledge-simplex-organizer\references\schema-v1.md](C:\Users\15816\.codex\skills\knowledge-simplex-organizer\references\schema-v1.md)
