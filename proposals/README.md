# Proposals

A proposal publishes a change to the contract before any revision carries it.
The contract's subsection on proposals says what one means for a workspace.

## A proposal

Each proposal is one document, named after the proposal, and its fixtures sit in
a directory of the same name under `suite/proposals/`. Its `amends` field
records the revision it amends.

The document gives every change it makes to the contract in exact wording,
together with its reasons. A change can be a clause, a definition, or a choice
added to those section 6 leaves open.

A proposal can change the verdict of one of the amended revision's fixtures. It
then carries its own version of that fixture, under the same name. The document
lists each fixture it replaces.

## States

A proposal's directory gives its state:

- `proposals/` holds drafts, whose wording may still change.
- `proposals/accepted/` holds proposals a revision took in. Each records that
  revision in an `accepted_into` field.
- `proposals/withdrawn/` holds proposals that no revision takes in. One that
  another proposal replaced names it in a `replaced_by` field.

A draft's wording may change, and git history keeps every earlier version. A
workspace that opts into a draft takes its current wording, with no promise that
it stays the same. A name is never reused, whatever its state. The suite's own
description says what happens to a proposal's fixtures at each change of state.

## Overlap

Two drafts never change the same clause or replace the same fixture. When a new
proposal would, it and the existing draft merge into one under a new name. Both
originals are then withdrawn, naming the merged proposal. A draft adds clauses
only under numbers no other draft uses.

Once a revision takes a proposal in, a later proposal on the same clause amends
that revision. A workspace may name several proposals, and each one's fixtures
then pass with every named proposal in force.
