# The Agentic Workspace artefact model

**Contract revision 1.** Published 2026-09-16.

This document specifies how to classify the documents an agentic workspace
accumulates and what eventually happens to each of them. It names no tool. Any
implementation passing the conformance suite for a given revision conforms to
that revision. The workspace template's `bin/lint-artefacts` script is the
reference implementation, not the definition.

## 1. Conventions

The key words MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY are to be interpreted
as described in RFC 2119.

Every normative statement is a numbered clause carrying one of those keywords.
Clause numbers are stable within a revision and are what guides, agent
instructions, and the conformance suite cite. Each maps to at least one fixture
in the suite. A fixture is a working tree or an ordered sequence of commits. A
clause whose subject is a commit takes the second.

Material carrying no keyword and no clause number is not normative, says so, and
binds nobody.

The working tree is the set of files the workspace holds as a reader sees it
now. Version-control history is separate: an artefact that leaves the working
tree stays recoverable from that history.

### The revision number

The revision number is a plain integer without leading zeros. It increments when
a normative clause changes, and not for an editorial change, an example, or a
release of any implementation.

A workspace states the revision it conforms to. Conforming to revision N means
passing revision N's fixtures.

The publication date above records when this text was issued. The revision alone
determines conformance: an editorial reissue moves the date and leaves the
revision where it is.

**1.1.1** An implementation MUST refuse a revision it does not recognise, rather
than applying the rule set of a different revision.

### Version control

Deletion counts as disposition only because history is the archive. The contract
requires version control.

**1.2.1** A workspace MUST be kept under version control that records changes as
commits and retains history.

## 2. The model

An artefact declares two fields, `kind` and `status`. A third property, its
class, is derived from the first.

**Kind** is what the artefact is: a handover, a spec, a ledger. An agent writing
one knows its kind without reasoning about it.

**Class** is its lifecycle, determining which state machine applies, what
closure means, and whether the artefact may leave the working tree. The class
follows from the kind by a lookup in a registry and is never written into the
artefact.

**Status** is its current position in that machine.

**2.1** An artefact MUST declare `kind` and `status`.

**2.2** An artefact MUST NOT declare its class. The class is derived from the
kind.

**2.3** A registry MUST map every kind the workspace uses to exactly one class.

**2.4** An artefact whose `kind` has no entry in the registry MUST be rejected.

**2.5** An implementation MUST NOT give any kind a meaning beyond its registry
entry. The registry alone determines an artefact's class, and no name carries a
class the registry has not given it.

**2.6** A workspace MUST declare which files are artefacts, in a form an
implementation can read without parsing prose. Clauses 2.1 to 2.4 and section 5
apply to those files and to no others.

> **Why the class is derived.** A field that could be computed but is typed by
> hand into every file eventually disagrees with the field it came from, and
> nothing says which is right. Deriving class from kind makes a lifecycle change
> one registry row instead of an edit to every file carrying the old answer.

> **Why no name is built in.** Kind names are a workspace's own domain language,
> and the model works whatever they are. Naming a fixed set here would reserve
> ordinary words permanently and turn a description of one workspace into a
> requirement on every other. Clause 2.5 makes that freedom testable.

A classification argument is about lifespan. A plan is episodic not for what it
discusses but because its authority ends when the work it governs ends.

## 3. The classes and their state machines

Each class has one state machine, and every kind in that class moves through
those states and no others.

| Class         | States                        | What the terminal state means                               |
| ------------- | ----------------------------- | ----------------------------------------------------------- |
| **standing**  | `live → retired`              | stays in the working tree, no longer maintained             |
| **binding**   | `active → superseded`         | stays in the working tree, linked to its replacement        |
| **episodic**  | `open → closed → compacted`   | compaction rewrites the closed artefact to its residue      |
| **ephemeral** | `open → graduated \| expired` | leaves the working tree in the commit recording its residue |

**3.1** An artefact's `status` MUST be one of the states of its class.

**3.2** An implementation MUST reject a `status` that belongs to another class
and not to the artefact's own.

**3.3** A status MUST NOT move backwards along its machine, and MUST NOT skip a
state.

**3.4** A standing artefact SHOULD be rewritten in place.

**3.5** A standing artefact SHOULD NOT be appended to.

**3.6** A binding artefact MUST be append-only: body content once recorded MUST
NOT be changed, and new body content MUST follow it. The frontmatter is not body
content.

**3.7** A binding artefact reaching `superseded` MUST name what supersedes it.

### The purpose of each class

Standing artefacts answer what is true now and stay current by being rewritten.
Binding artefacts answer what was decided and why, and stay honest by never
editing what is already written. Episodic artefacts govern a stretch of work and
lose their authority when it ends. Ephemeral artefacts serve one hand-off and
are worth nothing afterwards.

The ephemeral class carries the most weight. Without it a workspace keeps every
document it has produced, and a reader cannot tell which ones still describe the
present.

## 4. Disposition

Classification determines disposition. Disposition is what eventually happens to
an artefact: kept, replaced, or deleted. An artefact's class decides its
disposition, and that is settled once for the whole class.

### Ephemeral artefacts

**4.1.1** An ephemeral artefact in a terminal state MUST NOT remain in the
working tree. The clause binds the working tree as a reader sees it now: a
commit may record the terminal state and a later commit delete the artefact.

