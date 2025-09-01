# langcodec Roadmap

This document outlines progressive, bite‑sized tasks to enhance langcodec and langcodec‑cli. It’s structured so we can pick items incrementally and track progress over time.

Legend: [ ] todo, [x] done, [~] in progress

## Recently Completed

- [x] Android `<plurals>` parse/write support (library)
- [x] `.strings` writer escaping (quotes, backslashes, control chars)
- [x] Symmetric language matching for multi‑language formats (`xcstrings`, `csv`, `tsv`)
- [x] CLI view prints “Type: Plural” and plural categories
- [x] Conversion tests: CSV→Android, XCStrings→Android (with plurals)

---

## M1. Quality & Safety

- [ ] Placeholder normalization and validation
  - [ ] Mapping between iOS (`%1$@`, `%d`) and Android (`%1$s`, `%d`)
  - [ ] Detect placeholder mismatches across languages; fail in strict mode, warn otherwise
  - [ ] Auto‑fix option for common cases (`%@` → `%s`, `%1$@` → `%1$s`)
  - [ ] Tests across `.strings`, Android, `.xcstrings`
- [ ] Plural rules engine
  - [ ] CLDR‑driven required category sets per locale (few/many/etc.)
  - [ ] Validation pass: flag missing categories per key+locale
  - [ ] CLI: `view --check-plurals` and `validate` output
- [ ] Strict vs. permissive parsing
  - [ ] Global setting in lib; CLI `--strict` flag
  - [ ] Consistent error surfaces with actionable context
- [ ] Better error context
  - [ ] Include file path and entry id for parse/convert errors
  - [ ] (Optional) capture line/column when parser knows it

## M2. Formats

- [ ] Apple `.stringsdict` (plurals/select)
- [ ] Flutter `.arb`
- [ ] Gettext `.po`
- [ ] XLIFF 1.2 / 2.0
- [ ] (Later) ICU MessageFormat v2 (exploration)

For each new format:

- [ ] Implement `Parser` and conversions to/from `Resource`
- [ ] Round‑trip tests + cross‑conversion tests
- [ ] CLI convert + view coverage
- [ ] README updates

## M3. CSV/TSV Schema

- [ ] Optional extended columns: `comment`, `status`, `context`, `developer_note`
- [ ] CLI: `--schema` flag (e.g., `basic`, `extended`, custom mapping)
- [ ] Lossless round‑trip for supported metadata
- [ ] Tests to ensure consistent ordering and schema stability

## M4. CLI UX

- [ ] `diff` subcommand
  - [ ] Compare two files; output added/removed/changed keys by language
  - [ ] Machine‑readable JSON output and pretty mode
- [ ] `stats` subcommand
  - [ ] Per‑language counts by `EntryStatus`, completion %, missing plurals
- [ ] `normalize` subcommand
  - [ ] Canonicalize whitespace, escapes, key casing; optional rules
- [ ] Filters and export
  - [ ] `view --where 'status=stale and lang in(en,fr)' --format csv`
  - [ ] `--grep` for key/value regex
- [ ] Stdio support: `-` for stdin/stdout across commands
- [ ] Config file: `langcodec.toml` for project defaults (langs, merge strategy, schema, placeholder policy)

## M5. Developer Experience

- [ ] API ergonomics
  - [ ] Borrowed iterators and helpers: `iter_keys()`, `iter_entries(lang)`
  - [ ] Mutators: `rename_key`, `bulk_rename`, `map_values`
- [ ] Deterministic ordering everywhere (keys, languages)
- [ ] Provenance tracking (source file, optional line) per entry
- [ ] Benchmarks (Criterion) for parse/convert/merge

## M6. Ecosystem & Distribution

- [ ] WASM target (browser/Node) for view/convert/diff in web tools
- [ ] GitHub Action templates
  - [ ] Validate PRs, enforce placeholder policy, fail on regressions
  - [ ] Example workflows in `.github/workflows/examples/`
- [ ] Documentation site
  - [ ] Task‑oriented guides (convert recipes, plural pitfalls, placeholder mapping)
  - [ ] API docs deep links; examples gallery

## Testing Strategy

- [ ] Start with unit tests near each format parser/writer
- [ ] Add conversion matrix tests for common paths (strings↔android↔xcstrings↔csv/tsv)
- [ ] Property tests where feasible (e.g., round‑trip invariants)
- [ ] Large sample corpora in `tests/data/` for regression

## Contribution Guide Enhancements

- [ ] Add coding standards and commit message conventions
- [ ] Issue templates for formats vs CLI vs core
- [ ] Local dev quickstart and common cargo commands

## Release Checklist (per minor)

- [ ] Update README Supported Formats table
- [ ] Changelog highlights (breaking changes, new formats, CLI flags)
- [ ] Version bumps in workspace `Cargo.toml` and README
- [ ] Tag + GitHub release notes

---

If you pick up an item, feel free to mark it with [~] and open a PR referencing this roadmap.
