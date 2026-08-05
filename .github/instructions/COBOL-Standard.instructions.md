---
description:  COBOL coding standard for the project.
applyTo: '**'
---

# COBOL Coding Standard

Validate code against the following rules to ensure it meets the COBOL coding standard. These rules are intended to be used as a guide for writing maintainable, readable, and reliable COBOL code.

## 1. General Principles

- Use structured programming; avoid `GO TO` except for documented error handling.
- Write code that is readable, testable, restartable, and maintainable.
- Use consistent naming, indentation, paragraph structure, and comments.
- Preserve business rules exactly during maintenance or modernization.
- Compile with strict compiler warnings enabled.

## 2. Source Format

- Use standard COBOL divisions and sections:

```cobol
       IDENTIFICATION DIVISION.
       PROGRAM-ID. CUSTOMER-UPDATE.

       ENVIRONMENT DIVISION.

       DATA DIVISION.

       PROCEDURE DIVISION.
```

- Use a consistent indentation scheme:
  - Division and section names: Area A.
  - Paragraph names: Area A.
  - Statements: Area B.
  - Continuation lines: aligned consistently.
- Limit source lines to the project’s compiler-supported length, preferably 72 columns for legacy compatibility.
- Keep one logical statement per line where practical.

## 3. Naming Conventions

| Element | Standard | Example |
|---|---|---|
| Programs | Verb-noun or noun-purpose | `CUSTOMER-UPDATE` |
| Paragraphs | Verb-noun | `VALIDATE-CUSTOMER` |
| Files | `FD-` or business-oriented prefix | `FD-CUSTOMER-FILE` |
| Data items | Descriptive, hyphenated | `CUSTOMER-ACCOUNT-NUMBER` |
| Constants | `C-` prefix | `C-MAX-RETRIES` |
| Switches | `SW-` prefix | `SW-END-OF-FILE` |
| Counters | `CNT-` prefix | `CNT-RECORDS-READ` |
| Flags | `WS-...-FLAG` or `SW-...` | `SW-VALID-RECORD` |
| Return codes | `RETURN-CODE` or `WS-RETURN-CODE` | `WS-RETURN-CODE` |

Avoid abbreviations unless they are defined in the project glossary.

## 4. Data Definition

- Define data at the smallest appropriate scope.
- Use level-88 condition names for meaningful values:

```cobol
       01  WS-STATUS              PIC X.
           88  STATUS-ACTIVE       VALUE 'A'.
           88  STATUS-INACTIVE     VALUE 'I'.
```

- Use `PIC 9` for numeric values and `PIC X` for text.
- Use explicit `COMP-3` or binary usage only when required by performance or interface standards.
- Always define numeric signs explicitly when negative values are valid.
- Avoid redefining data unless the representation is fully documented.
- Initialize working-storage fields explicitly when required by program flow.

## 5. Procedure Division

- Organize logic into small, single-purpose paragraphs.
- Use a consistent top-level flow:

```cobol
       MAIN-PROCESS.
           PERFORM INITIALIZE-PROGRAM
           PERFORM PROCESS-RECORDS
               UNTIL SW-END-OF-FILE
           PERFORM TERMINATE-PROGRAM
           GOBACK.
```

- Prefer `PERFORM`, `EVALUATE`, and structured conditional logic.
- Avoid deeply nested `IF` statements.
- Use `CONTINUE` only when it improves clarity.
- Every paragraph should have one clear entry point and one clear purpose.
- Avoid altering control flow with implicit paragraph fall-through.

## 6. File Handling

- Check file status after every `OPEN`, `READ`, `WRITE`, `REWRITE`, and `CLOSE`.
- Define file status fields consistently:

```cobol
       SELECT CUSTOMER-FILE
           ASSIGN TO CUSTOMERDD
           FILE STATUS IS WS-CUSTOMER-FILE-STATUS.
```

- Handle end-of-file explicitly.
- Do not ignore unexpected file statuses.
- Use a standard error paragraph for file errors.
- Ensure files are closed during normal and abnormal termination where possible.

## 7. Error Handling

- Validate all external inputs.
- Use standardized return codes.
- Log enough information to diagnose failures without exposing sensitive data.
- Distinguish business validation errors from system errors.
- Never silently ignore an error.
- Avoid broad error handling that masks the original failure.

## 8. Database and Transaction Processing

- Check SQL return codes after every SQL statement.
- Check `SQLCODE` and relevant diagnostic fields.
- Keep transaction boundaries explicit.
- Do not issue `COMMIT` or `ROLLBACK` from utility routines unless documented.
- Avoid commits inside loops unless restartability requires them.
- Use parameterized SQL and avoid constructing SQL from unvalidated input.

## 9. Comments and Documentation

- Explain **why**, not merely what the code does.
- Document:
  - Business rules
  - External interfaces
  - Restart and recovery behavior
  - Special numeric formats
  - Copybook dependencies
  - Known compiler or platform constraints
- Update comments when changing logic.
- Remove obsolete comments.

## 10. Security

- Do not place credentials, keys, or sensitive data in source code.
- Mask personally identifiable information in logs.
- Validate input lengths and formats.
- Avoid writing full records to diagnostic output.
- Protect files and datasets according to the platform’s access-control standards.

## 11. Testing and Change Control

- Every change must include regression tests for affected logic.
- Test normal, boundary, invalid, duplicate, missing, and restart scenarios.
- Verify packed-decimal, sign, truncation, and date-handling behavior.
- Perform impact analysis before changing copybooks or shared layouts.
- Require code review for changes to shared copybooks, database interfaces, JCL, and transaction logic.

## 12. Example Error Pattern

```cobol
       READ-CUSTOMER-FILE.
           READ CUSTOMER-FILE
               AT END
                   SET SW-END-OF-FILE TO TRUE
               NOT AT END
                   IF WS-CUSTOMER-FILE-STATUS NOT = '00'
                       MOVE C-READ-ERROR TO WS-RETURN-CODE
                       PERFORM HANDLE-FILE-ERROR
                   END-IF
           END-READ.
```
