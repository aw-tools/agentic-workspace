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
changes, and not for an editorial change or a release of any tool. A workspace
states the revision it conforms to.

An implementation must refuse a revision it does not recognise, rather than
apply the rules of a different one. The publication date records when the text
was issued; the revision alone decides conformance.

## How conformance is proved

The conformance suite is a set of fixtures in the spec repository, under
`suite/`, one directory per revision. A fixture is a small workspace, either a
working tree or an ordered sequence of commits. It carries the verdict a
conforming tool must give: accept or reject. Every clause maps to at least one
fixture.

A tool conforms when it gives the expected verdict on every fixture of the
revision. The suite fixes one form for the things the contract leaves open, such
as the registry format, so that any implementation can read the fixtures. That
choice governs the fixtures alone and places no requirement on a workspace.

The suite also covers what a tool must not assume. Several fixtures use a
vocabulary unlike the template's, and one maps a familiar word to a different
class. A tool that gives a kind a meaning beyond its registry entry fails them.

## Where the implementation stands

The template's `bin/lint-artefacts` is the implementation this guide describes.
It enforces the contract's clauses at commit time. It also enforces rules of its
own that the contract leaves open: the topic-first layout, the state file's
caps, the staleness warning and remits. Those are this implementation's tuning,
and a workspace that sets them differently still conforms.

The lint is a shell script. Reimplementing it as an `aw` verb is planned and not
built.

[Chapter 6](06-working-with-agents.md) explains how a session works within these
rules.
