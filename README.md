# Agentic Workspace

Lifecycle rules for the documents AI agents leave behind.

An agentic workspace accumulates plans, handovers, decisions, and notes. This
repository holds two documents about them: a guide for people and a contract for
implementations.

## The guide

[The guide](guide/01-what-this-is.md) is the human narrative of the whole
toolkit: what a workspace is, how to start one, what the record holds, and how
to work in it with agents. Eight chapters and a glossary, under `guide/`, read
in order. Start there.

## The contract

[The contract](SPEC.md) says how to classify each document a workspace
accumulates and what eventually happens to it, in twenty-six numbered clauses
any implementation can be tested against. It names no tool.

The conformance suite under `suite/` proves it. One directory per revision holds
the fixtures, each a small workspace with the verdict a conforming
implementation must give. An implementation conforms to a revision when it gives
the expected verdict on every fixture of that revision.

## Contributing

Not accepting external contributions yet. See
[CONTRIBUTING.md](CONTRIBUTING.md).

## Licence

Copyright 2026 Front Seed Labs Ltd.

The specification and the guide are licensed under
[CC BY 4.0](LICENSE-CC-BY-4.0). The conformance suite under `suite/` is licensed
under either [MIT](LICENSE-MIT) or [Apache-2.0](LICENSE-APACHE), at your option.
