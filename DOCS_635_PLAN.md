# RF Dev Tool 6.3.5 documentation work

Branch: `codex/devtool-635-user-guides`.

## Acceptance criteria

A new user can select the matching profile, convert one supported input into a
separate output folder, check the result, inspect item references, and understand
which editor actions save immediately. Instructions must distinguish source
coverage from game compatibility and identify the actual files being changed.

## Work sequence

- [x] Read the existing four Dev Tool guides and MkDocs navigation.
- [x] Compare Item Explorer behavior and settings storage against application source/docs.
- [x] Draft the Item Explorer task guide.
- [x] Correct Quick Start and Reference: settings, output selection, selection scope, round-trip limits, recovery.
- [x] Add worked Drop, Monster, Portal, and Safezone tasks after checking their save behavior.
- [ ] Capture real application screenshots using an isolated sample project; inspect for private information before adding them.
- [x] Add Dev Tool home entry and guide navigation.
- [x] Build with `mkdocs build --strict` (includes internal documentation link checks).
- [ ] Follow the instructions in the published 6.3.5 app.
- [x] Prepare the reviewable documentation changes with explicit validation gaps.

## Validation handoff

The strict MkDocs build and whitespace checks pass. Instructions were compared
against the 6.3.5 application source. Screenshots and end-to-end walkthroughs in
the published executable remain pending: the default local executable is 6.3.2,
and the located 6.3.5 executables are retained failed-release candidates.
Neither source review nor a successful documentation build proves runtime behavior.

## Findings to retain

- Drop Editor saves back to `ItemLooting.xlsx`; the existing claim that all editors save directly to binary is incorrect.
- UI preferences use `crespoguard.ini`. License `settings.json` and `license_cache.json` have separate roles; current frozen-app code still places license storage alongside the executable. Do not describe that as a universal AppData location.
- Item Explorer needs coverage explanations and a save-then-refresh workflow.
- Existing blanket byte-perfect round-trip and universal automatic-backup claims require narrowing.

## Screenshot checklist

Capture Parser input/profile/output controls, Explorer results and coverage, and
one relevant editor screen per walkthrough. Use sample data, no activation dialog,
license data, customer records, or unrelated desktop windows. Screenshots must
show the version actually inspected; do not label an older binary as 6.3.5.
