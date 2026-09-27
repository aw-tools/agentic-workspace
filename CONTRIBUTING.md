# Contributing

This specification is in early development and is **not accepting external
contributions** at this time.

## The guide

The chapters under `guide/` and its glossary follow these rules.

- The guide describes only released behaviour of the stack: `aw`, the template,
  the skills and lore. A guide change for unreleased behaviour waits on its
  branch until that behaviour ships.
- Each thing has one name. A chapter introduces a term with its definition, the
  glossary lists it against that chapter, and every later use keeps the exact
  name.
- `bin/lint-guide` runs Vale over the chapters and checks each chapter against
  its word budget in `guide/budget.txt`. Run it before pushing. Raise a budget
  line in a commit that says why. When a Vale rule changes,
  `bin/test-lint-guide` proves it still fires on its fixture.
