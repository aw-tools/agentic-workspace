# The contract

The contract is the written definition of the record. It says what a file in the
record must declare and how a file's kind decides its lifetime. It also says
what happens to a file at the end of its life. The contract lives in the spec
repository as `SPEC.md`.

## Why there is one

The record is only useful if a second tool can read it. Without a written
definition, the layout is whatever `aw` happens to do, and a workspace is tied
to one binary for life.

So the contract names no tool. It defines the model. Any implementation that
passes the conformance suite for a revision conforms to that revision. `aw` and
the template's lint are one implementation among possible others.

## What it covers

The contract is short and numbered. Each normative statement is a clause with a
number and one of the key words MUST, SHOULD or MAY. Clause numbers are stable
within a revision, so the guide, the agent instructions and the suite cite them.
Material without a number is explanation and binds nobody.

The clauses cover:

- The two fields a file declares, `kind` and `status`, and the class derived
  from the first through a registry (section 2).
- The four classes, their state machines, and how a file may move through its
  states (section 3).
- Disposition: what happens to a file at the end of its life (section 4). A
  graduated file leaves the working tree, and the same commit records its
  residue.
- The frontmatter block and where it sits (section 5).

Section 6 lists what the contract leaves open on purpose:

- where files sit;
- what the kinds are called;
- how big a file may be;
- whether staleness is tracked;
- which version control system holds the workspace.

Each is a real choice, and the contract does not make it.

## Revisions

The contract carries an integer revision. The number moves when a clause
changes. An editorial change or a tool release does not move it. A workspace
states the revision it conforms to.

An implementation must refuse a revision it does not recognise. The publication
date records when the text was issued; the revision alone decides conformance.

## How conformance is proved

The conformance suite is a set of fixtures in the spec repository, under
`suite/`, one directory per revision. A fixture is a small workspace, either a
working tree or an ordered sequence of commits. It carries the verdict a
conforming tool must give: accept or reject. Every clause maps to at least one
fixture.

A tool conforms when it gives the expected verdict on every fixture of the
revision. The suite fixes one form for the things the contract leaves open, such
as the registry format, so that any implementation can read the fixtures. That
choice places no requirement on a workspace.

The suite also covers what a tool must not assume. Several fixtures use a
vocabulary unlike the template's, and one maps a familiar word to a different
class. A tool that gives a kind a meaning beyond its registry entry fails them.

## Proposals

A proposal is a published change to the contract that no revision yet carries.
It binds only a workspace that opts in. The revision number stays where it is.
The spec repository holds each proposal under `proposals/`.

A workspace opts in by adding the proposal's name to the `proposals` list in its
manifest's `[template]` block, beside the revision it states there. Both
`aw init` and `aw lint` refuse a name the installed `aw` does not know.

`aw` knows one proposal, `supersession-marks`, which lets both entries of a
supersession carry the marks [chapter 4](04-the-record.md) describes. It is a
draft, a proposal whose wording may still change. A workspace that opts into a
draft takes whatever wording the draft has when you upgrade `aw`. The template
declares it, so a workspace made by `aw init` is opted in.

Without the opt-in, the lint reads a mark added anywhere but the end of the
decision register as a rewrite, and fails it. A workspace made from an earlier
release of the template opts in by taking the template change that declares the
proposal. [Chapter 6](06-working-with-agents.md) says where the procedure for
taking template changes lives.

## Where the implementation stands

`aw lint` is the implementation this guide describes. At commit time it enforces
the contract's clauses and those of each proposal the workspace opts into. It
also enforces rules of its own that the contract leaves open. One is a layout
rule: shared files sit at the top of `context/`, and each engagement's files sit
in its own directory. The others are the state file's caps, the staleness
warning and remits. These rules are this implementation's tuning, and a
workspace that sets them differently still conforms.

The caps, the staleness warning and remits run only when the registry has a
`[state]` table. The template's registry has one. Remove the table to turn the
three off.

[Chapter 6](06-working-with-agents.md) explains how a session works within these
rules.
