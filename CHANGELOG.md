# Changelog

All notable changes to Music Duplicate Finder are documented here.

---

## [3.2] - 2026-03-04

### Added
- **Fingerprint confidence tiers** — fingerprint matches are now reviewed in three separate Notepad lists grouped by match score: High (95–100%), Medium (90–94%), and Low (85–89%). Allows batch-approving confident matches while focusing scrutiny on lower-confidence ones.
- **`--clear-fp-cache` CLI flag** — clears the fingerprint cache (`music_fp_cache.json`) without needing to delete the file manually. Can be combined with other flags, e.g. `--clear-fp-cache --dry-run`.
- **Degenerate fingerprint detection** — fingerprints with fewer than 50 integers are now rejected before being added to the library index. A warning is printed at startup for any skipped files (e.g. `! 01 - I'm a Slave 4 U.mp3 — short fingerprint (3 integers) — file may be corrupted or silent`).
- **High-frequency match warning** — if any single organized library file matches 5 or more unsorted files during the fingerprint pass, it is flagged in the output as a likely bad fingerprint source.
- **Fingerprint resume tracking** — `fp_processed` is now saved in `music_resume.json` alongside `processed`, so an interrupted fingerprint pass can resume without re-fingerprinting files already completed.

### Fixed
- Syntax error (unterminated f-string) in the high-frequency match warning print statement.

---

## [3.1] - 2026-03-01

### Added
- **AcoustID audio fingerprint matching** (optional) — a second pass on No Match files using Chromaprint (`fpcalc`). Compares actual audio content to catch renamed or retagged duplicates that filename/metadata matching cannot find. No API key required — all comparisons are local.
- **Fingerprint caching** — fingerprints stored in `music_fp_cache.json`. First run is slow; subsequent runs are fast.
- **`acoustid` config section** — `enabled` (default false), `fpcalc_path`, `similarity_threshold` (default 85%), `fp_cache_file`.
- **"Why not exact" explanations** — Standard Duplicates now show a human-readable reason why they didn't qualify for auto-move (e.g. `size diff: 2.5% (tolerance: 3.0%)`). Shown in Notepad review list, log output, and CSV report.
- **`Why Not Exact` and `Match Method` CSV columns** — every row in the report now records how the match was found and why it wasn't auto-moved.

### Changed
- **Exact match size tolerance increased from 0.5% to 3.0%** — files at the same bitrate frequently differ slightly in size due to embedded artwork and tag overhead. The previous 0.5% threshold was causing 9 valid exact matches to fall through to Standard Duplicates requiring manual review.
- **Quality detection changed to bitrate-only** — `unsorted_is_better()` previously used file size as a proxy for quality. Changed to compare bitrate only, since size differences at the same bitrate are tag/artwork noise rather than audio quality differences.

### Fixed
- **Mode 4 cross-file false positives** — `find_match()` now verifies that a filename match and a metadata match refer to the same organized file. Previously, a filename match against one file and a metadata match against a different file would both satisfy Mode 4 ("both must match"), causing incorrect duplicate detection.

---

## [3.0] - 2026-02-27

### Added
- **Multi-threaded scanning** — organized folder scanned in parallel using configurable thread count (`max_threads` in config, default auto-detect).
- **Metadata cache** — organized folder scan results saved to `music_cache.json`. Subsequent runs skip unchanged files for significantly faster startup.
- **Fuzzy metadata matching** — uses `rapidfuzz` to catch tag inconsistencies (e.g. `"The All-American Rejects"` vs `"All-American Rejects"`). Configurable threshold (default 88%).
- **Duration matching** — track length included in match criteria to reduce false positives from songs sharing a title. Configurable tolerance (default ±2s).
- **Four match modes** — Filename only, Metadata only, Either, or Both (recommended). Mode 4 requires both filename and metadata to match on the same file.
- **Resume capability** — interrupted runs save state to `music_resume.json` and offer to continue from where they left off.
- **Progress bars** — real-time progress display during scanning, matching, and fingerprinting using `tqdm`.
- **JSON config file** — all settings in `music_config.json` (folder paths, matching options, performance, resume, output).
- **CSV report** — full log of every file processed, every decision made, and every action taken. One file per run, timestamped.
- **Dry run mode** — `--dry-run` flag previews all actions without moving any files.
- **`--clear-cache`** — forces a full re-scan of the organized folder.
- **`--clear-resume`** — discards any saved interrupted run state.
- **`--config`** — specify an alternative config file.
- **`undo_duplicates.py`** — companion script that reads any previous CSV report and restores files to their original locations. Supports `--dry-run` and `--filter-category`.

### Changed
- Complete rewrite from v1/v2 single-pass script.

---

## [2.0] - 2026-02-27

### Added
- `undo_duplicates.py` — basic restore script to reverse a previous run using the log file.
- Improved log output with per-file detail.

---

## [1.0] - 2026-02-27

### Added
- Initial script — single-threaded scan of unsorted folder against organized library.
- Filename-based matching.
- Manual file moving with Notepad review list.
- Basic log output.
