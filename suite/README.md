# Conformance suite

Fixtures an implementation of [the contract](../SPEC.md) is tested against.
Conforming to revision N means passing every fixture under `rev<N>/`. The suite
is data: it ships no runner, and each implementation brings its own.

## A fixture

One directory per fixture, named `<clause>-<slug>`, holding:

- `fixture.toml`, the test itself. `clause` names the clause under test;
  `outcome` is `pass` or `fail`, the verdict a conforming implementation reaches
  for the whole fixture; `artefacts` lists the globs naming which files are
  artefacts (clause 2.6); `[[unit]]` tables in `units.toml` declare units of
  work (clause 4.2.1), each with `name`, `state` of `open` or `closed`, and
  `artefacts`, the paths belonging to it; the file is absent when the fixture
  declares none.
- `registry.toml`, mapping each kind the fixture uses to its class (clause 2.3),
  with the states of each class.
- The artefact files, with YAML frontmatter. A binding artefact that reaches
  `superseded` names its successor in a `superseded_by` field (clause 3.7).

A fixture whose clause concerns commits holds no files at the top level beyond
`fixture.toml`; instead `steps/01`, `steps/02`, … each hold a full working tree.
A runner creates an empty repository, copies each step in order, commits it, and
hands the result to the implementation. `registry.toml` sits inside each step.

The verdict is the outcome alone. Which clause an implementation cites, and in
what words, is its own business.

## The harness convention is not normative

The contract leaves open the registry format, the form of the two declarations,
the frontmatter syntax and the version-control system. This suite fixes one of
each so that any implementation can load its fixtures: TOML for the registry and
`fixture.toml`, YAML for frontmatter, git for steps. That choice binds the
fixtures alone. A workspace using any other machine-readable form conforms just
the same; an implementation of it runs the suite through a loader or a converter
of its own.

## Coverage

Every clause has at least one fixture. Two have no failing fixture, because the
harness satisfies them by construction: clause 1.2.1, since a runner always
places a fixture under version control, and clause 2.6, since `fixture.toml`
always declares the artefacts. Clause 4.1.3 is a permission and has nothing to
violate. The SHOULD clauses 3.4 and 3.5 hold a violating fixture with
`outcome = "pass"`: a conforming implementation may warn, and must not fail.

## Vocabulary

Fixtures use kind names foreign to any particular workspace, and at least one
maps a familiar word to an unexpected class, so an implementation cannot pass by
privileging names (clause 2.5).
