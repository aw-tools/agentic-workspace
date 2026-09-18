# Going further

A workspace with `aw`, the template and the record is complete on its own. Three
optional layers sit above it. Each is public, and you add it by opting in. No
step-by-step install story for them is written yet.

## Skills

A skill is an instruction file an agent loads for a given task. A member offers
skills only when the manifest opts it in. The member's entry then carries a
`skills` field, naming the directories to read and which skills to take.

`aw bootstrap` and `aw sync` then link each skill into the agent surfaces at the
workspace root. Claude Code and Codex therefore read the same file. One surface
is `.claude/skills/`. The other is `.agents/skills/`.

The `portable-skills` repository publishes two skills:

- `aw-orienting` holds the refresh procedure chapter 6 describes: the cold
  start, the targeted fetch mid-session, what to do on divergence and on a
  failed fetch. The template's `AGENTS.md` points at it when the workspace opts
  in.
- `orchestration-kernel` runs a multi-unit engagement across several agents.

## The orchestration kernel

The kernel is for an engagement with several units of work, dependencies between
them, or work across more than one member at once. One orchestrator session
plans the units of work, writes each worker a self-contained handover,
dispatches it, and keeps the ledger. A worker receives one unit of work and one
report file to write. It receives nothing about the shared state, because the
handover holds everything it needs.

The gates chapter 6 lists stay with you. A worker never waives one, whatever
launched it. Each member's delivery model still governs how that worker's work
lands.

On Claude Code the kernel binds the worker role to two agent definitions. One is
for a worker that writes a repository, and one for a worker that writes nothing.
It ships the guard hook each definition carries, which refuses writes outside
that worker's allowance. The kernel does not publish the definitions themselves.

On other agents, prose in the handover carries the roles, because Codex and
Crush have no such format.

## Dotfiles and sandbox profiles

The `portable-dotfiles` repository holds a host configuration for macOS, Linux
and remote coding environments. The layout suits GNU Stow, so each module maps
onto the same path under your home directory. It covers the shell, the editor
defaults, git, the terminal and the agent instruction links.

The part that matters for a workspace is the `nono` module. `nono` is a
capability-based sandbox that runs a tool with explicit filesystem, environment
and network grants.

The module holds profiles for Claude Code, Codex and Crush, each extending a
shared base profile. The Claude Code and Codex profiles also extend a package
that nono supplies, which you install first. The profiles are examples: review
the grants before use, in particular filesystem writes and credential access.

## Lore

Lore is a local search engine for your own engineering patterns. The patterns
are markdown files in a git repository. Lore indexes them with full-text and
vector search. It serves them to the agent over the Model Context Protocol, so
the agent consults your conventions before it writes code.

It comes in three parts:

- `lore`, the engine, a single Rust binary with one runtime dependency, Ollama,
  for embeddings. Releases carry prebuilt binaries with checksums.
- `lore-patterns`, one author's corpus, shared as a reference for the shape of a
  pattern file rather than as conventions to adopt.
- The Claude Code plugin, inside the engine repository, which adds the search
  server plus hooks that inject a relevant pattern before an edit.

Lore is independent of the workspace. It reads a pattern repository and needs
nothing from the record.

Chapter 8 gives the reasons behind the design.
