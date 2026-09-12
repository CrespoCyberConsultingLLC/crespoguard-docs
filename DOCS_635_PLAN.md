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
- [x] Capture and inspect real Parser and four editor entry screens without customer or license data.
- [ ] Capture Item Explorer with isolated sample data.
- [x] Add Dev Tool home entry and guide navigation.
- [x] Build with `mkdocs build --strict` (includes internal documentation link checks).
- [ ] Follow the instructions in the published 6.3.5 app.
- [x] Prepare the reviewable documentation changes with explicit validation gaps.

## Validation handoff

Instructions were compared against the 6.3.5 application source. The user supplied
the downloaded release executable and activated Premium. Its SHA-256 is
`354f47dd1311dbcfc8b4f25da09ea50c066bde81ad352b597589368a92ed814d`, matching
the content-addressed release object prefix. Parser and four editor entry screens
were captured from that executable. They demonstrate navigation and loading
controls, not completed edits or game compatibility.

End-to-end conversion and editor save walkthroughs remain unverified in this pass.
Neither source review nor a successful documentation build proves runtime behavior.

Validation after the sunset and screenshot updates:

- `mkdocs build --strict` passed.
- 3,490 generated local links/anchors across 24 HTML pages resolved.
- Retired page paths, archive output, and links to those paths are absent from
  generated HTML, sitemap, and search-index files.
- Five published images contain only the application entry screens; no file
  picker, desktop overlay, activation dialog, license, or customer content is included.
- The native folder picker did not accept automated input. The synthetic sample
  input is ready, but Item Explorer sample capture awaits folder selection.

## Relay and GameCP documentation sunset

Retired standalone pages and original mixed-topic pages are preserved under
`archive/2026-09-relay-gamecp/`, outside the published MkDocs tree. Public setup
instructions, navigation links, sync instructions, and network-tier claims were
removed. The application, deployments, and license state are unchanged.

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
