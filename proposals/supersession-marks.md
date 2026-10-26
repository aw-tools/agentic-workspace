---
amends: 1
---

# Proposal: supersession marks

**Amends contract revision 1.**

## Why

A binding artefact often holds entries recorded over time, such as numbered
decisions. A later entry can supersede an earlier one, wholly or in part: it can
replace it, narrow it, or override part of it. Revision 1 records supersession
only for a whole artefact, in clause 3.7.

Within one artefact, only the later entry can name what it supersedes, because
clause 3.6 forbids changing the earlier one's body content. A reader who reaches
the earlier entry first, for example by searching, sees nothing to say a later
entry supersedes it.

This proposal lets both entries carry a mark naming the other. A mark records
how entries relate and leaves unchanged what each entry decided, which is what
clause 3.6 exists to keep. A mark may therefore be added, changed, or removed at
any time, and history records each change. A workspace can then mark the
supersessions it recorded before opting in, and correct a wrong mark.

Adding a mark to an entry can move the line breaks of its paragraph, when the
text is wrapped to a fixed width. Clause 3.6 therefore protects what body
content says, not how the artefact is laid out.

## Definitions

These definitions join section 3, after the table and before clause 3.1:

> An entry is a separately identified part of a binding artefact, such as one
> numbered decision. A supersession mark is machine-readable text on an entry,
> marked off from the entry's prose, naming entries by their identifiers. A
> **supersedes** mark names entries that the entry carrying it supersedes,
> wholly or in part. A **superseded-by** mark names entries that supersede the
> entry carrying it, and tells a reader to read them before relying on that
> entry.

## Open choice

This item joins the choices section 6 leaves open, after the one on the syntax
of the frontmatter block:

> - How a binding artefact is laid out, how its entries are delimited and
>   identified, and how supersession marks are written.

## Clauses

Clause 3.6 is replaced, and clauses 3.8 and 3.9 follow clause 3.7:

> **3.6** A binding artefact MUST be append-only: body content once recorded
> MUST NOT be changed, and new body content MUST follow it. The frontmatter is
> not body content, and neither are supersession marks or the artefact's layout,
> such as where its lines break.
>
> **3.8** A supersession mark MUST name entries of its own artefact.
> Supersession marks MUST agree: entry A carries a superseded-by mark naming
> entry B exactly when B carries a supersedes mark naming A.
>
> **3.9** A supersedes mark MUST name only entries that come before the entry
> carrying it in the artefact.

None of revision 1's fixtures is replaced.

## Editorial changes

Two sentences of revision 1 stop being accurate once the clauses above are taken
in:

- In section 3, under "The purpose of each class", "never editing what is
  already written" becomes "never changing recorded body content".
- In clause 4.1.2, "such as a binding entry" becomes "such as a binding
  artefact".

## Harness convention

On acceptance, this joins the suite's description of its harness convention:

> The fixtures fix one form for a binding artefact. An entry is a numbered list
> item whose number starts in the first column of its line, identified by that
> number, unique within the artefact. A supersedes mark is `**Supersedes:**` and
> a superseded-by mark is `**Superseded by:**`. Each is followed by a space, one
> or more entry numbers separated by a comma and a space, and a full stop.
> Outside the frontmatter and the marks, body content is the sequence of entries
> and the sequence of words, a word being a maximal run of characters other than
> whitespace. Everything else is layout.

## Set aside

- **A mark on the later entry only.** A reader reaching the earlier entry would
  still see nothing.
- **A separate index of supersessions.** A reader reaching an entry does not see
  it.
- **A clarification inside revision 1.** Marks change what clause 3.6 accepts,
  and the revision number increments when a normative clause changes.
