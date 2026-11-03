---
amends: 1
---

# Proposal: supersession marks

**Amends contract revision 1.**

## Why

Binding artefacts often record entries over time, such as numbered decisions. A
later entry can supersede an earlier one, wholly or in part. Revision 1 records
supersession only for a whole artefact, in clause 3.7.

Clause 3.6 forbids changing the earlier entry, so only the later one can name
what it supersedes. A reader who reaches the earlier entry first sees no sign of
the later one. This proposal lets both entries carry a mark naming the other.

A mark records how entries relate, never what an entry decided, so a mark may be
added or reworded at any time, and a workspace can mark supersessions it
recorded before opting in. Readers and implementations rely on the entries a
mark names, so an entry once named stays named. A workspace that noted
supersessions in an older form, here called legacy notes, can rewrite each as a
mark.

Adding a mark can move the line breaks of a wrapped paragraph. Clause 3.6
therefore protects what body content says, not how it is laid out. Where an
artefact holds entries, text outside them only frames them, so it may change.
How entries are held, identified, ordered and laid out is the workspace's
choice, so the clauses hold the same for one artefact or one per entry.

Some changes of form reach the words, such as renaming a label on every entry or
turning headings into a numbered list. A comparison of words cannot tell them
from a change of meaning, and a list of what counts as form would only grow. A
commit may therefore declare a format change. The change keeps every entry, its
identifier and its order, adds none, and review sees each change of words once.

## Definitions

These definitions join section 3, after the table and before clause 3.1:

> An entry is a separately identified part of what binding artefacts record,
> such as one numbered decision. A binding artefact may hold several entries or
> be a single entry. Entries have an order, such as the order of their numbers.
>
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

Clause 3.6 is replaced, and clauses 3.8 to 3.18 follow clause 3.7:

> **3.6** A binding artefact MUST be append-only: body content once recorded
> MUST NOT be changed, and new body content MUST follow it, except as clause
> 3.14 allows.
>
> **3.8** An implementation MUST NOT count the frontmatter, supersession marks,
> legacy notes or the artefact's layout as body content.
>
> **3.9** Where an artefact's last recorded version holds an entry, an
> implementation MUST NOT count text outside its entries as body content.
>
> **3.10** An entry's supersedes set holds the identifiers its supersedes marks
> name, and its superseded-by set those its superseded-by marks and legacy notes
> name. An identifier once recorded in either set MUST stay in that set.
>
> **3.11** Supersession marks MUST agree: entry A carries a superseded-by mark
> naming entry B exactly when B carries a supersedes mark naming A.
>
> **3.12** A supersedes mark MUST name only entries that come before the entry
> carrying it in the order of entries.
>
> **3.13** A binding artefact's frontmatter MAY carry a `reformatted` field,
> with any value. A commit that adds the field to an artefact already recorded,
> or changes its value there, declares a format change of that artefact. The
> value counts as changed when the text written for it changes, white space
> around it aside, even where a parser reads both texts as the same value.
> Removing the field declares nothing, and neither does an artefact the commit
> creates.
>
> **3.14** A commit that declares a format change of an artefact MAY change how
> that artefact's body content is worded, and MUST NOT change what it says. An
> implementation MUST NOT reject the commit under clause 3.6 for a change of
> words in that artefact.
>
> **3.15** Each entry an implementation reads in the last recorded version of an
> artefact whose format change a commit declares MUST remain in it, with the
> same identifier and in the same order. An entry it cannot read in the new
> version counts as removed.
>
> **3.16** Where an implementation reads an entry in the last recorded version
> of any artefact whose format change a commit declares, the commit MUST NOT add
> an entry to any artefact.
>
> **3.17** An implementation MUST report the changes of words in the body
> content of the artefacts whose format change a commit declares, in groups, and
> name the entries each group touches. A group holds every such change that
> removes the same words and inserts the same words. A group touches an entry
> when one of its changes falls within that entry in its artefact's last
> recorded version.
>
> **3.18** An implementation SHOULD warn of a group that touches only one or two
> entries, and of an artefact whose format change a commit declares and in whose
> last recorded version it reads no entry.

None of revision 1's fixtures is replaced.

## Editorial changes

Three passages of revision 1 change with the clauses above:

- In section 3, under "The purpose of each class", "never editing what is
  already written" becomes "never changing what recorded body content says".
- In clause 4.1.2, "such as a binding entry" becomes "such as a binding
  artefact".
- In section 6, "and the version-control system are open here" becomes "the form
  of entries, of supersession marks and of legacy notes, and the version-control
  system are open here".

## Harness convention

On acceptance, the suite's description of its harness convention gains these
paragraphs, and its opening sentence lists "the form of entries, of supersession
marks and of legacy notes" after "the frontmatter syntax":

> The fixtures fix one form for entries, supersession marks and legacy notes.
> Headings, lists and thematic breaks are read as CommonMark 0.31.2 reads them.
> An entry is an item of a numbered list at the top level of a binding
> artefact's body, or a level-two heading there whose text opens with a number,
> a full stop and white space. Such a heading is written `##`, with no closing
> sequence of `#`. Where a version of an artefact holds a heading entry, no list
> item in that version is an entry. An entry holds everything after its number
> and full stop up to whichever comes first: the next entry, a top-level
> heading, a top-level thematic break, or the end of the body. A heading of
> level three or more does not end a heading entry. An entry's identifier is the
> number written before it, unique among the fixture's binding artefacts, and
> entries are ordered by number.
>
> A supersedes mark is `**Supersedes:**` and a superseded-by mark is
> `**Superseded by:**`, each followed by one or more entry numbers separated by
> commas and ending in a full stop. A legacy note is `SUPERSEDED by` followed by
> one or more entry numbers separated by commas, and any full stop directly
> after the last number. Any run of white space, line breaks included, may
> separate the words of a mark or a legacy note. A mark or a legacy note may
> begin or end inside a word, and what is left of that word is a word of its
> own. Neither counts inside a code span or a code block.
>
> Outside the frontmatter, the marks and the legacy notes, body content is each
> entry's identifier with its sequence of words. Where the artefact's last
> recorded version holds no entry, the words outside any entry are body content
> too. A word is a maximal run of characters other than Unicode white space.
> Everything else is layout.

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
- **Legacy notes read as marks.** Clauses 3.11 and 3.12 would then hold notes
  written before marks existed to rules they were not written to meet.
- **Only the rewrite from legacy note to mark.** Other changes of form that keep
  the identifiers lose nothing a reader relies on either.
- **A specification of the fields of an entry.** How entries are laid out stays
  the workspace's choice.
- **Changes of form named one by one, by a revision or a release.** A label read
  as layout covers one syntax of label and lets any commit reverse a ruling by
  changing it. Whatever a revision or a release named, every other change of
  form would stay blocked until one named it too.
- **Failing a change that touches one or two entries.** A genuine change to a
  label used once would fail too.
