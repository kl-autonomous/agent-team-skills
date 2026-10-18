# Build plan

The order the project gets built in. The builder drafts it, and the owner
approves it before step 1 starts. One step at a time; each step is
reviewed before the next begins.

## How each step goes

1. The builder says the step's goal and how we'll know it's done.
2. It builds the step, with tests, and runs the tests and the build
   itself.
3. For anything on screen, it takes screenshots in every mode the project
   supports.
4. It commits, with plain messages about what the owner will notice.
5. The reviewer checks the step independently and gives a verdict (see
   `docs/workflow.md`).

## Steps

| # | Step | Done when |
|---|---|---|
| 1 | {{The riskiest foundation first: the data and its rules, with no screens}} | {{Tests prove every rule in the design}} |
| 2 | {{Saving, and safety for the data}} | {{Tests prove each safety rule; a large made-up data set loads quickly}} |
| 3 | {{…}} | {{…}} |

Put the parts that could lose data first and the screens last. A bug in
the data is expensive; a bug on screen is cheap.

## Rules for every session

- Never weaken a safety check to make something pass.
- Never use real records in tests; test files use made-up data only.
- Ask before adding outside code, and before anything the design doesn't
  cover. Record each new decision in the decisions file.
