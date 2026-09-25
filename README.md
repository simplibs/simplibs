# 🧩 `simplibs`

[![PyPI](https://img.shields.io/pypi/v/simplibs)](https://pypi.org/project/simplibs/)
[![Python](https://img.shields.io/badge/python-3.11%2B-blue)](https://www.python.org/downloads/)
[![Licence](https://img.shields.io/badge/licence-MIT-green)](https://github.com/simplibs/simplibs/blob/main/LICENSE)

**The root of the `simplibs` ecosystem — a namespace, not a library.**

`simplibs` carries no logic of its own. It exists purely to anchor the `simplibs.*`
namespace that every library in this ecosystem shares, so that `simplibs.exception`,
`simplibs.rules`, `simplibs.validate`, `simplibs.types`, and `simplibs.actions` all
live under one coherent root instead of colliding, unrelated top-level packages. If
you're looking for something to `pip install` and *use*, you want one of the libraries
below — this page is a map of how they fit together.

---

## 🧭 The Core Philosophy

`simplibs` (Simple Libraries) is a family of small, single-purpose Python libraries
that build on each other in layers. Rather than one large library where everything is
entangled, each `simplibs` package solves exactly one problem well, and the next
package up reuses it rather than reinventing it. Every layer shares the same
engineering philosophy — described in full at the bottom of this page — and, just as
importantly, every layer is usable entirely on its own: needing `simplibs-rules`
doesn't mean adopting the whole ecosystem.

---

## 🏛️ The Layers

### 1. [`simplibs-exception`](https://pypi.org/project/simplibs-exception/) — the shared diagnostic language

The foundation everything else is built on. A standard Python exception tells you
*where* code broke, but leaves you guessing *what* was actually wrong and *how* to fix
it. `simplibs-exception` turns a crash into a structured diagnostic card instead — a
clear `expected` vs. `value`, a `problem` statement, and concrete `how_to_fix` steps —
plus a ready-made family of exception types (`ParamError`, `ValidationError`, ...)
every other `simplibs` library raises instead of inventing its own.

### 2. [`simplibs-rules`](https://pypi.org/project/simplibs-rules/) — the shared language for defining conditions

Built on `exception`. A `Rule` answers exactly one question — *"does this value
satisfy me?"* — and nothing else; it never transforms or coerces a value. Rules
compose with plain Python operators (`is_integer & greater_than(0)`, `is_string |
is_none`, `~is_blank`), and a failing rule produces the same structured diagnostic
card `exception` defines. Because rules can also describe whole typing constructs
(`list[int]`, `dict[str, int] | None`, ...), they serve two purposes at once:
validating values, and — one layer up — defining types.

### 3. [`simplibs-validate`](https://pypi.org/project/simplibs-validate/) — automating validation

Built on `rules`. Where `rules` gives you the building blocks, `validate` removes the
need to call them by hand everywhere. Its central tool, `@validate_call`, reads a
function's own type annotations and validates every call against them automatically —
no validation code inside the function body at all. Alongside it: `validate_dataclass`
(the same idea for dataclasses) and `log_this` (automatic entry/exit/timing/exception
logging, with no logging code of your own).

### 4. [`simplibs-types`](https://pypi.org/project/simplibs-types/) — reusable, named types

Built on `rules`. The last piece: naming a type constraint once instead of repeating
it everywhere (`PositiveInt` instead of `Annotated[int, greater_than(0)]` at every
call site). Built directly on plain `Annotated` definitions — parameter-free types get
full IDE/autocomplete support, and the library carries no dependency on `validate`.

### 5. [`simplibs-actions`](https://pypi.org/project/simplibs-actions/) — composable pipeline steps

Built on `rules`, `validate`, `exception`, and `types` together — the newest, and
currently topmost, layer. Where the layers below answer "is this value valid", `actions`
answers a different question: *"run this step over a value, and hand the result to the
next step in line."* An `Action` is exactly that — one step, wrapped into an object —
and actions compose with the same operator-based style as rules: `step_a >> step_b`
(sequence), `step_a & step_b` (parallel), `step_a | step_b` (fallback). Any ordinary
function becomes a fully validated, logged, diagnostics-ready `Action` with a single
decorator (`@to_action`). This layer is both an endpoint (nothing more abstract than
"run a step, get a result" is needed) and a foundation — further `simplibs` libraries
build their own domain-specific actions directly on top of it.

---

## 🛠️ How the Layers Fit Together

```
simplibs-exception   ◄── structured diagnostics — the shared language every layer raises errors in
        ▲
simplibs-rules        ◄── composable conditions (Rule), built on exception
        ▲
   ┌────┴────┐
   │         │
simplibs-   simplibs-
validate     types      ◄── automatic validation, and named types — both built on rules
   │         │
   └────┬────┘
        ▲
simplibs-actions      ◄── composable pipeline steps (Action), built on all of the above
```

Each arrow means "built on", not "requires the whole stack" — every library above is
independently installable and useful on its own.

---

## 🔗 The Libraries

| Library | Layer | What it gives you |
|---|---|---|
| [`simplibs-exception`](https://pypi.org/project/simplibs-exception/) | 1 | Structured, diagnostic exceptions instead of bare tracebacks. |
| [`simplibs-rules`](https://pypi.org/project/simplibs-rules/) | 2 | Composable `Rule` predicates (`&`, `\|`, `~`) with structured diagnostics on failure. |
| [`simplibs-validate`](https://pypi.org/project/simplibs-validate/) | 3 | `@validate_call`/`@validate_dataclass`/`@log_this` — automatic validation and logging from type annotations. |
| [`simplibs-types`](https://pypi.org/project/simplibs-types/) | 4 | Reusable, named validated types, built on plain `Annotated`. |
| [`simplibs-actions`](https://pypi.org/project/simplibs-actions/) | 5 | Composable `Action` pipeline steps (`>>`, `&`, `\|`), built on everything above. |

---

## ☯️ About simplibs

All libraries in the **simplibs** (Simple Libraries) ecosystem share a common engineering philosophy:

* **Dyslexia-friendly:** 
We actively minimize cognitive load. Code is atomized into small, self-contained units, 
files are named directly after the logical task they perform, 
and explanations describe *why* something is designed, not just *what* it is.
* **Programmer's Zen:** 
Nothing should be missing, and nothing should be superfluous. 
We value clean execution paths and robust, understandable code architectures over rushed, messy feature sets.
* **Defensive Style:** 
We actively anticipate edge cases and failure modes so that only safe operational paths remain. 
Our code is built to degrade gracefully rather than crash unexpectedly.
* **Minimalism:** 
Find the most direct path to the goal in as few operational steps as possible 
without taking shortcuts on safety, readability, or completeness.
* **Code as Craft:** 
Code should be pleasant to look at, readable at a glance, and evoke structural harmony. 
We treat software engineering as a precision trade.

---

### 🤝 Contributing & Community

This is an **open-source project** built with love and care. 
We strongly believe in community collaboration and welcome any feedback, bug reports, or feature ideas!

* **Want to contribute?** Feel free to open an Issue or submit a Pull Request.
* **Want to get in touch?** If you'd like to discuss the project further, collaborate,
or just say hello, feel free to open a GitHub Issue or start a Discussion.

---

### 📝 License

This library is released under the **MIT License**. Build great things!

---

[▲ Back to Top](#-simplibs)