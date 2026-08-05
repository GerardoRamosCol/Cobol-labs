---

on:
  issues:
    types: [opened, edited]

permissions: read-all

safe-outputs:
  add-comment:

---

# Issue validator

This workflow will analyze the details of the issue and determine if it is a valid issue or not (valid if the issue as anything realated to COBOL, if not is not valid). If it is a valid issue, it will be labeled as such. If it is not a valid issue, it will be closed with a comment explaining why.