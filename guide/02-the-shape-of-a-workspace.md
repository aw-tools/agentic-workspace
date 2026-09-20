# The shape of a workspace

A workspace is one git repository with other git repositories inside it. It
tracks a thin layer of its own files and nothing else.

## The workspace

The workspace starts from a template. Its `.gitignore` denies everything first,
then allows the layer file by file. Git therefore never descends into a member
and never records one as a submodule. Clone the workspace anywhere and only the
layer arrives.

The layer holds:

- `workspace.toml`, the manifest. It names the workspace and lists each member:
  its path inside the workspace, its clone URL and, if needed, its branch.
- `AGENTS.md`, the instructions an agent reads first. `CLAUDE.md` is a link to
  it.
- `bin/`, the scripts. `bin/bootstrap` calls `aw bootstrap`.
  `bin/lint-artefacts` checks the record against the contract before each
  commit.
- `context/`, the record itself.
- `README.md`, for the humans who open the repository on its host.

## The members

A member is a code repository checked out at the path the manifest gives it. The
workspace does not own it. Each member keeps its own remote and branches, and
its own rules for how work lands. `aw bootstrap` clones any member that is
missing, and `aw sync` fetches every member at once.

A member may also offer skills, which are instruction files an agent can load
for a given task. A member offers them only when the manifest opts it in.

## The record

`context/` holds these files and directories:

- `STATE.md`, the state file. It holds one item per live thread of work. Each
  item has four fields: now, next, blocked, and where its ledger is. A session
  reads this file whole, so it stays short.
- `DECISIONS.md`, the decision register. Numbered entries, each with its reason.
  An entry is added at the end and never edited.
- `FINDINGS.md`, the findings file. Facts about the environment that a session
  checked, such as which tool version is installed on which machine.
- `NOTES.md`, for friction and observations not yet routed anywhere.
- `artefacts.toml`, the machine half of the registry. It lists the kinds of file
  the record may hold and how each one lives and ends.
- `artefacts.md`, the prose half of the registry. It explains each kind and
  gives its template.
- `engagements/`, one directory per engagement. Each holds a plan, a ledger, and
  any reports, plus two optional directories: `attachments/` for tracked source
  material and `tmp/` for untracked scratch.

Every file in the record declares what kind of file it is and its current status
in a short header. The registry says which statuses each kind may take. The lint
refuses a commit that breaks those rules.

## What is durable

Durable means committed to the workspace. A remote is not required, but if the
workspace has one, every commit is pushed there too.
[Chapter 3](03-getting-started.md) says how to choose one.

- The layer above is durable. A session commits it often, every time the state
  file changes.
- The members' contents are never durable through the workspace. They are
  durable through their own repositories.
- `tmp/` directories are not durable. They are untracked scratch and can be
  emptied at any time.
- Secrets are never durable. The record goes wherever the workspace goes, so
  nothing goes in it that every copy may not hold.

[Chapter 3](03-getting-started.md) creates a workspace and runs the first
session in it.
