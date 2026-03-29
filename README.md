# simplibs

This package serves as the **namespace root** for the `simplibs` (simple libraries) ecosystem.

It does not contain any code of its own. Its purpose is to reserve the
`simplibs` namespace on PyPI, so that all ecosystem packages can be
imported under a unified `simplibs.*` prefix.

## Ecosystem packages

| Package | Description | PyPI |
|---------|-------------|------|
| [simplibs-sentinels](https://github.com/simplibs/simplibs-sentinels) | Shared sentinel values (UNSET, MISSING, DEFAULT, EMPTY) | [![PyPI](https://img.shields.io/pypi/v/simplibs-sentinels)](https://pypi.org/project/simplibs-sentinels/) |
| simplibs-exception | Structured exception with diagnostics *(coming soon)* | — |
| simplibs-signature | Function signature introspection *(coming soon)* | — |

## Installation

Install individual packages based on what you need:
```bash
pip install simplibs-sentinels
pip install simplibs-exception
pip install simplibs-signature
```

Then import them together under one roof:
```python
from simplibs.sentinels import UNSET, UnsetType
from simplibs.exception import SimpleException
from simplibs.signature import SignatureCreator
```

## About the simplibs ecosystem

The **simplibs** ecosystem is a collection of small, self-contained Python
libraries. Each one solves exactly one thing — but all of them share a common
philosophy:

**Dyslexia-friendly** — minimise mental load. Atomise code into self-contained
units, name files after the logic they contain, write explanations that describe
*why* — not just *what*.

**Programmer's zen** — nothing should be missing and nothing should be
superfluous. The journey is the destination: code should be fully understood;
better to go slowly and correctly than quickly and with mistakes. The
crystallisation approach — not perfection on the first try, but gradual
refinement towards it.

**Defensive style** — anticipate all possible failure modes so that only safe
paths remain. Never raise unexpected errors; degrade gracefully.

**Minimalism** — find the path to the goal in as few steps as possible, but
leave nothing out. Each file has one responsibility.

**Code as craft** — code should be pleasant to look at and evoke a sense of
harmony. Treat code as a small work of art — like a carpenter carving a
sculpture. Optimise for the user: everything should make sense without having
to study the documentation at length.

These are aspirations — a sense of direction. And that is exactly what the
note about the journey becoming the destination is all about. 🙂

---

*Individual packages are covered by tests across all modules — unit tests and
integration tests alike. Tests are part of each repository and serve as living
documentation of the expected behaviour.*