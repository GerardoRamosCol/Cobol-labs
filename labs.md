# GitHub Copilot for COBOL: Agentic Workshop Labs

This workshop uses the COBOL examples in this repository to practice GitHub
Copilot capabilities without installing a COBOL compiler, database, native
library, container runtime, or other development tool. All activities happen
through source inspection, Copilot Chat, Agent mode, reviewable file changes,
and documented reasoning.

The goal is to learn how to use Copilot effectively with COBOL, especially when
the local environment cannot execute the application.

## Audience

- Developers learning GitHub Copilot with COBOL repositories.
- COBOL maintainers exploring agentic analysis and documentation workflows.
- Teams evaluating Copilot for legacy-code understanding and change planning.

## Prerequisites

- VS Code.
- GitHub Copilot and GitHub Copilot Chat.
- Access to GitHub Copilot Agent mode.
- This repository opened as the VS Code workspace.

No COBOL compiler or other local software is required. Commands already present
in source comments and READMEs are analyzed as repository evidence, not executed.

## Lab Overview

| Lab | Title | Description |
| ---: | --- | --- |
| 1 | [Repository Reconnaissance](#lab-1-repository-reconnaissance) | Classify the repository examples and distinguish source-backed facts from assumptions. |
| 2 | [Repository Instructions for Copilot](#lab-2-repository-instructions-for-copilot) | Create persistent guidance that helps Copilot follow the repository's COBOL conventions. |
| 3 | [COBOL Explanation and Knowledge Transfer](#lab-3-cobol-explanation-and-knowledge-transfer) | Use Copilot to explain COBOL constructs, data flow, and program behavior for different audiences. |
| 4 | [Documentation from Source Evidence](#lab-4-documentation-from-source-evidence) | Generate example documentation while clearly separating source evidence from runtime unknowns. |
| 5 | [Static Test-Case Design](#lab-5-static-test-case-design) | Derive decision tables, equivalence classes, boundaries, and expected source paths without execution. |
| 6 | [Test-Driven Change Planning for SEARCH](#lab-6-test-driven-change-planning-for-search) | Define acceptance criteria and review a proposed validation change without applying it. |
| 7 | [Multi-File Contract Analysis](#lab-7-multi-file-contract-analysis) | Analyze caller, subprogram, linkage, passing modes, and storage lifetime across files. |
| 8 | [Dependency-Aware Code Review](#lab-8-dependency-aware-code-review) | Review SQL and native-library examples while separating confirmed findings from runtime unknowns. |
| 9 | [Agentic Capstone](#lab-9-agentic-capstone---design-a-new-cobol-example) | Supervise the design, generation, documentation, and static review of a new COBOL example. |

## Workshop Method

Use this pattern in every lab:

1. Give Copilot a narrow goal and identify the relevant files.
2. Ask it to cite source evidence for every important claim.
3. Separate facts, inferences, assumptions, and unknowns.
4. Review its plan before allowing edits.
5. Inspect the generated diff and reject unrelated changes.
6. Validate results by tracing source paths and comparing related files.
7. Record what would still require compilation or runtime verification.

## Lab 1: Repository Reconnaissance

### Use case

Use Copilot to understand an unfamiliar COBOL repository without executing it.

### Steps

1. Open Copilot Chat and select Agent mode.
2. Enter this prompt:

   > Analyze this repository without changing files or running COBOL code.
   > Identify its purpose, COBOL implementation, source conventions, example
   > categories, and external dependencies. Cite a file for every conclusion.

3. Ask Copilot to classify each top-level example as one or more of:
   deterministic, interactive, file-based, multi-program, screen-dependent,
   generated-code, or external-service-dependent.
4. Compare its classification with [README.md](README.md) and source headers.
5. Ask it to distinguish repository facts from assumptions.
6. Challenge one claim:

   > Show the exact source or documentation evidence for that claim. If there
   > is none, relabel it as an inference or unknown.

7. Ask Copilot to create a capability matrix containing folder, source files,
   COBOL concepts, inputs, outputs, dependencies, and confidence level.
8. Review the matrix for all repository folders and correct omissions.

### Deliverables

- Repository capability matrix.
- List of facts, inferences, and unresolved questions.

### Completion check

Every example folder is classified, every factual claim cites repository
evidence, and unsupported claims are explicitly marked as assumptions.

## Lab 2: Repository Instructions for Copilot

### Use case

Teach Copilot persistent COBOL and repository conventions so future responses
are more consistent.

### Steps

1. Ask Copilot to inspect representative files:
   - [trim/trim.cbl](trim/trim.cbl)
   - [search/search.cbl](search/search.cbl)
   - [sub_program/main_app.cbl](sub_program/main_app.cbl)
   - [sub_program/sub.cbl](sub_program/sub.cbl)
   - [sql/README.md](sql/README.md)
2. Prompt Copilot:

   > Propose repository instructions for working with these COBOL examples.
   > Preserve source formatting, educational intent, existing naming, and
   > Tectonics comments. Require evidence citations and clearly label behavior
   > that cannot be confirmed without execution. Do not edit yet.

3. Review the proposal and remove conventions not demonstrated in the code.
4. Ask Agent mode to create `.github/copilot-instructions.md`.
5. Review the diff for concise repository-wide guidance rather than
   task-specific instructions.
6. Start a new chat and ask Copilot to explain `sub_program/` and `sql/`.
7. Compare the new answer with the answer from Lab 1.
8. Refine the instruction file only where the comparison reveals a concrete
   gap.

### Deliverables

- `.github/copilot-instructions.md`.
- Before-and-after response comparison.

### Completion check

A new chat preserves COBOL formatting conventions, cites repository evidence,
and avoids claiming that unexecuted behavior was verified.

## Lab 3: COBOL Explanation and Knowledge Transfer

### Use case

Use Copilot to explain COBOL constructs at different levels of technical depth.

### Steps

1. Select [redifines/redefines.cbl](redifines/redefines.cbl) and ask:

   > Explain this program paragraph by paragraph. For each data item, describe
   > its PIC clause, storage relationship, and role in REDEFINES. Do not infer
   > runtime output beyond what the statements establish.

2. Ask Copilot to rewrite the explanation for a developer who knows Java or C#
   but not COBOL.
3. Select [unstring/unstring.cbl](unstring/unstring.cbl) and request a data-flow
   table showing source fields, delimiters, receiving fields, and count or
   pointer values.
4. Select [numval_test/numval_test.cbl](numval_test/numval_test.cbl) and ask for
   a comparison of alphanumeric and numeric representations.
5. Ask Copilot to cite exact declarations and statements for each explanation.
6. Request a Mermaid flowchart for one selected program.
7. Review the explanation for invented language semantics or output.
8. Ask Copilot to correct only the unsupported sections.

### Deliverables

- Paragraph-level explanation of one program.
- Cross-language explanation for a non-COBOL developer.
- Data-flow table and Mermaid flowchart.

### Completion check

Another learner can trace every explanation back to a declaration or statement
in the selected source.

## Lab 4: Documentation from Source Evidence

### Use case

Generate useful documentation without pretending that the programs were run.

### Steps

1. Ask Copilot to identify example folders that lack a local README.
2. Select `report_writer/` and `merge_sort/`.
3. Ask Copilot to inspect:
   - [report_writer/report_test.cbl](report_writer/report_test.cbl)
   - [report_writer/input.txt](report_writer/input.txt)
   - [merge_sort/merge_sort_test.cbl](merge_sort/merge_sort_test.cbl)
4. Use this prompt:

   > Draft documentation from source evidence only. Include purpose, files,
   > inputs, outputs, important COBOL constructs, and build commands already
   > documented in the source. Label predicted behavior as inferred and state
   > that commands were not executed during this workshop.

5. Review Copilot's proposed structure before allowing edits.
6. Allow it to create `report_writer/README.md`.
7. Trace each documented input and output to the `FILE-CONTROL`, `FD`, report,
   or procedure statements that establish it.
8. Ask Copilot to correct unsupported statements.
9. Repeat for `merge_sort/README.md`.
10. Compare both new READMEs with existing example documentation.

### Deliverables

- `report_writer/README.md`.
- `merge_sort/README.md`.
- A short "not runtime verified" statement in each generated document.

### Completion check

Every technical statement is supported by source or existing documentation,
and no output is presented as observed runtime behavior.

## Lab 5: Static Test-Case Design

### Use case

Use Copilot to design COBOL test cases and expected paths without creating or
executing a test harness.

### Steps

1. Ask Copilot to inspect [is_numeric/is_numeric.cbl](is_numeric/is_numeric.cbl).
2. Request a decision table covering plain input, zero-filled input, trimmed
   input, spaces, signs, decimal characters, alphabetic input, and empty input.
3. Require columns for input, target paragraph, relevant condition, predicted
   branch, expected message pattern, and confidence.
4. Ask Copilot to mark cases whose behavior depends on GnuCOBOL semantics and
   cannot be proven from the source alone.
5. Repeat for [trim/trim.cbl](trim/trim.cbl), focusing on leading, trailing, and
   complete trimming.
6. Ask Copilot to identify equivalence classes and boundary cases instead of
   producing redundant examples.
7. Have it create a Markdown test-design document, not executable test code.
8. Review every predicted result against the source control flow.

### Deliverables

- Static decision tables for `is_numeric` and `trim`.
- Equivalence classes, boundaries, and runtime-verification notes.

### Completion check

Every test case identifies the exact source branch it is intended to exercise,
and uncertain runtime semantics are visibly labeled.

## Lab 6: Test-Driven Change Planning for SEARCH

### Use case

Plan a safe COBOL enhancement with acceptance criteria and source-level test
scenarios, without implementing or running the change.

### Steps

1. Read [search/README.md](search/README.md) and ask Copilot to explain the
   difference between `SEARCH` and `SEARCH ALL` in this example.
2. Ask it to map every accepted input to its owning paragraph, table, key, and
   found/not-found branch in [search/search.cbl](search/search.cbl).
3. Define this enhancement: malformed numeric input should produce a clear
   validation message without altering valid searches.
4. Request acceptance criteria for valid, invalid, missing, first-key, and
   last-key cases.
5. Ask Copilot to write a source-level test matrix showing the intended path
   before and after the proposed change.
6. Prompt Copilot:

   > Identify the smallest paragraph that should own this validation. Propose
   > a COBOL diff, but do not apply it. Preserve the distinction between SEARCH
   > and SEARCH ALL and explain every changed line.

7. Review the proposed diff for unrelated table, ordering, or output changes.
8. Ask Copilot to perform a static self-review against the acceptance criteria.
9. Record compilation and runtime checks that a future implementation would
   require.

### Deliverables

- Acceptance criteria.
- Before/after source-path test matrix.
- Reviewed proposed diff that is not applied.
- Future validation checklist.

### Completion check

The proposal is limited to the owning input-validation path and preserves the
teaching purpose of both search forms.

## Lab 7: Multi-File Contract Analysis

### Use case

Use Copilot to reason across a COBOL caller, subprogram, linkage contract, and
state lifetime.

### Steps

1. Ask Copilot to analyze:
   - [sub_program/main_app.cbl](sub_program/main_app.cbl)
   - [sub_program/sub.cbl](sub_program/sub.cbl)
2. Request a contract table containing argument order, caller name, callee
   name, PIC definition, passing mode, mutation, and lifetime.
3. Ask for a sequence diagram covering the by-content call, by-reference call,
   `CANCEL`, and final call.
4. Require Copilot to distinguish `WORKING-STORAGE`, `LOCAL-STORAGE`, and
   `LINKAGE SECTION` behavior using exact source references.
5. Define a hypothetical third status parameter that reports fresh or retained
   subprogram state.
6. Ask Copilot to propose coordinated caller and callee changes without
   applying them.
7. Require a consistency checklist for argument count, order, PIC size,
   `PROCEDURE DIVISION USING`, every `CALL USING`, and passing mode.
8. Ask Copilot to identify likely failures if only one file were changed.
9. Review the proposal against [sub_program/README.md](sub_program/README.md).

### Deliverables

- Caller/callee contract table.
- Call sequence diagram.
- Coordinated change proposal and consistency checklist.

### Completion check

The proposed signatures match across both files, and every predicted state
transition is tied to a specific `CALL`, storage section, or `CANCEL` statement.

## Lab 8: Dependency-Aware Code Review

### Use case

Use Copilot to review dependency-heavy COBOL while separating confirmed source
findings from concerns that require external systems.

### Steps

1. Ask Copilot to review these files without editing:
   - [sql/sql_example.cbl](sql/sql_example.cbl)
   - [sql/generated_sql_ex.cbl](sql/generated_sql_ex.cbl)
   - [sql/create_test_db.sql](sql/create_test_db.sql)
   - [sql/README.md](sql/README.md)
2. Require findings first, ordered by severity.
3. Require each finding to include file evidence, impact, owning source,
   recommendation, confidence, and future validation method.
4. Separate confirmed source defects, documentation inconsistencies, security
   risks, generated-code concerns, and runtime unknowns.
5. Ask Copilot why generated precompiler output should not normally be the
   primary edit target.
6. Challenge generic findings that do not cite repository evidence.
7. Repeat a focused review for [json_generate/README.md](json_generate/README.md)
   and [xml_generate/README.md](xml_generate/README.md).
8. Ask Copilot to produce a dependency matrix for all optional examples.
9. Convert verified findings into issue drafts with acceptance criteria, but do
   not install dependencies or attempt execution.

### Deliverables

- Evidence-based review report.
- Optional-dependency matrix.
- Prioritized issue drafts.

### Completion check

Every finding is evidence-backed and clearly labels whether it is confirmed by
source inspection or requires future external validation.

## Lab 9: Agentic Capstone - Design a New COBOL Example

### Use case

Supervise Copilot through clarification, design, source generation,
documentation, and static review without executing generated COBOL.

### Scenario

Design a dependency-free COBOL example that accepts an eight-digit `YYYYMMDD`
value and displays `YYYY-MM-DD` or a clear invalid result.

### Steps

1. Give Copilot the outcome and require clarification before editing:

   > Design a COBOL example that accepts YYYYMMDD, validates its structure and
   > agreed date ranges, and formats valid input as YYYY-MM-DD. Do not run code
   > or assume a compiler is available. Ask clarifying questions, inspect
   > neighboring examples, and present a plan before editing.

2. Answer its questions about leap years, month-specific limits, retry
   behavior, spaces, nonnumeric input, and exact messages.
3. Record the agreed scope as acceptance criteria.
4. Ask Copilot to identify repository templates for source layout, input
   handling, naming, comments, and README structure.
5. Review its plan for source, documentation, decision table, traceability, and
   static review.
6. Allow Agent mode to create the new source and README.
7. Ask Copilot to explain every data declaration and paragraph in the generated
   source.
8. Create a decision table for valid dates, invalid length, nonnumeric input,
   invalid month, and invalid day.
9. Trace every decision-table case to a source condition and output path.
10. Ask Copilot for a code review focused on COBOL syntax risks, boundary
    logic, repository consistency, and missing cases.
11. Address only evidence-backed findings.
12. Review the final diff for accidental changes outside the new example.
13. Add a verification note listing compilation and runtime checks that remain
    for a future environment with GnuCOBOL.

### Deliverables

- New COBOL example source.
- Source-grounded README.
- Acceptance criteria and decision table.
- Static code review and future validation checklist.

### Completion check

The source, documentation, and decision table agree with one another; every
case traces to a source path; and unverified runtime behavior is clearly marked.

### Scoring rubric

| Area | Weight |
| --- | ---: |
| Source reasoning and traceability | 30% |
| Prompt quality and acceptance criteria | 25% |
| Repository consistency | 20% |
| Scope discipline | 15% |
| Documentation | 10% |

## Optional Copilot-Only Extensions

1. Ask Copilot to generate a glossary of COBOL terms used in this repository.
2. Create issue templates for future compilation and runtime verification.
3. Compare two alternative refactoring proposals without applying either one.
4. Generate onboarding questions and answers from the example inventory.
5. Produce a pull-request review checklist for COBOL caller/callee changes.

These extensions remain source-only and require no local software installation.
