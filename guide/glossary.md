# Glossary

Each thing in the guide has one name, and every chapter uses that exact name.
The last column links to the chapter that introduces the term and explains it.

| Term                 | Meaning                                                                                                        | Chapter                             |
| -------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| session              | One run of a human or an agent against the workspace, from opening it to closing it.                           | [1](01-what-this-is.md)             |
| the record           | Everything a session knew that the code does not hold. Lives in `context/`.                                    | [1](01-what-this-is.md)             |
| workspace            | The outer git repository that holds the record, with members inside it.                                        | [1](01-what-this-is.md)             |
| member               | A code repository checked out inside the workspace at the path the manifest gives it.                          | [1](01-what-this-is.md)             |
| state file           | `STATE.md`. What is live, next, blocked, and where each ledger is.                                             | [1](01-what-this-is.md)             |
| decision register    | `DECISIONS.md`. Numbered decisions with reasons; entries are added, never changed.                             | [1](01-what-this-is.md)             |
| findings file        | `FINDINGS.md`. Verified facts about the environment.                                                           | [1](01-what-this-is.md)             |
| engagement           | One piece of work with a start and an end. Has a directory under `engagements/`.                               | [1](01-what-this-is.md)             |
| plan                 | What an engagement intends to do, in units of work with acceptance criteria.                                   | [1](01-what-this-is.md)             |
| ledger               | The log of what an engagement ran and what it showed.                                                          | [1](01-what-this-is.md)             |
| contract             | The written definition of the layout of the record.                                                            | [1](01-what-this-is.md)             |
| conformance suite    | The set of example workspaces a conforming tool must accept or reject.                                         | [1](01-what-this-is.md)             |
| template             | The repository `aw init` clones to create a workspace.                                                         | [1](01-what-this-is.md)             |
| the layer            | The files the workspace tracks: manifest, instructions, scripts, the record.                                   | [2](02-the-shape-of-a-workspace.md) |
| manifest             | `workspace.toml`. Names the workspace and lists the members.                                                   | [2](02-the-shape-of-a-workspace.md) |
| skill                | An instruction file an agent loads for a given task. A member offers skills only when the manifest opts it in. | [2](02-the-shape-of-a-workspace.md) |
| thread of work       | One live line of work. The state file holds one item for each.                                                 | [2](02-the-shape-of-a-workspace.md) |
| registry             | `artefacts.toml` and `artefacts.md`. The kinds of file the record may hold and how each lives and ends.        | [2](02-the-shape-of-a-workspace.md) |
| lint                 | `bin/lint-artefacts`. Checks the record against the contract before each commit.                               | [2](02-the-shape-of-a-workspace.md) |
| durable              | Committed to the workspace, and pushed if it has a remote.                                                     | [2](02-the-shape-of-a-workspace.md) |
| frontmatter          | The short header at the start of a file in the record, naming its kind and status.                             | [4](04-the-record.md)               |
| kind                 | What a file in the record is: a plan, a ledger, a handover. Declared in the frontmatter.                       | [4](04-the-record.md)               |
| status               | Where a file is in its life. Declared in the frontmatter; legal values come from the class.                    | [4](04-the-record.md)               |
| class                | A file's lifetime: standing, binding, episodic or ephemeral. Derived from the kind, never written.             | [4](04-the-record.md)               |
| handover             | The instructions that launch a worker on one unit of work. Ephemeral.                                          | [4](04-the-record.md)               |
| report               | The result a worker returns: what was done, what needs a decision, what is blocked. Ephemeral.                 | [4](04-the-record.md)               |
| graduation           | The moment an ephemeral file's residue is recorded in a durable file and the file is deleted, in one commit.   | [4](04-the-record.md)               |
| remits               | The standing areas of responsibility an engagement's work counts against.                                      | [4](04-the-record.md)               |
| unit of work         | One dispatchable piece of an engagement, with its own acceptance criteria.                                     | [4](04-the-record.md)               |
| disposition          | What happens to a file in the record at the end of its life.                                                   | [5](05-the-contract.md)             |
| revision             | The contract's integer version. Moves when a clause changes, never for an editorial change or a tool release.  | [5](05-the-contract.md)             |
| fixture              | One example workspace in the conformance suite, with the verdict a conforming tool must give.                  | [5](05-the-contract.md)             |
| delivery model       | A member's own declaration of how work lands in it: commit, branch, draft pull request, ready, merge.          | [6](06-working-with-agents.md)      |
| gate                 | An action that needs your sign-off, whatever a delivery model says.                                            | [6](06-working-with-agents.md)      |
| orchestrator         | The one session that writes the record in a multi-agent engagement: plans, dispatches, keeps the ledger.       | [6](06-working-with-agents.md)      |
| worker               | A session that takes one unit of work, writes its member and its own report, and nothing else.                 | [6](06-working-with-agents.md)      |
| orchestration kernel | The skill that runs orchestrator and workers across a multi-unit engagement.                                   | [7](07-going-further.md)            |
| agent definition     | A Claude Code file that names an agent and sets its tools and hooks.                                           | [7](07-going-further.md)            |
| profile              | A file that lists what a sandboxed agent may read, write and reach.                                            | [7](07-going-further.md)            |
| pattern              | One markdown file in a lore corpus, holding a convention or practice for an agent to apply.                    | [7](07-going-further.md)            |
