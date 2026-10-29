---
amends: 1
---

# Proposal: supersession marks

**Amends contract revision 1.**

## Why

Binding artefacts often record entries over time, such as numbered decisions,
whether one artefact holds them all or each has its own. A later entry can
supersede an earlier one, wholly or in part: it can replace it, narrow it, or
override part of it. Revision 1 records supersession only for a whole artefact,
in clause 3.7.

Only the later entry can name what it supersedes, because clause 3.6 forbids
changing the earlier one's body content. A reader who reaches the earlier entry
first, for example by searching, sees nothing to say a later entry supersedes
it.

This proposal lets both entries carry a mark naming the other. A mark records
how entries relate and leaves unchanged what each entry decided, which is what
clause 3.6 exists to keep. A mark may therefore be added at any time, so a
workspace can mark the supersessions it recorded before opting in. Readers and
implementations rely on the entries a mark names, so once named, an entry stays
named, but the form of a mark may change. A workspace that noted supersessions
in an older form, here called legacy notes, can then rewrite each as a mark.

Adding a mark to an entry can move the line breaks of its paragraph, when the
text is wrapped to a fixed width. Clause 3.6 therefore protects what body
content says, not how the artefact is laid out. Where an artefact holds entries,
text outside them, such as a heading, only frames them, so it may change. How
entries are held, identified, ordered and laid out stays the workspace's choice,
so the clauses hold the same whether one artefact records every entry or each
entry is an artefact of its own.

Some changes of form reach the words, such as renaming a label on every entry,
or turning a register of headings into a numbered list. A comparison of words
cannot tell them from a change of meaning, and a list of what counts as form
would only grow. A commit may therefore declare a format change.

Every entry already recorded keeps its identifier and its order, and the commit
adds no entry unless no artefact it declares held one. Review of the wording
then sees each change of words once, with any entries it touches.

## Definitions

These definitions join section 3, after the table and before clause 3.1:

> An entry is a separately identified part of what binding artefacts record,
> such as one numbered decision. A binding artefact may hold several entries or
> be a single entry. Entries have an order, such as the order of their numbers.
> A supersession mark is machine-readable text on an entry, marked off from the
> entry's prose, that carries nothing but the identifiers of the entries it
> names. A **supersedes** mark states that the entry carrying it supersedes the
> entries it names, wholly or in part. A **superseded-by** mark states that the
> entries it names supersede the entry carrying it, and tells a reader to read
> them before relying on that entry.
>
> A legacy note is machine-readable text on an entry, marked off from the
> entry's prose, that names the entries superseding it, in a form the workspace
> used before it wrote supersession marks. It is not a supersession mark.

## Open choice

This item joins the choices section 6 leaves open, after the one on the syntax
of the frontmatter block:

> - Whether a binding artefact holds several entries or one, how entries are
>   identified and ordered, how a binding artefact is laid out, and how
>   supersession marks and legacy notes are written.

## Clauses

Clause 3.6 is replaced, and clauses 3.8 to 3.10 follow clause 3.7:

> **3.6** A binding artefact MUST be append-only: body content once recorded
> MUST NOT be changed, and new body content MUST follow it, except in a format
> change of that artefact under clause 3.10. The frontmatter, supersession
> marks, legacy notes and the artefact's layout are not body content. Text
> outside the entries of an artefact whose last recorded version holds an entry
> is not body content either. An entry's supersedes set holds the identifiers
> its supersedes marks name, and its superseded-by set those its superseded-by
> marks and legacy notes name. An identifier once recorded in either set MUST
> stay in that set.
>
> **3.8** Supersession marks MUST agree: entry A carries a superseded-by mark
> naming entry B exactly when B carries a supersedes mark naming A.
>
> **3.9** A supersedes mark MUST name only entries that come before the entry
> carrying it in the order of entries.
>
> **3.10** A binding artefact's frontmatter MAY carry a `reformatted` field,
> with any value. A commit that adds the field to an artefact already recorded,
> or changes the field's value there, declares a format change of that artefact.
> The field's value counts as changed when the text written for it changes,
> leaving aside white space around it, even where a parser reads both texts as
> the same value. Removing the field declares nothing, and neither does an
> artefact the commit creates. A commit that declares a format change of an
> artefact MAY change how that artefact's body content is worded, and MUST NOT
> change what it says. Each entry an implementation reads in that artefact's
> last recorded version MUST remain in it, with the same identifier and in the
> same order; an entry it cannot read in the new version counts as removed.
> Where it reads an entry in the last recorded version of any artefact the
> commit declares, the commit MUST NOT add an entry to any artefact. An
> implementation compares no words in an artefact the commit declares. It MUST
> report the changes of words in the body content of the artefacts the commit
> declares, in groups. A group holds every such change that removes the same
> words and inserts the same words. A group touches an entry when one of its
> changes falls within that entry in its artefact's last recorded version. The
> report MUST name any entries each group touches. The implementation SHOULD
> warn of a group that touches only one or two entries, and of a declared
> artefact in whose last recorded version it reads no entry.

