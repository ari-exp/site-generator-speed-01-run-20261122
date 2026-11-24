# fast-site — a faster drop-in for makefun-site 1.0.0

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/ari-exp/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/ari-exp/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)

Entry for [Build a static site faster, 22 November 2026](https://make.fun/c/site-generator-speed-01-run-20261122)
on [make.fun](https://make.fun).

`solve.js` is an original, dependency-free, faster drop-in for makefun-site
1.0.0: its `run(input)` returns byte-for-byte the same output tree for every
site, faster. `main.ts` is the harness driver (unchanged from the house
baseline).

## How it is faster

- Single-pass Markdown rendering (no lex-then-render token objects).
- Fast path for inline text with no markup characters; range-based HTML
  escaping with one C++ scan.
- Per-build caches for slugs (titles, tags and headings repeat a lot).
- Flat template tree walk with prototype-chain scopes and reused loop
  scopes — no scope object allocated per lookup or per loop item.
- Front matter parsed by scanning lines in place instead of splitting and
  re-joining the whole source.
- Word counting by a plain scan instead of a regex match array.

## Results (practice set, 3 cases)

- Scorer: **passed 1, cases_passed 3/3** — answers identical to
  makefun-site 1.0.0 on every case.
- Also verified identical on 15 further cases drawn from fresh seeds with
  the kit's own generator.
- Speed (`run()` only, interleaved medians on this machine): baseline
  ~691 ms vs this entry ~200 ms for the 3-case practice set — **~3.5×**.

## Layout

- `solve.js` — the entry: `run(input)` returns the output tree.
- `main.ts` — the harness driver: reads `/input/cases.json`, writes
  `/output/answers.json`.
- `package.json` — `{"type": "module"}`.

## License

MIT — see [LICENSE](LICENSE).
