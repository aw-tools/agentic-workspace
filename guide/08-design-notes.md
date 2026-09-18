# Design notes

Each note gives the reason for one choice the earlier chapters state. The
choices are settled. The notes exist so you can judge whether they fit your
case.

## Why the record lives outside the code

A code repository exists for its code. Its commits, branches and pull requests
describe changes to that code, and its readers expect nothing else there. The
thread of working on it, what was tried, why it was rejected and what comes
next, has no natural place in it. Work also spans repositories, and a thread
that crosses two of them cannot live in either.

So the record has a repository of its own, and the code repositories sit inside
it as members. The workspace ignores their contents, so a clone of it is small
and carries only the record. A member keeps its own remote, branches and rules,
because the workspace holds only the record of the work.

## Why the state file is rewritten

A file that is only appended to grows into a log, and a log answers what
happened rather than what is true now. A session resuming from a log must read
the whole log and reconstruct the present. A session rewrites the state file
instead: each update replaces an item's fields with the present state. A reader
then takes the file as current.

The caps exist for the same reason. Every session reads the file whole at its
start, so its size is a cost paid every time. You raise a cap that is too tight
in one line of the registry, with a reason in the commit.

## Why graduation is deletion

Whether you keep a document and whether it sits in the working tree are separate
questions. Version control already holds every version of every file, so a
second copy in an archive directory adds nothing. It also costs every reader who
has to work out whether that copy is live.

A handover that has been consumed, and a report whose findings are recorded,
describe nothing about the present. So the same commit that records the residue
deletes the file.

The rule is strict in one place. An implementation must reject the deleting
commit when no durable file changes in it, because a deletion with no residue
recorded is a loss. Recovery is one git command away.

## Why the class is derived

A file could carry its class as a third field beside kind and status. It does
not, because a hand-typed copy of a computed field drifts from the field it came
from. Nothing then says which of the two is right. Deriving the class from the
kind through the registry makes a lifetime change one registry row instead of an
edit to every file.

The same reasoning keeps the kind names out of the contract. The names are a
workspace's own vocabulary. Fixing a set in the contract would reserve ordinary
words for every workspace and turn a description of one into a requirement on
all.

## Why the contract names no tool

A record that only one binary can read is tied to that binary for life. The
contract defines the model and not the tool, so that a second implementation can
pass the same fixtures and read the same record. The suite makes that claim
testable rather than asserted.

The contract and `aw` also live in separate repositories, with separate revision
lines. You report a defect in the definition against the spec repository, and a
release of the binary moves no clause.

## Why the model is old

Records management has held for decades that classification determines
disposition. A record's class decides how long it is kept and how it is
destroyed, and its content does not. Content modelling in document systems
supplies the same triple of type, policy and state. The contract applies both to
a git repository.

The taxonomy of kinds and classes drives agent behaviour, so an agent matches on
kind and inherits a lifetime. Signed, linear version control is the disposition
archive. Those two parts are new.

The model is old on purpose. A shape derived from published practice is one a
second implementer can arrive at on their own.
