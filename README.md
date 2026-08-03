# GitHub Copilot for COBOL Workshop

This repository is a hands-on training workshop for using GitHub Copilot with
COBOL. It combines small GnuCOBOL examples with agentic exercises covering
repository discovery, explanation, documentation, static test design, change
planning, coordinated multi-file reasoning, and code review.

The examples were originally written with
[GnuCOBOL](https://gnucobol.sourceforge.io/) on Linux. The workshop does not
require GnuCOBOL or any other local software because the exercises use source
inspection and GitHub Copilot rather than executing COBOL programs.

## Learning Objectives

By completing the workshop, you will practice how to:

- Ground GitHub Copilot in repository evidence before allowing changes.
- Write effective prompts with scope, constraints, and acceptance criteria.
- Review an agent's plan and reject unsupported assumptions.
- Generate source-grounded COBOL explanations and documentation.
- Design decision tables and test scenarios from control flow.
- Plan behavior changes without applying unverified code.
- Analyze caller and subprogram contracts across multiple files.
- Review dependency-heavy code without unsupported runtime claims.
- Supervise source generation and static review in an agentic workflow.

## Prerequisites

- VS Code.
- GitHub Copilot and GitHub Copilot Chat.
- Access to GitHub Copilot Agent mode.
- This repository opened as the VS Code workspace.

No COBOL compiler, database, native library, container runtime, or command-line
tool needs to be installed. Existing build commands and dependencies are
analyzed as source evidence only.

## Workshop Workflow

Use the following supervision loop in every lab:

1. Establish the current behavior or repository fact.
2. Ask Copilot to inspect the relevant files and propose a bounded plan.
3. Review the plan and correct unsupported assumptions.
4. Permit edits only when the lab explicitly creates an artifact.
5. Validate claims by tracing declarations, statements, and related files.
6. Separate confirmed facts from inferences and runtime unknowns.
7. Review the final diff and reject unrelated changes.
8. Record what future compilation or execution would still need to verify.

The complete exercises, prompts, deliverables, and completion checks are in
[labs.md](labs.md).

## Workshop Labs

| Lab | Agentic capability | Outcome |
| ---: | --- | --- |
| 1 | Repository reconnaissance | Classify the examples and ground claims in source evidence. |
| 2 | Repository instructions | Teach Copilot persistent COBOL and repository conventions. |
| 3 | COBOL explanation | Explain programs for COBOL and non-COBOL developers. |
| 4 | Documentation from source | Create documentation without claiming execution evidence. |
| 5 | Static test-case design | Derive decision tables, boundaries, and source paths. |
| 6 | SEARCH change planning | Design acceptance criteria and a proposed diff without applying it. |
| 7 | Multi-file contract analysis | Map caller, callee, linkage, passing modes, and storage lifetime. |
| 8 | Dependency-aware review | Separate confirmed findings from runtime unknowns. |
| 9 | Agentic capstone | Generate and statically review a complete COBOL example. |

Start with [Lab 1](labs.md#lab-1-repository-reconnaissance-and-agent-instructions)
or review the full [workshop guide](labs.md).

## Example Inventory

| Folder | Source and data files | COBOL concepts | Notes and dependencies |
| --- | --- | --- | --- |
| `accept/` | [accept.cbl](accept/accept.cbl), [accept_from.cbl](accept/accept_from.cbl), [accept-secure.cbl](accept/accept-secure.cbl) | `ACCEPT`, `ACCEPT FROM`, and secure input | Interactive; secure and screen behavior can depend on terminal support. See [accept/README.md](accept/README.md). |
| `comp_test/` | [comp_test.cbl](comp_test/comp_test.cbl) | Converting computational (`COMP`) values for display | Core GnuCOBOL example. |
| `display_test/` | [display-test.cbl](display_test/display-test.cbl) | `DISPLAY` statement options | Interactive terminal output. |
| `display_timing/` | [display_timing.cbl](display_timing/display_timing.cbl) | Comparing screen-writing approaches and timing | Terminal behavior and timing can vary by platform. |
| `is_numeric/` | [is_numeric.cbl](is_numeric/is_numeric.cbl) | `IS NUMERIC`, justification, `INSPECT`, and `TRIM` | Interactive; used by the static test-case design lab. |
| `json_generate/` | [json_generate.cbl](json_generate/json_generate.cbl) | `JSON GENERATE` | Requires GnuCOBOL JSON support and `json-c`. See [json_generate/README.md](json_generate/README.md). |
| `merge_sort/` | [merge_sort_test.cbl](merge_sort/merge_sort_test.cbl) | COBOL `SORT` and `MERGE` with file data | File-based; input fixtures may be required. |
| `mouse/` | [mouse_example.cbl](mouse/mouse_example.cbl) | Mouse events and a simple terminal paint program | Requires compatible curses and terminal mouse support. See [mouse/README.md](mouse/README.md). |
| `numval_test/` | [numval_test.cbl](numval_test/numval_test.cbl) | Converting alphanumeric data with `NUMVAL` | Core GnuCOBOL example. |
| `read_command_args/` | [read_cmd_line_args.cbl](read_command_args/read_cmd_line_args.cbl), [read_specific_cmd_line_args.cbl](read_command_args/read_specific_cmd_line_args.cbl) | Reading the full command line and individual arguments | Run with command-line arguments. |
| `redifines/` | [redefines.cbl](redifines/redefines.cbl) | Overlaying data with `REDEFINES` | Folder name is retained from the original example. See [redifines/README.md](redifines/README.md). |
| `report_writer/` | [report_test.cbl](report_writer/report_test.cbl), [input.txt](report_writer/input.txt) | Report Writer and file-driven reports | File-based example with checked-in input data. |
| `screen_size/` | [get_screen_size.cbl](screen_size/get_screen_size.cbl) | Reading terminal row and column counts | Requires compatible screen handling. See [screen_size/README.md](screen_size/README.md). |
| `search/` | [search.cbl](search/search.cbl) | Sequential `SEARCH` and binary `SEARCH ALL` | Interactive; used by the test-driven hardening lab. See [search/README.md](search/README.md). |
| `sql/` | [sql_example.cbl](sql/sql_example.cbl), [generated_sql_ex.cbl](sql/generated_sql_ex.cbl), [create_test_db.sql](sql/create_test_db.sql) | Embedded SQL, PostgreSQL access, and variable-length query values | Requires PostgreSQL, esqlOC, unixODBC, and the PostgreSQL ODBC driver. See [sql/README.md](sql/README.md). |
| `sub_program/` | [main_app.cbl](sub_program/main_app.cbl), [sub.cbl](sub_program/sub.cbl) | `CALL` by content/reference, `LINKAGE SECTION`, local/working storage, and `CANCEL` | Multi-source example used by the contract-analysis lab. See [sub_program/README.md](sub_program/README.md). |
| `trim/` | [trim.cbl](trim/trim.cbl) | Leading, trailing, and complete intrinsic `TRIM` | Deterministic core example and first golden-output test. See [trim/README.md](trim/README.md). |
| `unstring/` | [unstring.cbl](unstring/unstring.cbl) | Splitting fields with `UNSTRING` | Core GnuCOBOL example. See [unstring/README.md](unstring/README.md). |
| `xml_generate/` | [xml_generate.cbl](xml_generate/xml_generate.cbl) | `XML GENERATE` | Requires GnuCOBOL XML support and `libxml2`. See [xml_generate/README.md](xml_generate/README.md). |

## Suggested Learning Path

1. Use Agent mode to inventory the repository and distinguish facts from
	assumptions.
2. Add repository instructions that preserve COBOL conventions.
3. Explain and document selected programs from source evidence.
4. Design static test cases and a proposed improvement to `search`.
5. Analyze the contract across the two `sub_program` sources.
6. Review SQL and native-library examples while respecting evidence limits.
7. Complete the source-generation and static-review capstone.

## Scope

The workshop preserves these programs as focused COBOL teaching examples.
Rewriting them in another language, installing toolchains, provisioning
services, and executing the examples are outside scope. Any behavior that
depends on compilation or runtime execution must be labeled as unverified.

## License

See [LICENSE](LICENSE).



