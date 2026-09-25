# What this is

`aw` keeps the record of software work alive between sessions. The record is
everything a session knew that the code does not hold: what was being done, why,
what was decided, and what comes next.

## The problem

Work with an AI coding agent happens in sessions. An agent such as Claude Code
or Codex holds what it knows in a context. A session ends when that context
fills, when the terminal closes or when you move to another machine.

What the session knew dies with it. It knew the task and the reason for it. It
knew which decisions were taken, which attempts were rejected, and what to do
next. None of that survives the end of the session on its own.

The code repository is the wrong place for the record, because it holds the code
and not the thread of working on it. Commit messages and pull requests capture
fragments. Work often spans several repositories at once, so no single
repository can hold the whole record.

So the next session starts cold. Whether a human or an agent runs it, it
rebuilds the record from memory, chat scrollback and guesswork.

## The answer

A workspace is an outer git repository that holds the record. The code
repositories are checked out inside it as members. The workspace ignores the
contents of its members, so it never commits their code. It commits only its own
files, among them a directory named `context/`.

That directory holds five kinds of record:

- A state file: what is live, what is next, what is blocked.
- A decision register, which lists each decision with a number and a reason.
  Entries are added and never changed.
- A findings file, which holds verified facts about the environment.
- A notes file, for friction and observations not yet routed anywhere.
- One directory per engagement, an engagement being one piece of work with a
  start and an end. Each holds a plan and a ledger, the ledger being the log of
  what ran and what it showed.

The `aw` command line tool manages the workspace:

- `aw init` creates a workspace from a template.
- `aw bootstrap` clones the members on a new machine.
- `aw sync` fetches everything at once.
- `aw status` reports each repository's position and whether its working tree is
  clean.
- `aw fast-forward` brings the members that are safe to move up to date with
  their remotes.
- `aw doctor` checks the environment, the remotes, and the skill links.
- `aw adopt` adds a checkout you already have to the manifest.

A session starts by reading the state file. Then it does the work. Then it
rewrites the state file and commits it. Because the record lives in the
workspace, it moves with that repository to any machine.

A written contract defines the layout. A conformance suite, a set of example
workspaces that a conforming tool must accept or reject, proves that a tool
follows the contract. So other tools can read the same record.

[Chapter 2](02-the-shape-of-a-workspace.md) describes the shape of a workspace
in detail, and [chapter 3](03-getting-started.md) shows how to start.
