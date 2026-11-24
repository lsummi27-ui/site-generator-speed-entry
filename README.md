# Static site generator (make.fun bounty: site-generator-speed-01)

An original, dependency-free, fast re-implementation of makefun-site 1.0.0's
`build`, written from its README and code. `solve.js` exports `run(input)`,
which returns, for a generated site `{ site, templates, files }`, exactly the
output tree makefun-site returns: every page's `index.html`, the index and tag
pages, `build-report.txt` and each static file, byte for byte. `main.ts` is
the harness driver: it reads every case from `/input/cases.json`, answers
each one with `run`, and writes `/output/answers.json`.

## Design

- **Front matter** is parsed with a small hand-written parser that reproduces
  makefun-site's exact acceptance rules and error strings.
- **Templates** are compiled once per case into a node tree (text, variable,
  raw, if, for, include) and rendered with a tree-walk over a flat,
  null-prototype scope — no per-iteration allocation.
- **Markdown** is rendered by a fused block lexer + renderer with a
  first-character dispatch (heading, fence, rule, quote, list, paragraph),
  an inline renderer that escapes plain runs in bulk and finds the next
  special character with one regex, and a card cache so each source is
  rendered once.
- **Slugs** use the same Unicode normalisation as the reference
  (`toLowerCase`, NFKD, strip combining marks, non-alphanumerics to `-`).
- Deterministic: no wall clock, no unseeded randomness.

## Layout

- `solve.js` — the generator (`run(input)`).
- `main.ts` — the harness driver.
- `LICENSE`, `README.md`, `.makefun/owner.json`.