**4.1.2** An ephemeral artefact reaching `graduated` MUST have its residue
recorded in a durable artefact, such as a binding entry, a ledger, or a standing
document, in the same commit that deletes it. An implementation MUST reject the
deleting commit when no standing, binding, or episodic artefact changes in it.

**4.1.3** An ephemeral artefact reaching `expired` MAY be deleted with no
residue recorded. Marking it `expired` asserts it held nothing worth keeping.

**4.1.4** The deletion MUST be recoverable from version-control history. An
implementation MUST NOT satisfy clause 4.1.1 by moving the artefact into an
archive directory that remains in the working tree.

> **Why deletion is not a loss.** Whether a document is retained and whether it
> sits in the working tree are separate questions. Version control already holds
> every version of every file, so a second copy in the working tree adds nothing
> and costs every reader who must work out whether it is live. In git,
> `git log --diff-filter=D` recovers anything deleted.

### Episodic artefacts

**4.2.1** A workspace MUST declare each unit of work, whether it is open or
closed, and which episodic artefacts belong to it, in a form an implementation
can read without parsing prose.

**4.2.2** Closing a unit of work MUST close every episodic artefact belonging to
it, in the same commit.

**4.2.3** An implementation MUST reject a closed unit of work still holding an
episodic artefact in the `open` state.

**4.2.4** Compaction MUST preserve the closed artefact's durable residue and MAY
discard the rest. A compacted artefact MUST retain content beyond its
frontmatter.

## 5. Frontmatter

Frontmatter is a short block of machine-readable fields at the very start of a
file, marked off from the prose that follows. The example below is illustrative;
this contract does not require YAML.

```yaml
---
kind: handover
status: open
---
```

**5.1** An artefact MUST carry its `kind` and `status` in a frontmatter block at
the very start of the file.

**5.2** The frontmatter block MUST be delimited so an implementation can find it
without parsing the artefact's prose.

**5.3** An artefact MAY carry fields beyond `kind` and `status`, except `class`,
which clause 2.2 forbids. An implementation MUST NOT reject an artefact for
carrying a field this contract does not define.

## 6. Out of scope

Each of these is a real choice a workspace must make, and the contract does not
make it. They are listed so an implementer knows they were left open
deliberately.

- Where artefacts sit on disk, and how they are named.
- The form of the declaration naming which files are artefacts, such as a
  directory, a path list, or a registry field.
- Which kinds exist, beyond whichever ones a workspace needs.
- How large an artefact may be, and whether any size limit is enforced.
- Whether staleness is tracked, and what happens when something is stale.
- The vocabulary for grouping work, such as engagements, topics, or milestones,
  and the form in which a unit of work is declared.
- The format of the registry itself.
- The syntax of the frontmatter block, beyond its being delimited and
  machine-findable.
- Which version-control system holds the workspace.
- Whether one workspace's kind names mean anything in another.

On the last of these: anything built against a particular vocabulary, a skill,
or an agent instruction that says "write a handover", is bound to the workspace
it was written for, and moving it to a workspace using different names is
ordinary adaptation work. The contract governs the model, not the words.

The registry's format, the declarations of clauses 2.6 and 4.2.1, and the
version-control system are open here. The conformance suite fixes one form of
each for its own fixtures, so that any implementation can read them. That choice
governs the fixtures alone and places no requirement on a workspace. The suite's
fixtures cover clause 2.5 in an unfamiliar vocabulary, including one that maps a
familiar word to a class other than the one this document's examples give it.

### A worked example

This section is not normative. It describes the workspace the contract was
written in and binds nobody.

Artefacts are arranged by topic. Standing and binding kinds sit directly in a
`context/` directory under uppercase names; episodic and ephemeral kinds sit one
directory below theirs, named `<kind>-<slug>.md`. Hand-off state lives in a
single standing file, limited to fifty tokens per field and twelve items in
total, warning once an item has gone thirty days without a rewrite. A subject
belonging to no topic takes a reserved `item-` prefix. Each unit of work
declares the standing areas of responsibility its work counts against.

Every number above is tuning. A larger corpus would set them differently, and
doing so is not a failure to conform.

## 7. Prior art

This section is not normative.

The model combines two mature bodies of practice. The combination is what is new
here.

**Records management** (ISO 15489, MoReq2010, DoD 5015.2) supplies the governing
principle directly: classification determines disposition. A record's class, not
its content, dictates retention, review, and destruction. Destruction with a
recoverable audit trail is an intended outcome. Section 4 is that principle
applied to a git repository.

**Content modelling** in document management systems supplies the triple, which
maps one to one: content type is `kind`, the information-management policy
attached to that type is `class`, and workflow state is `status`. Retention
labels hanging off content types are exactly the kind-to-class derivation of
clause 2.2.

The per-class state machines echo the status fields of architecture decision
records and Python enhancement proposals. Compaction is the archivist's
appraisal: keep the residue, dispose of the bulk.

Only two things here are new: driving agent behaviour off the taxonomy, so that
an agent matches on kind and inherits a lifecycle; and using signed linear
version control as the disposition archive. The bones are decades old, which is
the point. A shape derived from published standards is one a second
implementation can arrive at independently.