None of revision 1's fixtures is replaced.

## Editorial changes

Three passages of revision 1 need new wording once the clauses above are taken
in:

- In section 3, under "The purpose of each class", "never editing what is
  already written" becomes "never changing what recorded body content says".
- In clause 4.1.2, "such as a binding entry" becomes "such as a binding
  artefact".
- In section 6, "and the version-control system are open here" becomes "the form
  of entries, of supersession marks and of legacy notes, and the version-control
  system are open here".

## Harness convention

On acceptance, the suite's description of its harness convention gains this
paragraph, and its opening sentence lists "the form of entries, of supersession
marks and of legacy notes" after "the frontmatter syntax":

> The fixtures fix one form for entries, supersession marks and legacy notes. An
> entry is an item of a numbered list at the top level of a binding artefact's
> body. It holds everything after that item up to whichever comes first: another
> such item, a top-level heading, a top-level thematic break, or the end of the
> body. Its identifier is the number written before it, unique among the
> fixture's binding artefacts, and entries are ordered by number. Lists nested
> in an entry are part of it. A supersedes mark is `**Supersedes:**` and a
> superseded-by mark is `**Superseded by:**`, each followed by one or more entry
> numbers separated by commas and ending in a full stop. A legacy note is
> `SUPERSEDED by` followed by one or more entry numbers separated by commas, and
> any full stop directly after the last number. A mark or a legacy note may
> begin or end inside a word, and what is left of that word stays a word of its
> own. Neither counts inside a code span or code block. Any run of white space,
> line breaks included, may separate the words of a mark or a legacy note.
> Outside the frontmatter, the marks and the legacy notes, body content is each
> entry's identifier with its sequence of words. Where the artefact's last
> recorded version holds no entry, the sequence of words outside any entry is
> body content too. A word is a maximal run of characters other than Unicode
> white space. Everything else is layout.

## Set aside

- **A mark on the later entry only.** A reader reaching the earlier entry would
  still see nothing.
- **A separate index of supersessions.** A reader reaching an entry does not see
  it.
- **Marks naming only entries of their own artefact.** A workspace keeping each
  entry in an artefact of its own could mark nothing.
- **A clarification inside revision 1.** Marks change what clause 3.6 accepts,
  and the revision number increments when a normative clause changes.
- **Legacy notes kept as body content.** Entries would carry two forms for good.
- **Legacy notes read as marks.** Clauses 3.8 and 3.9 would then hold notes
  written before marks existed to rules they were not written to meet.
- **Only the rewrite from legacy note to mark.** Other changes of form that keep
  the identifiers lose nothing a reader relies on either.
- **A specification of the fields of an entry.** How entries are laid out stays
  the workspace's choice.
- **Labels read as layout.** That covers one syntax of label, and lets any
  commit reverse a ruling by changing a label.
- **Labels and headings named as layout.** Every other change of form would stay
  blocked until a revision named it.
- **Each rename declared by the release that makes it.** A change of form no
  release declared would stay blocked.
- **Failing a change that touches one or two entries.** A genuine change to a
  label used once would fail too.
