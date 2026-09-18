# The record

The record is the set of files under `context/` that carry what a session knew.

## Kinds and classes

Every file in the record opens with a short header, called frontmatter, that
names its kind and its status. The kind says what the file is. The status says
where it is in its life. A third property, the class, is never written in the
file: the registry derives it from the kind.

The class decides the file's lifetime. There are four:

- Standing: answers what is true now. It is rewritten in place and never
  appended to. Its states are `live` and `retired`.
- Binding: answers what was decided and why. Content once written is never
  changed; new content follows it. Its states are `active` and `superseded`.
- Episodic: governs one stretch of work and loses its authority when that work
  ends. Its states are `open`, `closed` and `compacted`.
- Ephemeral: serves a single exchange between sessions and is worth nothing
  afterwards. Its states are `open`, `graduated` and `expired`, and in either
  terminal state the file leaves the working tree.

The template ships nine kinds:

- `state` and `doctrine` are standing.
- `decision` is binding.
- `spec`, `plan` and `ledger` are episodic.
- `handover`, `report` and `probe` are ephemeral.

A workspace may add kinds, because the contract fixes the classes and not the
names.

## The standing files

`STATE.md` is the state file, the one standing file every session reads whole.
It holds one item per live thread of work. Each item has four fields: now, next,
blocked, and the path of the ledger that holds the detail. Each item heading
carries the date it was last rewritten.

The file also holds an ordered list of what to pick up next, one line per item.
The registry caps four things so the state file stays readable:

- the number of items;
- the length of each field;
- the length of the next list;
- the days an item may go without a rewrite.

A grown workspace raises a cap in the registry, in one line, rather than
amending the lint.

`FINDINGS.md`, the findings file, holds facts about the environment that a
session checked: which tool version is on which machine, what a sandbox forbids.
A fact goes there once verified, never from memory. `NOTES.md` holds friction
and observations that have no home yet.

## The decision register

`DECISIONS.md` is the one binding file. Each entry has a number, the ruling, the
reason that carried it, and one line per alternative rejected. Entries are added
at the end and never edited or renumbered, so a citation such as "decision 40"
means the same thing forever. When a later entry overrides an earlier one, the
earlier one gains an inline `SUPERSEDED by N` marker naming its successor.

## Engagements

An engagement is one piece of work with a start and an end. It has a directory
under `context/engagements/`, and an entry in the registry. The entry says
whether the engagement is open or closed, and which standing areas of
responsibility its work counts against, called its remits. Inside the directory,
every file is named by its kind and a slug, such as `plan-milestone.md`.

An engagement holds:

- A plan: the units of work, their dependencies and their acceptance criteria.
- A ledger: the log of what ran and what it showed, including the command and
  its output, pasted verbatim. "Tests pass" without output is not a record.
- Handovers and reports: the instructions that launch a worker on one unit of
  work, and the result that comes back.

Closing an engagement closes its plan and ledger in the same commit, and moves
its directory to `context/archive/`. The handovers and reports are gone by then,
because they graduated as they were consumed.

## Graduation is deletion

An ephemeral file graduates when what it held reaches a durable file: a ledger
entry, a decision, a line in the state file. The same commit that records that
residue deletes the file. The record therefore holds only what still describes
the present. A reader never has to ask whether a file is live.

Nothing is lost, because git keeps every version of every file. To recover a
deleted file, run:

```sh
git log --diff-filter=D --name-only
```

## The lint

`bin/lint-artefacts` runs on every commit through the pre-commit hook. It reads
the registry, then checks the markdown files the commit stages:

- that the frontmatter is present;
- that the kind is registered;
- that the status belongs to the kind's class;
- that the filename matches the kind;
- that a closed engagement holds no open plan or ledger.

It also checks the state file's caps. A commit that fails is refused.

The registry is two files. `artefacts.toml` is the machine authority: the
classes, their states, each kind's class and naming rule, the state file's caps,
and the engagement register. `artefacts.md` is the prose beside it, giving each
kind's purpose and template.

Chapter 5 explains the contract these rules come from.
