---
name: dependency-reuse-check
description: Use when about to add a new dependency, library, or package to a project's dependency manifest (build.gradle, package.json, pom.xml, requirements.txt, go.mod, Cargo.toml, Gemfile, composer.json, etc.), or when a request implies pulling in an external library to gain a capability — before writing the dependency line. Portable across languages and stacks.
---

# dependency-reuse-check

## Overview

A new dependency is a permanent cost: supply-chain risk, version conflicts, license obligations, and maintenance that outlives the feature that introduced it. **Default to NO.** Adding a library is the last resort proven necessary — not the first recommendation.

The failure this prevents: leading with "add library X" and treating "could existing code already do it?" as a footnote.

## The Rule

**Do not propose, write, or recommend a new dependency line until you have proven that existing dependencies and the standard library cannot do the job — and then asked the user before adding it.**

Reaching for the idiomatic library because it's "the natural fit on this stack" is not proof of need. The actual requirement decides what's needed, not hypothetical edge cases.

## Procedure (in order — do not skip ahead)

1. **State the capability in one sentence.** e.g. "Parse a 4-column CSV upload." Scope it to what the task actually requires, not what a library could theoretically handle.

2. **Read the manifest. Scan existing deps — direct AND transitive.** Open the actual manifest file; do not recall it from memory. Can a dependency already present do this? Frameworks bundle a lot (HTTP client, JSON, validation, retry, scheduling). Check what arrives transitively too.

3. **Check the standard library.** Can the language's stdlib do it directly? Most "small utility" libraries replace a handful of lines of stdlib code you'd own outright.

4. **State the verdict explicitly:**
   - **Covered** by an existing dep or the stdlib → use it. Add nothing. Show the implementation with what's already there.
   - **Not covered** → continue to step 5.

5. **Justify, then ask.** State concretely why the existing/stdlib options fall short for *this* requirement (not a hypothetical one). Then ASK the user before adding — present it as a decision ("X is needed because Y; add it?"), not as a recommendation you've already acted on. Do not edit the manifest until approved.

6. **On approval — pin and vet.** Pin an explicit version. Note the license and whether it's actively maintained. Prefer a library from a family already in the tree over a brand-new vendor.

## Red Flags — STOP

- Writing the dependency line before stating the step-4 verdict
- "It's the natural fit since we're already on this stack"
- Justifying the add with edge cases the requirement doesn't include ("fields *might* contain commas")
- Recommending a library recalled from memory without reading the manifest
- The "no-add" path appears only as a footnote beneath the library recommendation

All of these mean: go back to step 1.

## Rationalization Table

| Excuse | Reality |
|--------|---------|
| "It's the idiomatic library for this stack" | Idiomatic ≠ needed. The requirement decides. |
| "It handles edge cases stdlib gets wrong" | Only relevant if the requirement HAS those edges. Scope to the actual task. |
| "It's tiny / zero transitive bloat" | Every dep is permanent surface. Size isn't the cost; existence is. |
| "Adding it is faster than writing the code" | A few lines of stdlib you own beats a dependency you must track forever. |
| "It's from the same family as an existing dep" | That lowers the cost, not the need to ask. Still a new line to maintain. |

## Common Mistakes

- **Eyeballing instead of reading** — recommending a library/version without opening the manifest to see what's already resolved.
- **Skipping the verdict** — sliding from "here's the library" into a footnote disclaimer instead of a clear "covered / not covered" call.
- **Asking after editing** — adding the line, then mentioning "let me know if you'd rather not." Ask first; edit after approval.
