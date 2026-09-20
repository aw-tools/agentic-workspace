# Working with agents

An agent session in a workspace follows one loop: refresh, read the record, do
the work, rewrite the record, commit.

## Orientation

The agent reads `AGENTS.md` at the workspace root first. `CLAUDE.md` is a link
to it, so Claude Code and Codex read the same file. `AGENTS.md` sends the agent
to the state file and sets the rules below.

A checkout is one of several. Other sessions and other machines push to the same
remotes, and a stale read looks exactly like a current one. So the agent
refreshes before it reads:

```sh
aw sync
aw status
```

If the workspace is behind its remote and not ahead, the agent fast-forwards it.
If it is both ahead and behind, the agent stops and reports the divergence,
because resolving it is your call. The agent never fast-forwards a member that
is behind, because that working tree belongs to whoever is working in it. It
refreshes the member's remote refs and nothing else.

The agent refreshes again mid-session before three actions:

- editing the state file or a register;
- asserting a member's branch or pull request state;
- resuming after a gap.

If you say you have worked elsewhere, it refreshes in full.

## The durability rule

The agent commits the layer whenever the record changes, without asking. Commits
are signed, linear and made directly to the default branch. If the workspace has
a remote, the agent pushes every commit at once.

A fresh workspace has no remote, so the agent commits only until you add one.
[Chapter 3](03-getting-started.md) says how to choose a remote, and the choice
is yours, never the agent's.

This authority covers the layer alone. It never covers a member or a secret. The
list of things that must not enter the record is in `context/README.md`, and it
is absolute.

## Delivery models

Each member decides how work lands in it, and says so in its own instruction
files. That declaration is the delivery model: whether an agent may commit, push
a branch, open a draft pull request, mark it ready, or merge. The workspace
reads the member's current model every time and keeps no copy.

A member that declares nothing takes the conservative default. The agent
commits, pushes a feature branch and opens a draft pull request, then stops.
Marking the pull request ready and merging it are yours.

Once you merge, the agent restores the checkout without being asked: it checks
out the default branch, fast-forwards it and deletes the merged local branch. A
checkout left on a merged branch reads to the next session as live work.

## Gates

Two actions need your sign-off in every workspace, whatever a member declares:

- The first commit of a member that does not yet exist.
- Creating a remote for any repository.

Both change which repositories exist, so no delivery model can waive them.

An agent that finds a conflict between what it discovers and a settled decision
stops and presents it as a choice with evidence. It does not decide alone.

## Roles

A solo workspace has one agent, and it does everything above. A workspace that
runs several agents at once splits them into two roles. The orchestrator is the
one session that writes the record; it plans, dispatches, and keeps the ledger.
A worker takes one unit of work, writes its member and its own report, and
nothing else.

## Taking template changes

The template records every governed change to itself as a numbered entry in its
own changelog. Your manifest records which entry your workspace last applied.

To bring the layer up to date, the agent clones the template into scratch and
reads the entries above that number. It applies them one at a time, committing
after each. It stops and asks whenever an entry's verification fails or your
copy has diverged from what the entry expects.

The procedure is in the template repository's `CONTRACT.md`. You run it by hand,
because no `aw` verb performs it.

[Chapter 7](07-going-further.md) describes the skill that runs a multi-agent
engagement.
